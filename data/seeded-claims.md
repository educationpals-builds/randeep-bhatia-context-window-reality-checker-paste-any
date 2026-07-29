# Seeded Claims — Calibration Record

This file contains the two seeded claims used to calibrate the context-window reality checker, the advisor's dial strips for each, and the builder's drift ruling.

---

## Seeded Claim 1: The Builder's Own Claim

**Claim text:**
> "The model achieves 99.8% needle-in-a-haystack retrieval at 1M tokens, uniformly across all insertion depths."

**Source:** Vendor model card, long-context section; haystack construction buried in appendix B

**Advisor's Dial Strip:**

| Dial | Advisor Score | Advisor Reasoning |
|------|---------------|-------------------|
| where_attention_spreads | 3 | Attention distribution not disclosed |
| how_relevance_was_measured | 2 | Measurement methodology unclear |
| what_reaches_the_answer | 2 | No verification of retrieval-to-answer path |
| what_the_context_holds | 2 | Haystack composition buried in appendix |
| what_it_must_find | 2 | Needle specification incomplete |

**Weakest dial:** what_the_context_holds

---

## Seeded Claim 2: Model Card Verification Claim

**Claim text:**
> "Our model maintains consistent performance across the full context window with no degradation at depth."

**Source:** Synthetic claim for verification testing

**Advisor's Dial Strip:**

| Dial | Advisor Score | Advisor Reasoning |
|------|---------------|-------------------|
| where_attention_spreads | 2 | No attention analysis provided |
| how_relevance_was_measured | 1 | "Consistent performance" undefined |
| what_reaches_the_answer | 2 | No output verification shown |
| what_the_context_holds | 1 | Context composition unspecified |
| what_it_must_find | 2 | Target retrieval task unstated |

**Weakest dial:** what_the_context_holds

---

## Builder's Drift Ruling

When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work

---

## Calibration Notes

The advisor and builder dial strips are compared to detect drift. When scores diverge by more than 1 point on any dial, the builder must rule on which read was correct and document the reasoning above.
