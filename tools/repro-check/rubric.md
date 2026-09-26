# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The repro report's environment record, including OS, relevant software/runtime versions, and repository/code state. | Pass if the report records enough relevant environment and code-state information for another contributor to understand the setup used for the reproduction. | required |
| steps-rerunnable | The repro report's reproduction steps and commands. | Pass if the steps are complete, ordered, and specific enough that another contributor could rerun the reproduction without having to guess a required command, input, or setup step. | required |
| behavior-matches-issue | The observed output, traceback, test result, log, screenshot, or other artifact, read against the behavior described in the issue and the issue's repro facts. | Pass if the evidence is aligned with the issue's reported behavior. For a reproduced result, the artifact must directly demonstrate the reported behavior. For a cannot-reproduce result, the artifact must show that the attempted reproduction did not exhibit the reported behavior and the report must identify relevant environment or setup differences. Reject if the artifact shows an unrelated failure or does not support the stated result. | required |
| claim-supported | The report's stated outcome compared with the concrete reproduction evidence it provides. | Pass if the evidence directly supports the contributor's stated outcome. A reproduced claim must be demonstrated by the evidence; an honestly evidenced cannot-reproduce result also passes. | required |
| conventions-followed | The claim comment and repro comment read against the repository rules and conventions in scope.md, the repo-facts block, and any stated contribution policy. | Pass only if the contributor follows all stated repository communication and contribution requirements. If the repository requires disclosure of AI assistance, the comment must explicitly include the required disclosure, including the tool and extent of assistance when the policy asks for them. Missing a required disclosure is a failure. | required |
## Verdict rule

Accept only if every required check passes. If any required check fails or is unclear, reject. Preferred checks, if any are added later, do not change the final verdict.
