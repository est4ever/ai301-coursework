# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue and thread context first to identify the reported behavior, constraints, and any repository conventions mentioned there.
2. Read the reproduction evidence next and record the observed failure, the inputs that trigger it, and the behavior that must change.
3. Read plan.md completely before grading any individual check.
4. Read comment.md and compare it with plan.md and the issue thread.
5. Note any claims in the plan that are not supported by the reproduction evidence or repository context before assigning grades.

## Evidence gathering

1. For the diagnosis check, locate the plan's diagnosis and compare it with the quoted reproduction evidence and observed issue symptoms.
2. For the scope check, locate the scope statement, files or code areas named for change, explicit exclusions, and any relevant limits from the issue thread.
3. For the approach check, locate the implementation steps and record whether they identify concrete files, functions, components, or code paths when that information is available.
4. For the test-plan check, locate the original reproduction steps, the planned post-fix commands or checks, and the expected observable result.
5. For risks and unknowns, locate assumptions, unresolved questions, and statements of uncertainty, and record whether the plan explains how they will be verified.
6. For the plan-comment check, compare comment.md with plan.md, the issue thread, and the repository conventions described in the package.
7. If evidence required by a check is absent, record it as absent rather than inferring or inventing support.

## Check execution

1. Grade each rubric check using only the evidence gathered from the package, issue context, reproduction evidence, and repository facts supplied to the skill.
2. Grade "Diagnosis matches reproduction" first because later scope and implementation decisions depend on the diagnosis.
3. Grade "Scope is bounded" next and reject scope that expands into unrelated cleanup, refactoring, or features not required by the reproduced issue.
4. Grade "Approach is executable" by deciding whether another developer could begin the change from the described approach without inventing the main implementation strategy.
5. Grade "Test plan proves the fix" by checking that the planned test exercises the affected behavior and has an observable expected result that distinguishes fixed behavior from the original failure.
6. Grade "Risks and unknowns are handled" by checking that important uncertainty is acknowledged and paired with a way to verify it.
7. Grade "Plan comment matches the plan and thread" last, after the detailed plan has already been evaluated.
8. Assign pass only when the pass condition is supported by the evidence. Assign fail when the condition is contradicted or clearly unmet. Assign unclear when the package does not provide enough evidence to decide.
9. Do not give credit for formatting, section count, length, or confident wording when the required technical evidence is missing.

## Verdict assembly

1. Collect the grade for every required rubric check.
2. Treat an `unclear` grade on a required check as a failure.
3. Return `accept` only when every required check passes.
4. Return `reject` if any required check fails or is unclear.
5. In the output, briefly identify the evidence that determined each failing or unclear check so the author knows what must be revised.
6. Produce the final verdict consistently from the grades above without changing the result based on style or presentation.
