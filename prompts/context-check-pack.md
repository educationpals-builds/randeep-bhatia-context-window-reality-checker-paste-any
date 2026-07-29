# Context-Check Prompt Pack

Five standalone prompts for evaluating long-context claims. Each targets one dial from the SIGHT framework. Paste any prompt into any chat model alongside a vendor claim to get a focused read on that dimension.

---

## Prompt 1: Where Attention Spreads

```
You are evaluating a long-context claim. Focus only on attention distribution.

Given this claim, answer:
1. Does the evaluation show where attention actually landed across the context window?
2. Is there evidence of uniform attention vs. attention decay at depth?
3. What would attention heatmaps or position-wise retrieval curves show if provided?

Claim to evaluate:
[PASTE CLAIM HERE]

Rate 0-4:
- 0: No attention data shown
- 1: Aggregate accuracy only, no positional breakdown
- 2: Some depth buckets shown but gaps remain
- 3: Full depth curve with minor methodology questions
- 4: Complete attention analysis with reproducible methodology

Output your rating and the single most important missing artifact.
```

---

## Prompt 2: How Relevance Was Measured

```
You are evaluating a long-context claim. Focus only on relevance measurement methodology.

Given this claim, answer:
1. How was "correct retrieval" defined and scored?
2. Was relevance binary (found/not found) or graded?
3. Did scoring check that the retrieved content was actually used in the answer?

Claim to evaluate:
[PASTE CLAIM HERE]

Rate 0-4:
- 0: No scoring methodology disclosed
- 1: Binary match only, no usage verification
- 2: Scoring defined but no downstream usage check
- 3: Relevance + usage measured with minor gaps
- 4: Full relevance pipeline with answer-grounding verification

Output your rating and the single most important missing artifact.
```

---

## Prompt 3: What Reaches the Answer

```
You are evaluating a long-context claim. Focus only on the retrieval-to-generation link.

Given this claim, answer:
1. Is there evidence the model's answer actually incorporated retrieved content?
2. Could the model have answered from parametric knowledge alone?
3. Was there a control condition (answer without the context) to isolate retrieval contribution?

Claim to evaluate:
[PASTE CLAIM HERE]

Rate 0-4:
- 0: No link shown between retrieval and answer
- 1: Retrieval claimed but answer sourcing unclear
- 2: Some examples of retrieval-grounded answers
- 3: Systematic check with minor gaps
- 4: Full attribution chain from retrieval to answer with controls

Output your rating and the single most important missing artifact.
```

---

## Prompt 4: What the Context Holds

```
You are evaluating a long-context claim. Focus only on haystack composition.

Given this claim, answer:
1. What documents or text comprised the context window?
2. Were there distractor passages designed to confuse retrieval?
3. How realistic is the haystack compared to production document sets?

Claim to evaluate:
[PASTE CLAIM HERE]

Rate 0-4:
- 0: Haystack composition not disclosed
- 1: Generic description only ("random text")
- 2: Document types named but no distractor analysis
- 3: Full composition with some distractor design
- 4: Production-realistic haystack with adversarial distractors documented

Output your rating and the single most important missing artifact.
```

---

## Prompt 5: What It Must Find

```
You are evaluating a long-context claim. Focus only on needle specification.

Given this claim, answer:
1. What exactly was the model asked to find?
2. Was the needle a simple fact or a complex cross-reference?
3. How does needle difficulty compare to your actual retrieval tasks?

Claim to evaluate:
[PASTE CLAIM HERE]

Rate 0-4:
- 0: Needle not specified
- 1: Simple fact retrieval only
- 2: Some complexity but not production-representative
- 3: Multi-hop or cross-reference tasks included
- 4: Needle complexity matches stated production use case with examples

Output your rating and the single most important missing artifact.
```

---

## Usage

1. Copy one prompt above
2. Replace `[PASTE CLAIM HERE]` with the vendor claim
3. Run in any chat model
4. Record the 0-4 rating and missing artifact
5. Repeat for all five dials
6. The lowest-scoring dial decides your read

## Calibration Reference

Builder's worked example claim:
> "The model achieves 99.8% needle-in-a-haystack retrieval at 1M tokens, uniformly across all insertion depths."

Builder's dial scores for this claim:
- where_attention_spreads: 3
- how_relevance_was_measured: 2
- what_reaches_the_answer: 2
- what_the_context_holds: 2
- what_it_must_find: 2

Weakest signal: what_the_context_holds

Use these scores to calibrate your own ratings against the builder's judgment.
