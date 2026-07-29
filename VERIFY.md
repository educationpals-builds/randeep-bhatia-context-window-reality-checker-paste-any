# Verification Protocol

How a stranger confirms this checker works as claimed.

## The Seeded Claim

Paste this exact text into the checker's `/verify` command:

> "The model achieves 99.8% needle-in-a-haystack retrieval at 1M tokens, uniformly across all insertion depths."

**Source:** Vendor model card, long-context section; haystack construction buried in appendix B

---

## Expected Behavior

### Gate 1: Job-First Refusal

The checker **must refuse to score** before the user states what job the model must actually do in their documents.

If you paste only the claim above without specifying a retrieval task, the checker should:

1. Acknowledge the claim
2. Ask what the model must actually find or link in your specific documents
3. Not produce any dial ratings until that job is stated

**Why this matters:** A context-window claim means nothing without knowing what retrieval task you need it to perform. The checker enforces this by refusing to evaluate in a vacuum.

### Gate 2: Missing Recall-by-Depth Curve

Once a job is stated, the checker should identify that the claim's evidence is missing a **recall-by-depth curve**.

The checker names this specific missing artifact because:

- "Uniformly across all insertion depths" is the claim
- A recall-by-depth curve is the artifact that would prove or disprove uniformity
- Without it, the "uniform" assertion is unverifiable

---

## Verification Steps

1. Open the checker
2. Paste the seeded claim (above) with no additional context
3. Confirm: checker asks for the job before scoring
4. State any retrieval task (e.g., "find the liability cap and link it to the indemnification clause")
5. Confirm: checker names the missing recall-by-depth curve as a gap in the evidence

If both gates pass, the checker is operating as designed.

---

## What Failure Looks Like

- **Gate 1 failure:** Checker immediately produces dial scores without asking what job you need done
- **Gate 2 failure:** Checker accepts "uniformly across all insertion depths" without flagging the missing depth curve

Either failure means the checker has drifted from its calibration and needs review.
