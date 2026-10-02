	# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis matches reproduction | The plan's diagnosis read against the quoted reproduction evidence and issue symptoms | The diagnosis identifies a specific likely cause that is consistent with the reproduced behavior and does not contradict the available evidence. | required |
| Scope is bounded | The plan's scope statement, files to change, exclusions, and repo/thread context | The plan identifies what will change and what will not change, and the proposed work stays limited to the reproduced issue rather than expanding into unrelated cleanup or features. | required |
| Approach is executable | The plan's implementation approach, named files or code areas, and repository facts | The plan describes concrete implementation steps specific enough that another developer could begin the change without having to invent the main approach. | required |
| Test plan proves the fix | The plan's test plan read against the reproduction steps and expected behavior | The test plan re-runs the reproduced failure through the real affected code and states an observable expected result that distinguishes the fixed behavior from the original bug. | required |
| Risks and unknowns are handled | The plan's risks, unknowns, assumptions, and any uncertainty in the diagnosis | Important uncertainty is stated explicitly and the plan explains how it will be checked during implementation rather than presenting guesses as confirmed facts. | required |
| Plan comment matches the plan and thread | The draft comment read against plan.md, the issue thread, and the repository's stated conventions | The comment accurately summarizes the diagnosis, scope, approach, and test plan, is consistent with the issue thread, and does not rely on another student's plan instead of the author's own reproduction. | required |

## Verdict rule

Return `accept` only when every required check passes.

Return `reject` if any required check fails.

Treat `unclear` as a failure for required checks, because a plan that does not provide enough evidence to verify a required part is not ready to post and build from.
