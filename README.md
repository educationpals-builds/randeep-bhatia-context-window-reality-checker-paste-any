# Context-Window Reality Checker

A tool for evaluating long-context claims before you bet your architecture on them.

## What This Is

This checker applies a five-dial framework to any long-context claim—vendor model cards, benchmark announcements, internal demos—and returns a structured read plus the questions you should send back before making decisions.

## The Worked Example

This checker was built by running the framework on a real claim:

**The claim (verbatim):**
> "The model achieves 99.8% needle-in-a-haystack retrieval at 1M tokens, uniformly across all insertion depths."

**Source:** Vendor model card, long-context section; haystack construction buried in appendix B

**What it decides:** Whether we delete our chunk-and-rerank stack before the next release ships

**Decision deadline:** Architecture review, next Wednesday

**The job it must actually do:**
> When I'm trying to understand how LLMs actually work,
>
> I want to grasp the fundamental mechanisms behind language model behavior,
>
> So I can make informed decisions about using, evaluating, or building with LLMs in my work

**Five-dial scores:**
- where_attention_spreads: 3
- how_relevance_was_measured: 2
- what_reaches_the_answer: 2
- what_the_context_holds: 2
- what_it_must_find: 2

**Weakest signal:** what_the_context_holds

**Verdict:**
> When I'm trying to understand how LLMs actually work,
>
> I want to grasp the fundamental mechanisms behind language model behavior,
>
> So I can make informed decisions about using, evaluating, or building with LLMs in my work

See [charter.md](charter.md) for the full run.

## Repository Structure

| Path | Purpose |
|------|---------|
| `charter.md` | The builder's complete run: claim, job, dials, verdict, questions |
| `blueprints/context-checker.md` | One-paste spec for the conversational checker |
| `prompts/context-check-pack.md` | 5 standalone prompts, one per dial |
| `METHOD.md` | The SIGHT framework explained (only file with the acronym) |
| `VERIFY.md` | How to verify this checker works |
| `skills/context-advisor.skill.md` | Portable skill file for assistant runtimes |
| `data/seeded-claims.md` | Calibration record with seeded claims |
| `tests/probe-board.md` | All 8 probes with results |
| `tests/pass-gate.md` | The gate this checker must hold |
| `tests/probes.jsonl` | Machine-readable probe export |
| `tests/run-local.md` | Run-anywhere guide |
| `STORY.md` | The builder's story for the marketplace card |

## One-Paste Rebuild

To rebuild this checker from scratch:

1. Start with a claim you need to evaluate
2. Pin the job: what must the model actually retrieve from your documents?
3. Run the five dials (see METHOD.md for the framework)
4. Call your verdict with the deciding dial and failure cost
5. Write the questions you'd send back to the claimant
6. Test against the probe board

The checker refuses to score any claim until the job is stated. This is by design—a context-window claim without a retrieval task is untestable.

## Verification

See [VERIFY.md](VERIFY.md) for the verification protocol. The short version: paste the seeded model-card claim and confirm the checker (1) refuses to score before the job is stated, and (2) names the missing recall-by-depth curve.

## License

This checker carries the builder's calibration. Use it, fork it, improve it.

<!-- educationpals-build-verified -->
