
Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

# Contribution: Improve toaster message on network connection

**Contribution Number:** [1]

**Student:** Jesse Lin

**GitHub Username:** est4ever

**Issue:** https://github.com/ICTU/quality-time/issues/12823

**Pull Request:** Not opened yet

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and improved an issue-scout tool for GitHub issues**

I upgraded my issue-scout tool to search for, rank, and evaluate open-source GitHub issues using a grading rubric. My initial scan returned 200 issues, but the ranking favored newer repositories with limited discussion, so I adjusted my search strategy. I also found that the original scope check was too strict for real issues, so I replaced it with `scope-feasible` and added difficulty notes. After regrading my candidates and checking for existing claims or related pull requests, I selected Quality Time #12823 as a manageable contribution.

**Unit 6: Installed a voice guard and created local repository memory**

I installed a voice guard to check my GitHub comments against my writing rules before posting them. I also created a `CLAUDE.local.md` file for the Quality Time repository, documenting its contribution conventions, relevant frontend files, and test commands. The initial template lacked repository-specific information, so I used Claude Code to inspect the measurement-fetching subsystem and added those findings to the memory file. I excluded the file using `.git/info/exclude` and verified that Git ignored it.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I initially considered contributing to NVDA, but after evaluating different issues, I decided to look for something more manageable. I used my issue-scout to directly grade three candidates: Quality Time #12823, simpler-grants-gov #12056, and Red-DiscordBot #6794. Quality Time ranked as the best fit because the maintainer had already explained the requested change, the repository was active, and nobody had claimed the issue when I evaluated it.

My issue-scout evaluated this issue on October 22, 2026, and gave it an **accept** verdict with a **moderate** difficulty rating. It passed all four checks: maintainer-active, repo-active, scope-feasible, and unclaimed. The scout considered it moderate because the maintainer had already specified the desired wording, but I still needed to locate the relevant error-handling code. It also noted that identifying a specific metric or subject would be difficult because some measurement requests retrieve data for all metrics at once.

I chose this issue because I wanted more experience working with real software projects, especially debugging existing code and improving how applications handle errors. Even though I have programming experience, I haven't worked much with React or large open-source frontend projects. This issue gives me an opportunity to understand how API requests, error notifications, and automated tests work together. I also learned that my original issue-scout rubric was too strict for real-world GitHub issues, so I updated it to better evaluate whether an issue was manageable.

**Claim Comment:** https://github.com/ICTU/quality-time/issues/12823#issuecomment-6333904364

**Toolkit**

- **Tool:** issue-scout, which I used to search for, evaluate, and compare potential GitHub issues.
- **What it produced:** A set of graded issues, including Quality Time #12823, that helped me decide which contribution to pursue.
- **Difficulties:** The original scope check was too strict for real GitHub issues. I also found that the `unclaimed` check could miss signs of ongoing internal work mentioned in previously closed pull requests.
- **What changed:** I replaced the original scope check with `scope-feasible`, added difficulty notes, and recorded the limitation of the `unclaimed` check for future improvements.

---

## Understanding the Issue

**Problem Description**

Quality Time currently displays a generic error notification when it cannot retrieve measurements from the server. The notification says "Could not fetch measurements" but does not provide enough information for users to understand which request failed or what caused the problem.

**Expected Behavior**

The error notification should provide clearer information about the failed request. When possible, it should identify the relevant metric and subject and clearly label HTTP status information. However, report-wide requests retrieve measurements for multiple metrics at once, so identifying one specific metric is not always possible.

**Current Behavior**

The frontend displays the title "Could not fetch measurements" followed by a generic error description such as "Failure" or "Internal Server Error." The notification does not identify the affected metric or subject. I confirmed this behavior by inspecting the error handlers and running the existing frontend tests, all 29 of which passed.

**Affected Components**

The main components involved are:

- `PageContent.jsx`: Handles report-wide measurement requests and displays an error notification when a request fails.
- `MetricDetails.jsx`: Handles measurement requests for individual metrics and displays errors for rejected requests or unsuccessful responses.
- `PageContent.test.jsx`: Tests the report-wide measurement error behavior.
- `MetricDetails.test.jsx`: Tests the individual-metric error behavior.

**Toolkit**

- **Tool:** Claude Code, used to examine the relevant frontend source files and understand the existing error handling.
- **What it produced:** An explanation of the two measurement-fetching paths, their error handlers, and the tests associated with each component.
- **Difficulties:** The application handles measurement requests in multiple places, which made it difficult to determine where the issue should be fixed.
- **What changed:** I narrowed my investigation to `PageContent.jsx` and `MetricDetails.jsx` and identified the relevant tests before planning any modifications.

---

## Reproduction Process

**Environment Setup**

I reproduced the existing error-message behavior using Windows 11, Node.js v24.19.0, and npm 11.17.0. I forked and cloned the Quality Time repository and created a local branch named `fix-12823-network-error`.

The tested commit was `b7d1a1a2fd620f4e4d515fdbb047542838a255d7`.

I installed the frontend dependencies using `npm ci`. The repository requests Node.js 26.11.1, so npm displayed an unsupported-engine warning. However, installation completed successfully, and the relevant tests ran without errors.

**Steps to Reproduce**

1. Clone `https://github.com/ICTU/quality-time.git` and check out commit `b7d1a1a2fd620f4e4d515fdbb047542838a255d7`.
2. Navigate to `components/frontend`.
3. Run `npm ci` to install the dependencies.
4. Run the following command:

   `npm run test -- --run src/metric/MetricDetails.test.jsx src/PageContent.test.jsx`

5. Inspect the tests named `fails to load measurements due to a failed promise` and `fails to load measurements due to an internal server error` in `MetricDetails.test.jsx`.
6. Inspect `fails to load measurements` in `PageContent.test.jsx`.
7. Compare the expected error messages against the actual handlers in `MetricDetails.jsx` and `PageContent.jsx`.

**Reproduction Evidence**

- **Commit showing reproduction:** none (I used existing tests at upstream commit `b7d1a1a2fd620f4e4d515fdbb047542838a255d7`).
- **Screenshots/logs:** The test output showed `2 passed` test files and `29 passed` tests, with a duration of 4.66 seconds.
- **My findings:** The existing tests confirm that failed measurement requests produce the generic notification "Could not fetch measurements". For example, the rejected-promise test expects a notification containing `severity: "error"`, `title: "Could not fetch measurements"`, and `description: "Failure"`.

I also inspected the source code. `PageContent.jsx` uses the error's message as the description, while `MetricDetails.jsx` uses either the error message or the HTTP status text. Neither currently adds metric or subject context to the notification.

The maintainer explained that report-wide measurement requests fetch data for multiple metrics simultaneously. Therefore, it is not always possible to identify one specific metric or subject in the error notification.

All 29 existing tests passed because they verify the current behavior. This reproduction uses simulated network failures rather than an actual live network outage.

**Toolkit**

- **Tool:** repro-check, used to evaluate my claim comment and reproduction report.
- **What it produced:** An accepted reproduction report containing environment details, exact testing commands, and evidence from the source code and existing tests.
- **Difficulties:** My first report was rejected because I only included the number of passing tests without showing the actual error messages. My second version was rejected because I had not recorded the Git commit ID.
- **What changed:** I added the exact error-message assertions, relevant code excerpts, test names, and commit hash. After these changes, repro-check passed all five checks.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
