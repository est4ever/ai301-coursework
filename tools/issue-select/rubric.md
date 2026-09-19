# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Repo-facts block: maintainer first-response sample and recent default-branch activity | Pass if there is evidence of recent maintainer activity or a maintainer response on recent issues. Fail if the available evidence shows no meaningful maintainer activity. | required |
| repo-active | Repo-facts block: last push to any branch, latest release, and recent default-branch commits | Pass if the repository has had meaningful development activity within the past 6 months. | required |
| newcomer-scope | Issue body, labels, and comment thread, including unresolved design questions and prior implementation attempts | Pass if the issue states a concrete desired outcome or identifiable defect and a contributor can begin work without first resolving an open product, design, or API decision. Do not fail only because the task spans several files, lacks step-by-step instructions, or involves performance or concurrency. Fail if the thread shows unresolved design/API behavior, substantial abandoned implementation attempts, or that the requested behavior is still being negotiated. | required |
| unclaimed | Repo-facts block: assignees and linked PRs; issue comment thread for explicit work-in-progress claims | Pass if there is no assignee, no active linked PR, and no clear evidence that another contributor is already implementing the issue. | required |

## Verdict rule

Accept only if all required checks pass. Reject if any required check fails. An unclear (?) grade counts as a failure because there is not enough evidence to safely accept the issue.