# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

Where it lives:
- In eval mode, read the plan-context block for the plan's scope, boundaries, and deviation notes, then compare them with the candidate PR's unified diff and PR description.
- In live mode, read plan.md and any recorded deviation notes, then compare them with `git diff main...HEAD` and the draft PR description.

What good looks like:
Every changed file and behavior is covered by the stated plan or an explicitly recorded deviation. Planned work that was not completed is disclosed rather than silently omitted. The PR description accurately states what the diff actually implements and does not claim behavior that is absent from the diff.

## Test evidence (harness category: not-tested)

Where it lives:
- In eval mode, read the plan-context test plan and reproduction steps, the candidate PR's test-evidence section, and any required checks named in repo-facts.
- In live mode, compare the plan's reproduction and test steps with captured before-and-after output and the results of the repository's required checks.

What good looks like:
The evidence shows an observable result for the behavior being tested. When the plan calls for reproducing a bug, the evidence shows the behavior before the fix and the result after the fix. Required repository checks have visible outcomes. A statement such as "tests pass" without observable results is not enough.

## Diff quality (harness category: unreviewable)

Where it lives:
- In eval mode, inspect the candidate PR's unified diff and commit list.
- In live mode, inspect `git diff main...HEAD` and the branch's commits.

What good looks like:
The intended change is easy to identify and review. The diff does not contain unrelated edits, debug leftovers, dead or commented-out experimental code, generated debris, or unrelated formatting churn that makes the change harder to review.

## Standards and comms (harness category: standards-wall)

Where it lives:
- In eval mode, read repo-facts for the PR-template requirements, contribution instructions, disclosure requirements, and explicit maintainer directions, then compare them with the candidate PR description.
- In live mode, inspect the repository's PR template, contribution instructions, issue-thread directions, disclosure requirements, and the draft PR description.

What good looks like:
Every applicable required template section and repository instruction is addressed with real content. Required disclosures, including an AI-use disclosure when applicable, are present. Boilerplate is not left unanswered, and explicit maintainer directions relevant to the PR are followed.