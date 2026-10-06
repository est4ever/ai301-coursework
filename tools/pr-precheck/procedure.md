# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Determine whether the tool is running in live mode or eval mode.
2. Read the issue and repository requirements first to establish the requested change and the rules the PR must follow.
3. Read the plan, including its scope, boundaries, test plan, and recorded deviation notes.
4. Read the candidate diff and commit list and record what files and behaviors actually changed.
5. Read the test evidence and compare it with the plan's reproduction steps and test plan.
6. Read the PR title and description after examining the actual diff so that its claims can be checked against what was implemented.
7. Use references/evidence-guide.md to locate the evidence required by each check in rubric.md.

This order matters because the plan establishes the intended work before the diff is judged, and the actual diff is known before claims in the PR description are evaluated.

## Evidence gathering

For Plan fidelity:
- Identify the files, behavior, and boundaries stated by the plan.
- Record any explicit deviations or shortfalls.
- Compare every changed file and behavior in the diff with that scope.
- Compare the PR description with the actual diff.
- Flag work outside the plan that has no recorded deviation, planned work silently omitted, or claims in the description that the diff does not support.

For Test evidence:
- Identify the behavior the plan says must be reproduced or tested.
- Identify the repository's required checks from repo-facts or repository instructions.
- Compare those requirements with the candidate's captured test evidence.
- Look for observable before-and-after results where reproduction is required and visible outcomes for required checks.
- Do not treat an unsupported statement such as "tests pass" as proof.

For Diff quality:
- Inspect the unified diff and commit list.
- Identify the intended fix.
- Look for unrelated hunks, debug code, dead or commented-out experimental code, generated debris, unrelated formatting churn, or unrelated changes that make the intended fix harder to review.

For Standards and comms:
- Identify required PR-template sections, contribution instructions, disclosure requirements, and explicit maintainer directions.
- Compare each applicable requirement with the PR description.
- Check that required sections contain real answers rather than untouched boilerplate and that required disclosures are present.

## Check execution

Execute the checks in the same order they appear in rubric.md.

For each check:
1. Gather only the evidence named by that rubric row and references/evidence-guide.md.
2. Compare the gathered evidence with the check's pass condition.
3. Grade the check `pass` when the evidence satisfies the complete pass condition.
4. Grade the check `fail` when the evidence shows that the pass condition is not satisfied.
5. Grade the check `unclear` when evidence required to decide the condition is missing or cannot be verified.

Do not invent missing facts, infer that an unreported command passed, or add requirements that are not present in the rubric or repository evidence.

Record one concise fact or quote that explains the grade for each check.

## Verdict assembly

After every check has been graded, apply the verdict rule from rubric.md exactly.

Return `accept` only when every required check passes.

Return `reject` when any required check fails or when any required check is graded `unclear`, because rubric.md treats an unclear required check as a failure.

A disclosed limitation, deferred edge case, or recorded plan deviation does not cause rejection by itself when the rubric's Plan fidelity condition is otherwise satisfied.

When more than one required check fails, report evidence for each failed check. Use the first failing required check in rubric order as the primary reason the PR is not ready.

Finish with the exact JSON output format required by SKILL.md.