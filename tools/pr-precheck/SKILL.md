---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

<!--
THIS IS THE PART YOU WRITE, and it is the last one: the frame itself.
Weeks 1 through 3 handed you a working SKILL.md and you filled the
files behind it; this week the frame ships as headings, and you write
what it says. The frontmatter above and the section headings below are
fixed (CONTRACT.md's layout rule); the instructions under each heading
are yours. Write instructions to the tool, in the imperative, the way
weeks 1-3's frames spoke to you: what to read, in what order, what to
refuse, what to emit. Your executor in the rotation is the test: a
frame gap they hit (cannot tell what the tool reads, or how a verdict
gets assembled) is a missing sentence here.

One section is not yours: the JSON schema in "Verdict and output" is
reproduced from CONTRACT.md verbatim and may not be altered. Your
words decide everything around it.
-->

## The question

Decide whether a pull request package is ready to submit.

A PR package consists of the proposed pull request title and description, the branch diff and commits, the test evidence, the implementation plan and any recorded deviations, and the issue the plan is intended to solve.

The tool answers only whether the package is ready to submit under the rubric. It does not perform a general code review or invent requirements that are not present in the package or repository rules.

## Inputs and modes

Operate in exactly two modes: live mode and eval mode.

In live mode, read:
- the issue being fixed;
- plan.md, including its scope, test plan, and recorded deviation notes;
- the current branch diff relative to the repository's default branch;
- the branch's commits;
- the draft PR title and description;
- captured reproduction and test evidence; and
- applicable repository requirements.

Use `git diff main...HEAD` when `main` is the repository's default branch. The branch diff means all changes on the student's branch relative to the default branch, not only uncommitted changes.

If the course supplies a house-chain plan or reproduction package instead of the student's own plan, use those supplied materials as the plan and evidence inputs.

In eval mode, treat the supplied evaluation bundle as the entire world. Read only the issue, plan-context, repo-facts, candidate PR description, commits, diff, and test evidence contained in that bundle. Do not fetch GitHub, inspect the student's local repository, or use outside facts in eval mode.

## The scope seam (live mode only)

In live mode, read scope.md before grading.

Grade only pull requests that target the Path Review repository named in scope.md and apply the house rules written there.

If the repository entry still contains a bracketed placeholder, stop without grading and tell the user that the course repository must be filled in from the instructor's course information. Never guess the repository.

If the pull request targets a repository outside the stated scope, refuse to grade it.

Ignore scope.md completely in eval mode.

## The voice seam (live mode only)

In live mode, read voice-guide.md and check the draft PR title and description against its writing rules.

Report any broken voice rule in the readable summary.

A voice-guide problem does not change the accept or reject verdict by itself unless rubric.md explicitly contains a graded check that makes it part of the verdict.

Ignore voice-guide.md completely in eval mode.

## Component reads

Read rubric.md for the checks, evidence sources, pass conditions, weights, treatment of unclear evidence, and final verdict rule.

Read references/evidence-guide.md to determine where evidence for each check is located and what observable evidence looks like.

Execute procedure.md as written to determine the read order, gather evidence, execute each check, and assemble the verdict.

Do not silently add a missing procedure step. If procedure.md does not explain how to obtain evidence required by a rubric check, report that procedure gap rather than improvising around it.

If rubric.md contains no student-written checks or procedure.md contains no student-written operating steps, refuse to grade the package and explain which required component is incomplete.

## Verdict and output

The only final verdicts are `accept` and `reject`.

`accept` means the PR package satisfies the rubric and is ready to submit.

`reject` means the package should be held because at least one required condition is not satisfied.

Grade every rubric check before producing the verdict.

A short readable explanation may appear before the machine-readable result. The response must end with the exact fenced JSON structure below. The fenced JSON block must be the final content in the response, with nothing written after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Grade evidence before presentation quality.

Grade the actual artifact rather than how polished or convincing the PR description sounds. A claim in the description must be supported by the diff, plan, test evidence, or repository facts required by the rubric.

Let rubric.md determine what passes and fails. Do not override a rubric condition because the package feels good or bad overall.

Let procedure.md determine how evidence is gathered and how checks are executed. Do not invent a new process when the written procedure is incomplete.

Do not invent missing evidence, assume that an unreported command passed, or infer repository requirements that are not present in the available inputs.

For every check, record the specific fact or quote that determined its grade.

Follow CONTRACT.md as a fixed boundary. If these instructions conflict with CONTRACT.md, CONTRACT.md wins.
