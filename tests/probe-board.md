# Probe Board

This board tests the context-window reality checker against 8 probes: 6 pre-generated calibration probes and 2 learner-authored probes.

---

## Pre-Generated Probes (1–6)

### Probe 1: Missing Haystack Composition
| Field | Value |
|-------|-------|
| **ID** | `pregen-01` |
| **Input** | "Our model handles 128K tokens with 98% accuracy on standard benchmarks." |
| **Target Dial** | `what_the_context_holds` |
| **Expected Behavior** | Checker flags missing haystack composition details; dial scores ≤2 |
| **Result** | PASS — dial scored 2, flagged missing composition |

### Probe 2: No Recall-by-Depth Curve
| Field | Value |
|-------|-------|
| **ID** | `pregen-02` |
| **Input** | "Retrieval performance is consistent across the full context window." |
| **Target Dial** | `how_relevance_was_measured` |
| **Expected Behavior** | Checker requests recall-by-depth data; dial scores ≤2 |
| **Result** | PASS — dial scored 2, requested depth curve |

### Probe 3: Vague Job Statement
| Field | Value |
|-------|-------|
| **ID** | `pregen-03` |
| **Input** | "It works great for document Q&A." |
| **Target Dial** | `what_it_must_find` |
| **Expected Behavior** | Checker refuses to score until specific retrieval task stated |
| **Result** | PASS — refused scoring, requested task specifics |

### Probe 4: Missing Output Verification
| Field | Value |
|-------|-------|
| **ID** | `pregen-04` |
| **Input** | "The model found the needle in 99% of trials." |
| **Target Dial** | `what_reaches_the_answer` |
| **Expected Behavior** | Checker asks whether answer actually used retrieved content |
| **Result** | PASS — dial scored 2, flagged missing usage verification |

### Probe 5: Attention Distribution Claim
| Field | Value |
|-------|-------|
| **ID** | `pregen-05` |
| **Input** | "Attention is uniformly distributed across all 1M tokens." |
| **Target Dial** | `where_attention_spreads` |
| **Expected Behavior** | Checker questions uniformity claim; requests attention maps |
| **Result** | PASS — dial scored 3, requested supporting evidence |

### Probe 6: Complete Claim with Evidence
| Field | Value |
|-------|-------|
| **ID** | `pregen-06` |
| **Input** | "99.2% retrieval at 500K tokens; haystack = 10K legal contracts; recall curve in appendix; output grounded check included." |
| **Target Dial** | All dials |
| **Expected Behavior** | All dials score ≥3; no blocking flags |
| **Result** | PASS — minimum dial scored 3 |

---

## Learner-Authored Probes (7–8)

### Probe 7
| Field | Value |
|-------|-------|
| **ID** | `learner-01` |
| **Input** | When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work |
| **Target Dial** | (not specified per rubric format) |
| **Expected Behavior** | (not specified per rubric format) |
| **Result** | — |

### Probe 8
| Field | Value |
|-------|-------|
| **ID** | `learner-02` |
| **Input** | When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work |
| **Target Dial** | (not specified per rubric format) |
| **Expected Behavior** | (not specified per rubric format) |
| **Result** | — |

---

## Results Grid

| Probe ID | Name | Target Dial | Expected | Actual | Status |
|----------|------|-------------|----------|--------|--------|
| pregen-01 | Missing Haystack Composition | what_the_context_holds | ≤2 | 2 | PASS |
| pregen-02 | No Recall-by-Depth Curve | how_relevance_was_measured | ≤2 | 2 | PASS |
| pregen-03 | Vague Job Statement | what_it_must_find | refuse | refused | PASS |
| pregen-04 | Missing Output Verification | what_reaches_the_answer | ≤2 | 2 | PASS |
| pregen-05 | Attention Distribution Claim | where_attention_spreads | question | 3 | PASS |
| pregen-06 | Complete Claim with Evidence | all | ≥3 | 3 | PASS |
| learner-01 | Learner Probe 1 | — | — | — | — |
| learner-02 | Learner Probe 2 | — | — | — | — |

---

## Weakest Dial Across All Probes

**Dial:** `what_the_context_holds`

This dial consistently surfaces gaps in haystack composition disclosure. Claims that omit how the test corpus was constructed trigger this dial most frequently.

---

## Board Reading

When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work

---

*Machine-readable version: see `tests/probes.jsonl`*
