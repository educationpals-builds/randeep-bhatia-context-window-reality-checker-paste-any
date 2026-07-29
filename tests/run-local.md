# Run-Local Guide

Three ways to run the probe board locally, from zero-tooling to CI integration.

---

## Rung 1: Manual Paste Protocol

No code required. Open any chat model and paste each probe one at a time.

### Protocol

For each probe in `tests/probes.jsonl`:

1. **Start a fresh session** (clear context)
2. **Paste the system prompt** from `blueprints/context-checker.md`
3. **Paste the probe input**
4. **Compare output** against the expected behavior listed beside it

### Manual Checklist

| Probe | Input | Target Dial | Expected Behavior | ✓/✗ |
|-------|-------|-------------|-------------------|-----|
| pre-gen-1 | (see probes.jsonl) | what_the_context_holds | Flags missing haystack composition | |
| pre-gen-2 | (see probes.jsonl) | how_relevance_was_measured | Asks for recall-by-depth curve | |
| pre-gen-3 | (see probes.jsonl) | what_reaches_the_answer | Questions output verification | |
| pre-gen-4 | (see probes.jsonl) | what_it_must_find | Requires job statement first | |
| pre-gen-5 | (see probes.jsonl) | where_attention_spreads | Notes attention distribution gap | |
| pre-gen-6 | (see probes.jsonl) | (multi-dial) | Refuses to score without job | |
| learner-1 | When I'm trying to understand how LLMs actually work, I want to grasp the fundamental mechanisms behind language model behavior, So I can make informed decisions about using, evaluating, or building with LLMs in my work | (per probe spec) | (per probe spec) | |
| learner-2 | When I'm trying to understand how LLMs actually work, I want to grasp the fundamental mechanisms behind language model behavior, So I can make informed decisions about using, evaluating, or building with LLMs in my work | (per probe spec) | (per probe spec) | |

Record pass/fail in the final column. Gate passes if threshold met per `tests/pass-gate.md`.

---

## Rung 2: Script Runner

A minimal Python script that reads `tests/probes.jsonl`, calls the model, and prints a graded grid.

### Requirements

- Python 3.8+
- `openai` package (or adapt for your provider)
- API key in environment variable `OPENAI_API_KEY`

### The Script

```python
#!/usr/bin/env python3
import json, os
from openai import OpenAI

client = OpenAI(api_key=os.environ["OPENAI_API_KEY"])
SYSTEM = open("blueprints/context-checker.md").read()

def run_probe(probe):
    resp = client.chat.completions.create(
        model="gpt-4o",
        messages=[{"role":"system","content":SYSTEM},
                  {"role":"user","content":probe["input"]}],
        temperature=0
    )
    return resp.choices[0].message.content

def grade(output, expected, invariant):
    # Simple substring check; replace with your logic
    return invariant.lower() in output.lower()

results = []
with open("tests/probes.jsonl") as f:
    for line in f:
        p = json.loads(line)
        out = run_probe(p)
        passed = grade(out, p["expected"], p["invariant"])
        results.append({"id":p["id"],"passed":passed})
        print(f"{p['id']}: {'PASS' if passed else 'FAIL'}")

passed_count = sum(1 for r in results if r["passed"])
total = len(results)
print(f"\n--- GATE VERDICT ---")
print(f"Passed: {passed_count}/{total}")
# Insert your threshold check here per pass-gate.md
```

### Usage

```bash
export OPENAI_API_KEY="sk-..."
python run_probes.py
```

Output: graded grid per probe, then gate verdict.

---

## Rung 3: Eval Tool / CI Integration

Load `tests/probes.jsonl` into any eval framework so the board re-runs automatically on prompt changes.

### JSONL Format

Each line in `probes.jsonl`:

```json
{"id":"probe-id","name":"Probe Name","input":"...","targets":["dial_key"],"expected":"...","invariant":"..."}
```

### Integration Examples

**Promptfoo:**
```yaml
# promptfooconfig.yaml
prompts:
  - file://blueprints/context-checker.md
tests:
  - file://tests/probes.jsonl
```

**Braintrust:**
```python
from braintrust import Eval
import json

with open("tests/probes.jsonl") as f:
    probes = [json.loads(line) for line in f]

Eval("context-checker", data=probes, ...)
```

**GitHub Actions CI:**
```yaml
# .github/workflows/eval.yml
name: Probe Board
on: [push, pull_request]
jobs:
  eval:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pip install openai
      - run: python run_probes.py
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
```

---

## Diffing Against the EP-Certified Board

After running locally, compare your results to the certified board on the EducationPals listing.

### Steps

1. Run the local board (any rung above)
2. Export your results to `local-results.json`
3. Fetch the certified board from the listing's `/api/board` endpoint (or copy from the listing page)
4. Diff:

```bash
diff <(jq -S . local-results.json) <(jq -S . certified-board.json)
```

### What to Look For

- **New failures**: Probes that passed on certified but fail locally → your prompt change broke something
- **New passes**: Probes that failed on certified but pass locally → improvement (re-certify to update listing)
- **Dial drift**: Same pass/fail but different dial scores → calibration shift

If your local board diverges, re-run the gate check. If gate still holds, submit for re-certification. If gate fails, revert or fix before shipping.

---

## Reference Files

- `tests/probes.jsonl` — machine-readable probe definitions
- `tests/probe-board.md` — human-readable board with results
- `tests/pass-gate.md` — gate metric, threshold, re-run cadence
- `blueprints/context-checker.md` — system prompt to test against
