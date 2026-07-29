# Pass Gate

## Gate Definition

**Metric:** When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work

---

## Threshold & Re-run Cadence

The gate specification above defines when this checker passes certification. Re-run the probe board against this gate whenever the system prompt changes or a new model version is deployed.

---

## Contested-Call Rulings

### Atlas's Opposing Case

When dial scores fall in the 2–3 range, reasonable evaluators may disagree. Atlas (the adversarial reviewer) argued for different readings on the following probes:

| Probe | Builder's Call | Atlas's Call | Ruling |
|-------|---------------|--------------|--------|
| *See probe-board.md for specific contested probes* | — | — | Builder's calibration stands unless new evidence surfaces |

Atlas's position is preserved here so future reviewers can re-litigate if the gate fails unexpectedly.

---

## Weakest Dial Across Board

**Weakest signal identified:** what_the_context_holds

This dial consistently scores lowest because haystack composition details are rarely disclosed in vendor claims. The gate accounts for this structural weakness.

---

## How to Use This Gate

1. Run all 8 probes from `tests/probes.jsonl`
2. Compare results against expected behaviors in `tests/probe-board.md`
3. Apply the metric and threshold above
4. If gate fails, check contested-call rulings before re-calibrating
