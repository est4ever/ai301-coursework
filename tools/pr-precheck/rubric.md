# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Plan-context scope and deviation notes; candidate PR diff; candidate PR description | Pass when every changed file and behavior is within the stated plan or an explicitly recorded deviation, every planned item that is not implemented is disclosed as a deviation or shortfall, and the PR description does not claim work the diff does not contain. | required |
| Test evidence | Plan-context test plan and reproduction steps; candidate test-evidence block; repo-facts required checks | Pass when the planned behavior is demonstrated with observable evidence, including before-and-after results where the plan calls for reproduction, and the outcome of each required repository check is visible. Unsupported statements such as "tests pass" do not count as evidence. | required |
| Diff quality | Candidate unified diff and commit list | Pass when the intended change is identifiable and reviewable and the diff contains no unrelated hunks, debug leftovers, dead or commented-out experimental code, generated debris, or unrelated formatting churn. | required |
| Standards and comms | Repo-facts PR-template requirements, contribution rules, disclosure requirements, and explicit maintainer instructions; candidate PR description | Pass when every applicable required template section and repository instruction is satisfied with real content, including any required AI-use disclosure. Leaving required boilerplate unanswered or omitting a required disclosure fails this check. | required |

## Verdict rule

Accept only when every required check passes.

Grade a check `unclear` when the evidence needed to apply its pass condition is missing or cannot be verified from the package. An `unclear` required check counts as a failure.

A disclosed limitation, deferred edge case, or plan deviation does not fail Plan fidelity by itself when it is explicitly recorded and the PR description accurately reflects what was and was not implemented.
