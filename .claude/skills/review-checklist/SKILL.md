---
name: review-checklist
description: Runs Jeremy's standard pre-flight review on a product brief or PRD before it goes any further. Use when the user says "review this brief", "run the review checklist", "check this PRD", or points at a brief or PRD file and asks for a review. Checks style, ownership, problem definition, constraints, success measures, scope consistency, problem-before-solution ordering, stakeholders, open questions, risks, dependencies, out-of-scope, evidence, and rollout, then reports pass/fail per item with evidence.
---

# Review checklist

Give the same review every time, so the user never has to re-explain what they look for. The target is a product brief or PRD. If the user didn't name one, ask which file; don't guess.

Read the whole document first. Then check each item below in order. Judge only what is on the page: if something isn't written down, it fails, even if it seems implied or you know the answer from elsewhere.

## The checks

1. **In my style.** Apply the `my-writing-style` skill (Jeremy's voice and preferred tone). Flag passages that read off-voice: padded, hedged, jargon-heavy, or generic. Quote the worst two or three and say what to change. If that skill isn't available, say so and mark this item "not checked" instead of inventing a style.
2. **Names who owns it.** There must be a named owner (a person, not a team) for the brief and, where relevant, for delivery. Fail if ownership is missing, shared across a group, or only a role with no name.
3. **Problem definition.** States the problem being solved: who has it, what happens, how often or how badly, and the evidence behind that. Fail if the problem is only implied by the solution, or has no evidence.
4. **Key constraints.** Constraints are written down, both explicit (deadlines, dependencies, policy, capacity, budget) and assumed. Where the brief leans on an assumption without saying so, list it as "unstated assumption" so it can be confirmed or struck.
5. **How we'll know it worked.** At least one measurable success criterion, with a baseline, a target, and a time frame. Fail if success is a feeling ("users are happier") or a metric with no target. Note if there's no trigger for rollback or a rethink.
6. **Scope matches end to end.** Compare the scope stated at the start (summary, goals, in/out of scope) against what the body, plan, phases, and risks at the end actually cover. List anything that appears at the end but not the start (scope creep), and anything promised at the start but never planned or measured.
7. **Problem before fix.** The problem is explained before any solution is proposed. Fail if a solution, feature name, or option appears before the problem section has been made clear, or if the problem section reads as a justification for a solution already chosen.

8. **Decision-makers and stakeholders.** Names who approves the work and who must be told or consulted. Fail if approvers are missing or only implied.
9. **Open questions have owners and dates.** Every open question has a named owner and a date to be answered. Fail on unowned or undated questions.
10. **Risks and rollback trigger.** Risks are listed with mitigations, and there is a stated trigger for stopping or reversing, plus who decides.
11. **Dependencies on other teams.** Work or decisions that rely on other teams (Engineering, Support, Data, Design) are named, with what is needed from each. Flag quiet reliance on another team that the brief never mentions.
12. **Out-of-scope list.** An explicit "not doing" section exists. Use it to sharpen check 6.
13. **Evidence vs hypothesis.** Claims are labelled as established (with source) or hypothesis. Flag causes asserted as fact without evidence.
14. **Rollout and comms plan.** Says who hears what, and when, including launch and any rollback.

## Output

Start with a one-line verdict: **Ready**, **Ready with fixes**, or **Not ready**. Then a table with one row per check: item, Pass / Partial / Fail, and the evidence (a short quote or section name). Below it, a prioritized list of fixes, most important first, each one specific enough to act on (what to add or change and where).

Don't rewrite the brief unless asked. Be plain about what is on the page versus your own inference, and don't soften a fail.

## After the review

If this brief seems to call for a check that is not in the list above, suggest it briefly (only if it would have changed the verdict) and ask whether the user wants it added permanently to this checklist.
