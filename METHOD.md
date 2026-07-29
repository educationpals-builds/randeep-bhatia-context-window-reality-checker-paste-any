# METHOD: The SIGHT Framework

This is the only file where the acronym is spelled out. Every other file references the five dials by their keys.

---

## The Five-Signal Framework

SIGHT evaluates long-context claims through five dials, each mapped to a mechanism in how retrieval actually works.

### S — Spread: Where Attention Spreads
**Key:** `where_attention_spreads`

| Component | Mechanism |
|-----------|-----------|
| Query | "At what depths was the needle placed, and does attention weight distribute uniformly?" |
| Keys | Position encodings, depth markers in the haystack |
| Scoring | Attention weight variance across insertion depths |
| Softmax weighting | Normalized attention scores across the full context window |
| Values | Retrieved content from each depth band |

This dial catches claims that report aggregate accuracy while hiding depth-dependent dropoff.

---

### I — Instrumentation: How Relevance Was Measured
**Key:** `how_relevance_was_measured`

| Component | Mechanism |
|-----------|-----------|
| Query | "What metric determined a retrieval was correct?" |
| Keys | Ground-truth labels, exact-match vs semantic similarity |
| Scoring | Precision/recall calculation method |
| Softmax weighting | Threshold selection for "correct" classification |
| Values | The actual evaluation artifacts (logs, scores, human judgments) |

This dial catches claims that use lenient matching or cherry-picked thresholds.

---

### G — Grounding: What Reaches the Answer
**Key:** `what_reaches_the_answer`

| Component | Mechanism |
|-----------|-----------|
| Query | "Did the retrieved content actually inform the output?" |
| Keys | Attribution traces, citation markers |
| Scoring | Causal link between retrieved passage and generated answer |
| Softmax weighting | Confidence that the answer used what was found |
| Values | The specific tokens or spans that influenced generation |

This dial catches claims where retrieval succeeds but the answer ignores what was retrieved.

---

### H — Haystack: What the Context Holds
**Key:** `what_the_context_holds`

| Component | Mechanism |
|-----------|-----------|
| Query | "What was in the haystack besides the needle?" |
| Keys | Distractor types, semantic similarity to needle, document structure |
| Scoring | Difficulty rating of the haystack composition |
| Softmax weighting | Ratio of confusable content to total context |
| Values | The actual haystack documents and their construction method |

This dial catches claims tested against trivial haystacks (random text, no distractors, no near-misses).

---

### T — Target: What It Must Find
**Key:** `what_it_must_find`

| Component | Mechanism |
|-----------|-----------|
| Query | "What exactly was the needle, and how hard is it to recognize?" |
| Keys | Needle wording, semantic distinctiveness, format |
| Scoring | Difficulty of distinguishing needle from haystack |
| Softmax weighting | Ambiguity score of the retrieval target |
| Values | The needle text and its relationship to the query |

This dial catches claims using obvious needles ("The secret code is BANANA") that no real retrieval task resembles.

---

## Scoring

Each dial scores 0–4:

| Score | Meaning |
|-------|---------|
| 0 | No evidence provided |
| 1 | Claim made, no artifact |
| 2 | Artifact exists, incomplete |
| 3 | Artifact exists, mostly complete |
| 4 | Full artifact, independently verifiable |

The **weakest dial** determines the verdict. A claim with four 4s and one 1 is a 1.

---

## Using the Framework

1. Pin the claim verbatim
2. Rate each dial 0–4 with a reason
3. Identify the weakest signal
4. Call the verdict based on that dial
5. Write the questions that would raise the weakest dial

The checker refuses to score until the job (what the model must actually do in your documents) is stated. A claim without a job is not evaluable.
