# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: Look in the repro report for the environment record, and use the issue context or repo-facts block to determine which versions, platforms, dependencies, and code state are relevant to the issue.

What good looks like: The report names the relevant operating system or platform, important software/runtime versions, and the repository or code state used for the reproduction. The information should be specific enough that another contributor can understand the setup and identify meaningful differences from the issue's target environment.

## Steps

Where it lives: Look in the repro report for the reproduction procedure, including setup instructions, commands, inputs, starting state, and the action that triggers the reported behavior.

What good looks like: The steps are complete, ordered, and specific enough for another contributor to follow from the stated starting condition to the trigger without guessing a required command, input, path, dependency, or setup step.

## Behavior shown

Where it lives: Look at the concrete artifacts in the repro report, such as command output, test results, tracebacks, logs, screenshots, or other recorded observations. Compare those artifacts with the behavior described in the issue and any repro facts supplied in the package.

What good looks like: For a successful reproduction, the artifact directly shows the behavior the issue reports rather than an unrelated failure. For an honest cannot-reproduce result, the artifact shows what happened when the stated reproduction steps were attempted, and the report identifies relevant environment or setup differences that may explain why the reported behavior did not occur. In either case, the report's conclusion must match the artifact.

## Honesty

Where it lives: Compare the contributor's stated outcome in the repro report with the commands, observations, and artifacts provided as evidence.

What good looks like: The conclusion says only what the evidence supports. A claim that the issue was reproduced must be backed by evidence showing the reported behavior. A cannot-reproduce result is also valid when the contributor clearly reports what was tried and the evidence shows that the reported behavior did not occur.

## Comms

Where it lives: Look at the claim comment and repro comment, and compare them with the repository rules in scope.md, the repo-facts block, and any stated contribution or disclosure requirements.

What good looks like: The comments are specific to the issue, describe the contributor's own work, and follow every stated repository communication and contribution rule. If the repo-facts block or contribution policy requires disclosure of AI assistance, the candidate comment must explicitly contain that disclosure and include any details the policy requires, such as the tool used and the extent of assistance. Absence of a required disclosure is a failure even when the technical reproduction is otherwise valid.