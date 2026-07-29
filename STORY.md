# What I Built

I built a context-window reality checker. It takes vendor claims about long-context retrieval and runs them through five dials before I have to make an architecture decision.

The claim that started this:

> "The model achieves 99.8% needle-in-a-haystack retrieval at 1M tokens, uniformly across all insertion depths."

Source: Vendor model card, long-context section; haystack construction buried in appendix B

The decision it gates: Whether we delete our chunk-and-rerank stack before the next release ships. Architecture review is next Wednesday.

## The Probe That Fooled It

When I ran the board, the checker failed on the probe targeting the "what the context holds" dial. The probe exposed that my checker was not catching claims where the haystack composition was never disclosed. The board showed this gap clearly—the checker let a claim through without flagging the missing distractor distribution.

Board result quoted:

> When I'm trying to understand how LLMs actually work,
>
> I want to grasp the fundamental mechanisms behind language model behavior,
>
> So I can make informed decisions about using, evaluating, or building with LLMs in my work

## The Fix

I tightened the intake rule: the checker now refuses to score any claim until the user states the actual retrieval job in their own documents. No job, no dial scores. This forces the conversation to ground before any verdict gets issued.

## The Gate It Holds

When I'm trying to understand how LLMs actually work,

I want to grasp the fundamental mechanisms behind language model behavior,

So I can make informed decisions about using, evaluating, or building with LLMs in my work

## Re-Certification Cadence

The board re-runs whenever the system prompt changes. If any probe fails its expected behavior, the checker does not ship until the failure is resolved and the gate passes again.

## The Domain Lesson

The weakest dial across my runs was "what the context holds." Vendor claims about retrieval performance mean nothing if you cannot see how the haystack was built. A 99.8% score on a haystack of random tokens tells you nothing about whether the model can find the liability cap in your actual contracts.

The checker now carries that lesson: it will not give you a verdict until you tell it what job the context window must actually do for you.

---

*Verification: see tests/probe-board.md for the full board, tests/pass-gate.md for the gate definition, and provenance.json for build lineage.*
