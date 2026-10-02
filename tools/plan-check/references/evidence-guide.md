# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives:** Look in the candidate plan's diagnosis or cause statement, then compare it with the repro-evidence block, the issue description, and any reproduced behavior recorded in the package.

**What good looks like:** The diagnosis explains the behavior actually shown by the reproduction evidence and does not contradict or ignore that evidence. A strong diagnosis identifies a specific likely cause while distinguishing confirmed observations from assumptions.

## Scope

**Where it lives:** Look in the candidate plan's scope statement, files or code areas named for change, explicit out-of-scope statements, and any constraints from the issue thread or repo-facts block.

**What good looks like:** The proposed change is limited to what is needed to resolve the reproduced issue. The plan identifies what will change and what will not change, and avoids unrelated refactoring, cleanup, or feature work.

## Executability

**Where it lives:** Look in the candidate plan's implementation approach, files or code areas to modify, order of work, and any repository facts that identify the affected component.

**What good looks like:** The plan gives concrete implementation steps that another developer could begin following without inventing the main strategy. When the package provides specific files, functions, or components, a good plan uses that information rather than staying generic.

## Test plan

**Where it lives:** Look in the candidate plan's test-plan section and compare it with the reproduction steps, commands, inputs, outputs, and failure behavior in the repro-evidence block.

**What good looks like:** The test plan exercises the behavior that originally failed and states an observable expected result after the fix. The test should distinguish the fixed behavior from the original bug rather than merely saying that tests should pass.

## Honesty

**Where it lives:** Look in the candidate plan's risks, unknowns, assumptions, and deviations sections, together with any claims elsewhere in the plan that depend on uncertain facts.

**What good looks like:** Unknowns are described as unknowns rather than presented as confirmed facts. Important uncertainty includes a concrete way to verify it during implementation. If the implementation later differs from the plan, the deviation is recorded and explained.

## Comms

**Where it lives:** Look in the plan comment, issue thread or thread-highlights block, the candidate plan, and repo-facts describing contribution conventions or disclosure requirements.

**What good looks like:** The comment accurately summarizes the author's own diagnosis, scope, implementation approach, and test plan. It responds to relevant issue-thread context and follows repository conventions. It does not rely on another student's plan or use phrases such as "same approach as above" instead of presenting the author's own work.