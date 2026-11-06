Contribution: Improve toaster message on network connection
Contribution Number: [1]

Student: Jesse Lin

GitHub Username: est4ever

Issue: https://github.com/ICTU/quality-time/issues/12823

Pull Request: https://github.com/ICTU/quality-time/pull/14940

One repository, built across units 5 to 8, telling the whole story of my real open-source contribution.

## Toolkit
Unit 5: Built and improved an issue-scout tool for GitHub issues

I upgraded my issue-scout tool to search for, rank, and evaluate open-source GitHub issues using a grading rubric. My initial scan returned 200 issues, but the ranking favored newer repositories with limited discussion, so I adjusted my search strategy. I also found that the original scope check was too strict for real issues, so I replaced it with scope-feasible and added difficulty notes. After regrading my candidates and checking for existing claims or related pull requests, I selected Quality Time #12823 as a manageable contribution.

Unit 6: Installed a voice guard and created local repository memory

I installed a voice guard to check my GitHub comments against my writing rules before posting them. I also created a CLAUDE.local.md file for the Quality Time repository, documenting its contribution conventions, relevant frontend files, and test commands. The initial template lacked repository-specific information, so I used Claude Code to inspect the measurement-fetching subsystem and added those findings to the memory file. I excluded the file using .git/info/exclude and verified that Git ignored it.

Unit 7: Configured a ship-gate, passed plan-check, implemented the fix, and opened PR #14940

I installed the Unit 7 ship-gate and created a manifest for the frontend tests, lint checks, and build. Windows timezone, shell, and CRLF line-ending differences initially caused failures, so I adjusted the commands without modifying unrelated files. I verified the ship-gate and voice guard independently, then used plan-check to review my proposed fix against the maintainer's comments. After several revisions to the notification wording, test expectations, and scope, plan-check returned accept with all six checks passing.

I then implemented the fix in one commit that changed four implementation and test files. Three updated regression tests failed against the original implementation, and all 29 focused tests passed after the fix. The full frontend suite passed 1,108 tests across 110 files, and lint, formatting, lockfile, and build checks passed. The ship-gate passed all required checks before Claude Code opened PR #14940, which is awaiting review.

Unit 8: Installed the PR monitor, wrote my playbook, and declined to post its draft

The PR monitor was already installed in ~/.claude/pr-monitor from the Unit 8 starter, and its files matched the starter's skill folder. Its playbook.md was still the blank template, so the first run would have refused to draft. I wrote my playbook from real data. CONTRIBUTING.md gives no review wait, and the five most recent merged PRs from non-maintainer, non-bot authors (#14709, #13062, #12913, #12592, #12507) all got a first response or a merge within a day. Those authors appear to be regular ICTU contributors, and several merged their own PRs, so I set a 7-day first-review threshold and a 14-day stalled-review threshold.

On November 6, 2026, monitor.py --state found PR #14940 on the first try: open for 4 days, 6 failing checks, 24 passing, 1 skipped, no review decision, MERGEABLE but BEHIND master. The full run classified it as BRANCH-STALE and used the rebase-plan form. The draft's move was "Don't rebase or push now. Wait note only." It said nothing should be posted, because my failure explanation was already in the thread, no maintainer had asked for a rebase, and my nudge threshold isn't reached until November 9. I agreed and posted nothing.

The monitor's shortcoming is that it can't tell whether failing checks come from my change. It picked the rebase form only because checks were red, and its run report counted the Copilot "insufficient quota" review as one of the thread's two entries. The draft's own notes told me to confirm the six failing jobs were the ones I had described, and it pointed out that my November 2 comment quoted only the Docker Hub error, not the exact job names. Because of that, I pulled the logs of all six failing jobs. Each one fails on a dependency advisory (minimatch GHSA-g3hv-75wg-m249, glob GHSA-3g3m-2fjr-6pw4, four pyasn1 advisories) or on the Docker Hub "Username and password required" error. I recorded the exact job names in the Implementation Notes so I can answer a maintainer without guessing. I didn't post a correction, because it would repeat my earlier comment.

## Why I Chose This Issue
I initially considered contributing to NVDA, but after evaluating different issues, I decided to look for something more manageable. I used my issue-scout to directly grade three candidates: Quality Time #12823, simpler-grants-gov #12056, and Red-DiscordBot #6794. Quality Time ranked as the best fit because the maintainer had already explained the requested change, the repository was active, and nobody had claimed the issue when I evaluated it.

My issue-scout evaluated this issue on October 22, 2026, and gave it an accept verdict with a moderate difficulty rating. It passed all four checks: maintainer-active, repo-active, scope-feasible, and unclaimed. The scout considered it moderate because the maintainer had already specified the desired wording, but I still needed to locate the relevant error-handling code. It also noted that identifying a specific metric or subject would be difficult because some measurement requests retrieve data for all metrics at once.

I chose this issue because I wanted more experience working with real software projects, especially debugging existing code and improving how applications handle errors. Even though I have programming experience, I haven't worked much with React or large open-source frontend projects. This issue gives me an opportunity to understand how API requests, error notifications, and automated tests work together. I also learned that my original issue-scout rubric was too strict for real-world GitHub issues, so I updated it to better evaluate whether an issue was manageable.

Claim Comment: https://github.com/ICTU/quality-time/issues/12823#issuecomment-6333904364

Toolkit
- Tool: issue-scout, which I used to search for, evaluate, and compare potential GitHub issues.
- What it produced: A set of graded issues, including Quality Time #12823, that helped me decide which contribution to pursue.
- Difficulties: The original scope check was too strict for real GitHub issues. I also found that the unclaimed check could miss signs of ongoing internal work mentioned in previously closed pull requests.
- What changed: I replaced the original scope check with scope-feasible, added difficulty notes, and recorded the limitation of the unclaimed check for future improvements.

## Understanding the Issue
Problem Description

Quality Time currently displays a generic error notification when it cannot retrieve measurements from the server. The notification says "Could not fetch measurements" but does not provide enough information for users to understand which request failed or what caused the problem.

Expected Behavior

The error notification should provide clearer information about the failed request. When possible, it should identify the relevant metric and subject and clearly label HTTP status information. However, report-wide requests retrieve measurements for multiple metrics at once, so identifying one specific metric is not always possible.

Current Behavior

The frontend displays the title "Could not fetch measurements" followed by a generic error description such as "Failure" or "Internal Server Error." The notification does not identify the affected metric or subject. I confirmed this behavior by inspecting the error handlers and running the existing relevant frontend tests, all 29 of which passed.

Affected Components

The main components involved are:
- PageContent.jsx: Handles report-wide measurement requests and displays an error notification when a request fails.
- MetricDetails.jsx: Handles measurement requests for individual metrics and displays errors for rejected requests or unsuccessful responses.
- PageContent.test.jsx: Tests the report-wide measurement error behavior.
- MetricDetails.test.jsx: Tests the individual-metric error behavior.
Toolkit
- Tool: Claude Code, used to examine the relevant frontend source files and understand the existing error handling.
- What it produced: An explanation of the two measurement-fetching paths, their error handlers, and the tests associated with each component.
- Difficulties: The application handles measurement requests in multiple places, which made it difficult to determine where the issue should be fixed.
- What changed: I narrowed my investigation to PageContent.jsx and MetricDetails.jsx and identified the relevant tests before planning any modifications.

## Reproduction Process
Environment Setup

I reproduced the existing error-message behavior using Windows 11, Node.js v24.19.0, and npm 11.17.0. I forked and cloned the Quality Time repository and created a local branch named fix-12823-network-error.

The tested commit was b7d1a1a2fd620f4e4d515fdbb047542838a255d7.

I installed the frontend dependencies using npm ci. The repository requests Node.js 26.11.1, so npm displayed an unsupported-engine warning. However, installation completed successfully, and the relevant tests ran without errors.

Steps to Reproduce
1. Clone https://github.com/ICTU/quality-time.git and check out commit b7d1a1a2fd620f4e4d515fdbb047542838a255d7.
2. Navigate to components/frontend.
3. Run npm ci to install the dependencies.
4. Run the following command:
   npm run test -- --run src/metric/MetricDetails.test.jsx src/PageContent.test.jsx
5. Inspect the tests named fails to load measurements due to a failed promise and fails to load measurements due to an internal server error in MetricDetails.test.jsx.
6. Inspect fails to load measurements in PageContent.test.jsx.
7. Compare the expected error messages against the actual handlers in MetricDetails.jsx and PageContent.jsx.
Reproduction Evidence
- Commit showing reproduction: None. I used existing tests at upstream commit b7d1a1a2fd620f4e4d515fdbb047542838a255d7.
- Screenshots/logs: The initial test output showed 2 passed test files and 29 passed tests, with a duration of 4.66 seconds.
- My findings: The existing tests confirm that failed measurement requests produce the generic notification "Could not fetch measurements". For example, the rejected-promise test expects a notification containing severity: "error", title: "Could not fetch measurements", and description: "Failure".
I also inspected the source code. PageContent.jsx uses the error's message as the description, while MetricDetails.jsx uses either the error message or the HTTP status text. Neither currently adds metric or subject context to the notification.

The maintainer explained that report-wide measurement requests fetch data for multiple metrics simultaneously. Therefore, it is not always possible to identify one specific metric or subject in the error notification.

All 29 existing tests passed because they verify the current behavior. This reproduction uses simulated network failures rather than an actual live network outage.

Toolkit
- Tool: repro-check, used to evaluate my claim comment and reproduction report.
- What it produced: An accepted reproduction report containing environment details, exact testing commands, and evidence from the source code and existing tests.
- Difficulties: My first report was rejected because I only included the number of passing tests without showing the actual error messages. My second version was rejected because I had not recorded the Git commit ID.
- What changed: I added the exact error-message assertions, relevant code excerpts, test names, and commit hash. After these changes, repro-check passed all five checks.

## Solution Approach
Analysis

Quality Time currently shows a generic Could not fetch measurements error notification, with the raw exception message or HTTP status text as the description. This does not clearly explain the failed operation or identify the metric and subject when that context exists.

In PageContent.jsx, the report-wide measurement request uses getMeasurements(minDate, maxDate). Its existing catch handler displays a generic notification for rejected requests. A report-wide request retrieves measurements for multiple metrics, so it cannot reliably name one affected metric or subject. Reading the code also revealed that a resolved report-wide response with ok === false does not currently raise the same notification. Changing that behavior would add a new error path, not merely improve an existing message.

In MetricDetails.jsx, the fetchMeasurements() helper calls getMetricMeasurements(metricUuid, reportDate). It displays the generic error for both rejected requests and responses where json.ok === false, using error.message or json.statusText respectively. The helper currently receives only metricUuid, even though the component has access to the metric and subject through report.subjects[subjectUuid].metrics[metricUuid] and report.subjects[subjectUuid]. The metric and subject names therefore need to be resolved using the existing naming conventions and made available to the helper.

The relevant existing tests in PageContent.test.jsx and MetricDetails.test.jsx pass because they assert the current generic notifications. They establish the original behavior but do not yet verify the maintainer's requested wording.

The maintainer's April 15, 2026 comment requested wording along these lines for an identifiable metric and subject:

Could not reach the Quality-time API-server to retrieve measurements for metric metric name in subject subject name. Please check your network connectivity.

It also requested explicit labels such as HTTP status code: XYZ. HTTP status message: Bad gateway. The April 1 comment clarifies that report-wide requests cannot necessarily identify one metric. The plan must respect both comments.

Proposed Solution

For individual-metric failures in MetricDetails.jsx, I will follow the maintainer's April 15 connectivity wording, inserting the actual metric and subject names when available. I will resolve the displayed names using the repository's existing naming and data-model fallback conventions and pass the required context to fetchMeasurements() at both its initial-load and reload call sites.

For json.ok === false responses, I will include clearly labeled HTTP status code and message values when provided. For a rejected network request with no HTTP response, I will keep the available exception description without inventing a status code.

For rejected report-wide requests in PageContent.jsx, I will improve the existing notification to say:

Could not reach the Quality-time API-server to retrieve measurements. Please check your network connectivity.

This variation reflects the fact that report-wide requests involve multiple metrics and cannot reliably name a single metric or subject. I raised the difference between report-wide and metric-specific wording as a question in plan-comment.md.

Conditional change: I also noticed that a resolved, non-OK HTTP response in PageContent.jsx currently does not produce this error notification. I will not add a new notification path for those responses in this PR unless the maintainer explicitly approves that extension. If it is not approved, I will leave the behavior unchanged and consider a separate follow-up issue. The underlying API request behavior, backend, unrelated components, and unrelated formatting remain outside this PR.

Implementation Plan
1. Inspect the error handlers in PageContent.jsx and MetricDetails.jsx and confirm which error, response, metric, and subject fields are actually available.
2. Check existing name helpers and data-model fallbacks to determine appropriate displayed metric and subject names.
3. Update or add focused tests that assert the new notification messages and verify that they fail against the original behavior before the implementation changes.
4. Change fetchMeasurements() in MetricDetails.jsx to accept the needed metric and subject context and update both its initial-load and reload call sites.
5. Improve the individual-metric notification wording and include labeled HTTP status details when available; do not invent HTTP codes for rejected requests without a response.
6. Improve only the existing PageContent.jsx catch-path notification for rejected report-wide requests, without claiming to identify a particular metric.
7. Conditional only: If the maintainer explicitly approves handling resolved report-wide HTTP errors in this PR, add the non-OK response notification and its dedicated test. Otherwise omit that change and test from this PR.
8. Use the following test cases to verify observable behavior:
   - Case A — report-wide rejected request (PageContent.test.jsx): Mock new Error("Error description"). Expect severity error, title Could not reach the Quality-time API-server to retrieve measurements. Please check your network connectivity., and description Error description.
   - Case B — individual-metric rejected request (MetricDetails.test.jsx): With a test metric called Violations and subject Example subject, reject with new Error("Failure"). Expect severity error, title Could not reach the Quality-time API-server to retrieve measurements for metric Violations in subject Example subject. Please check your network connectivity., and description Failure, without a fabricated HTTP status.
   - Case C — individual-metric HTTP failure (MetricDetails.test.jsx): Mock { ok: false, status: 502, statusText: "Bad gateway" }. Expect the same metric-specific title and description HTTP status code: 502. HTTP status message: Bad gateway.
   - Case D — report-wide HTTP failure (conditional, PageContent.test.jsx): Only if the maintainer approves the added behavior, mock { ok: false, status: 502, statusText: "Bad gateway" } and assert the report-wide title and labeled description HTTP status code: 502. HTTP status message: Bad gateway. Otherwise exclude this test from the PR.
9. Confirm each implemented test fails against the old behavior and passes after the fix. Run the full frontend tests, Windows-compatible lint/formatting checks, and production build.
10. Save before-and-after outputs and actual verification results in test_evidence.md, then inspect the diff for unrelated changes.
11. Prepare pr_draft.md, run the branch ship-gate and PR precheck, and open the PR only after those checks pass.
Risks and Unknowns
- Report-wide context: The maintainer's April 1 comment says a report-wide request may not identify one metric, while the April 15 message gives a metric-specific example. I will use the specific wording only where that context is available and leave the report-wide wording question in the draft comment.
- Additional HTTP error path: Handling resolved ok === false responses in PageContent.jsx would be a new behavior. If the maintainer says it belongs in a separate issue, I will leave it and conditional Test Case D out of this PR.
- Missing names: I will use the repository's existing naming fallback conventions rather than displaying undefined names.
- Missing HTTP status: Some rejected requests provide only an exception message. The notification should never fabricate HTTP status details.
Deviations from the Plan

The planning draft was revised after feedback to make the additional PageContent.jsx HTTP failure path conditional on maintainer approval. The implementation otherwise followed the accepted plan, with these recorded decisions:

- Focused ESLint dependency suppression (deviation): Passing the metric and subject names into fetchMeasurements() made them part of the measurement-fetching effect in MetricDetails.jsx. Adding them to the dependency array would refetch measurements whenever a name changed, even though the names are only used in the error message. I kept the dependency array as [metricUuid, reportDate] and added a targeted eslint-disable-next-line @eslint-react/exhaustive-deps comment, following an approach already used elsewhere in the project. The plan did not anticipate this, so I recorded it as a deviation and explained it in the PR description.
- Report-wide HTTP ok === false handling (deliberately excluded): The maintainer has not approved adding a new notification for resolved report-wide responses with ok === false, so I left that behavior unchanged and did not include conditional Test Case D. The PR description asks the maintainers whether it belongs in this PR or a separate issue.

Toolkit
- Tool: Claude Code plan-check, used to grade plan.md and plan-comment.md against the issue discussion, repository code, reproduction evidence, and the course rubric.
- What it produced: An approved implementation plan. The latest plan-check returned accept, with all six required checks passing.
- Difficulties: The first draft did not follow the April 15 maintainer wording or give precise expected test results. Later drafts conflicted about whether a new report-wide ok === false notification was included in scope, causing repeated rejections.
- What changed: I used the maintainer's requested metric-specific wording, specified concrete tests for rejected requests and HTTP failures, and made the new report-wide HTTP error path and Test Case D conditional on explicit maintainer approval. I kept plan-comment.md as an unposted draft.

## Testing Strategy
My testing strategy combines reproducing the existing behavior, verifying the new notification requirements, running the frontend test suite, checking code quality, and building the application.

1. Baseline Reproduction

Before implementing the fix, I ran the existing tests for PageContent.jsx and MetricDetails.jsx. All 29 tests passed, confirming that the existing error messages matched the current expectations.

These tests establish the baseline behavior. They do not prove that the maintainer's requested improvement has been implemented.

2. Focused Tests for the Fix

I updated three regression tests in PageContent.test.jsx and MetricDetails.test.jsx to verify that:
- A report-wide measurement failure produces the new connectivity title without falsely identifying an individual metric, and keeps the original error message as the description (Case A).
- An individual-metric rejected request names the metric and subject, including the fallback metric name Violations, and does not invent an HTTP status (Case B).
- An individual-metric HTTP 502 response displays HTTP status code: 502. HTTP status message: Bad gateway. and the measurement loading state indicates failure (Case C).
I also changed the test fixture's subject name to Example subject so the expected notification is clearer. Conditional Test Case D was not added because the report-wide ok === false path was excluded from the PR.

Results:
- Before the fix: The three updated regression tests failed against the original implementation, while the other 26 focused tests passed.
- After the fix: All 29 focused tests passed.

3. Full Frontend Test Suite

I ran the complete frontend test suite before modifying the code:

TZ=UTC npm --prefix components/frontend run test -- --run

Results:
- Test files: 110 passed out of 110.
- Individual tests: 1,108 passed out of 1,108.
- Failures: 0.
- Duration: Approximately 47 seconds.
The first full test run without the timezone override had two failures related to date interpretation in App.test.jsx and utils.test.jsx. Both involved March 13, 2020, being interpreted as March 12.

Running the suite with TZ=UTC resolved these failures without requiring source-code changes.

After implementing the fix, I reran the full suite. It again passed 1,108 tests across 110 test files.

4. Frontend Linting and Formatting

The original lint command encountered Windows-specific problems.

First, npm interpreted the project's semicolon-separated lint script in a way that caused ESLint to receive Prettier's --check option.

After running the commands through Bash, ESLint reported 27,676 formatting errors and one warning. Investigation with git ls-files --eol showed that Git stored the inspected files with LF line endings while the Windows working copy used CRLF.

I resolved the formatting-check issue without rewriting repository files. I ran ESLint's remaining rules separately from the Prettier check, then used Prettier with --end-of-line auto.

The successful combined check used:

bash -lc "cd components/frontend && npx eslint *.mjs src --rule 'prettier/prettier: off' && npx prettier *.mjs src --check --end-of-line auto --log-level warn && npx lockfile-lint"

Results:
- ESLint: 0 errors and 1 warning about an unused ESLint-disable directive.
- Prettier: No formatting issues reported.
- Lockfile check: No issues detected.
The ESLint warning was in DashboardCard.jsx, outside the files involved in my planned fix.

After implementing the fix, I reran these checks. ESLint reported zero errors and the same pre-existing DashboardCard.jsx warning, and Prettier and lockfile-lint passed.

5. Production Build

I ran:

npm --prefix components/frontend run build

The Vite build completed successfully and reported 2,003 modules transformed.

Vite displayed a warning that some generated chunks exceeded 500 kB after minification. This warning did not prevent the build from succeeding.

After implementing the fix, the production build passed again.

6. Final Verification

Final results after the fix:
- Focused regression tests: 29/29 passed; the three updated tests failed against the original implementation.
- Full frontend test suite: 1,108 tests across 110 test files passed.
- ESLint: 0 errors and 1 pre-existing warning in DashboardCard.jsx.
- Prettier: Passed.
- lockfile-lint: Passed.
- Production build: Passed.
- Ship-gate: Passed all required checks before the PR was opened.
The changes were verified with automated tests only. I did not perform manual testing against a running Quality-time instance. Testing ran on Node.js 24.19.0 rather than the repository's requested Node.js 26.11.1.

Toolkit
- Tool: Vitest, ESLint, Prettier, lockfile-lint, npm, Git Bash, and the Unit 7 ship-gate, used to establish baseline results and verify the fix before opening the PR.
- What it produced: A baseline of 1,108 passing tests, three regression tests that failed before the fix and passed after it, a passing full suite and build after the fix, and a passing ship-gate run.
- Difficulties: The initial test suite had timezone-dependent failures, while ESLint and Prettier encountered shell and line-ending compatibility problems on Windows.
- What changed: I used UTC for date-sensitive tests and separated linting from formatting with an automatic line-ending setting. This allowed verification without modifying unrelated source files.

## Implementation Notes
October 22, 2026 — Issue selection and initial investigation

I evaluated several GitHub issues using my issue-scout tool and selected Quality Time #12823. I chose this issue because the maintainer's request was reasonably focused and the changes appeared manageable for my experience level.

I inspected the relevant frontend components and identified PageContent.jsx and MetricDetails.jsx as the main locations involved in measurement-fetch errors.

I also identified the corresponding test files and confirmed that the existing tests expected the generic error notification.

October 28, 2026 — Installed the Unit 7 ship-gate

I cloned the ai301-unit7-starter repository and installed the ship-gate into my local Quality Time repository.

My first installation attempt failed because I used Windows PowerShell-style paths and Python launcher commands inside Git Bash. I corrected the path syntax and used the available python command to run the installer.

The installer successfully configured the ship-gate, added its Claude Code hook, enabled the real-repository precheck scope, and created ship-manifest.md.

I then prepared the manifest to run the frontend tests, lint checks, and production build.

October 28, 2026 — Investigated test failures

I ran the complete frontend test suite and initially encountered two failing tests out of 1,108.

The failures involved JavaScript date handling, with March 13, 2020, being interpreted as March 12.

After rerunning the tests with TZ=UTC, all 1,108 tests across 110 test files passed.

I decided not to change the unrelated date-handling code because the failures were caused by the local testing environment and were outside the scope of issue #12823.

October 28, 2026 — Resolved Windows linting issues

The original frontend lint command failed because of shell compatibility issues.

After correcting how the commands were executed, I encountered thousands of Prettier-related errors. I traced these to line-ending differences between Git's LF files and my Windows working copy's CRLF files.

I adjusted the verification commands to handle line endings appropriately rather than reformatting the entire repository.

The final combined lint check completed with zero ESLint errors, one unrelated warning, no Prettier formatting issues, and a successful lockfile check.

The frontend production build also completed successfully.

October 28, 2026 — Verified Claude Code hooks

I tested the ship-gate by asking Claude Code to run:

gh pr create --help

The ship-gate blocked the command with:

ship-gate: STOP. no PR text to check.

I separately tested the voice guard by asking Claude Code to run:

gh issue comment --help

The voice guard blocked the command because no comment body was provided.

These results confirmed that both hooks were active and enforcing their intended safeguards.

October 28, 2026 — Revised and passed plan-check

I created plan.md and plan-comment.md and ran Claude Code's plan-check against issue #12823. The first version was rejected for not following the maintainer's April 15 wording and for not specifying exact notification test expectations. Later versions were rejected because the plan and the comment disagreed on whether the PR would introduce new report-wide HTTP error handling. I revised the plan to keep that additional behavior conditional on maintainer approval. The final plan-check result was accept, with all six required checks passing. I have not posted plan-comment.md to GitHub.

November 1, 2026 — Implemented the fix for issue #12823

I implemented the fix with Claude Code in one commit, Clarify measurement fetch error notifications, which changed four files:
- components/frontend/src/metric/MetricDetails.jsx: The notification title now says the frontend could not reach the Quality-time API-server to retrieve measurements for the named metric in the named subject and asks the user to check network connectivity. The names come from the existing getMetricName and getSubjectName helpers, which fall back to the type name when no custom name exists. fetchMeasurements() receives the names at both the initial-load and reload call sites. HTTP errors now show labeled status code and message values, and rejected requests keep the original error message.
- components/frontend/src/PageContent.jsx: The report-wide notification uses the same connectivity wording without naming a metric or subject, because that request covers many metrics.
- components/frontend/src/metric/MetricDetails.test.jsx and components/frontend/src/PageContent.test.jsx: Three regression tests were updated for Cases A, B, and C.

I first confirmed that the three updated tests failed against the original implementation, then confirmed that all 29 focused tests passed after the change. I recorded the ESLint dependency suppression as a deviation and kept report-wide ok === false handling out of scope.

November 1, 2026 — Passed the ship-gate and opened the PR

I reran the full frontend suite, ESLint, Prettier, lockfile-lint, and the production build, and all passed apart from the pre-existing DashboardCard.jsx ESLint warning. The ship-gate passed all required checks, and Claude Code then opened PR #14940.

November 2, 2026 — Investigated CI failures on the PR

CI ran on head commit 23a49d85a, and 6 of 31 checks failed. The frontend tests and lint passed inside the CI job, but the job failed on npm dependency advisories for glob and minimatch. The documentation and API server checks reported dependency vulnerabilities, and the Docker-based workflows failed at Docker Hub login with "Username and password required." None of these involve my four changed files. I posted one comment on the PR explaining this and asked whether the maintainers wanted anything changed before review. I didn't change the upstream code to work around the failures.

November 6, 2026 — Unit 8: PR monitor and playbook

I confirmed the PR monitor in ~/.claude/pr-monitor matched the Unit 8 starter's skill folder. I kept the install and didn't change any Claude Code settings or hooks. The playbook was still blank, so I researched the repository before filling it in. CONTRIBUTING.md has no stated review wait. Of the last 400 merged PRs, 397 were opened by maintainer fniessink, so I used GitHub search to find the five most recent merged PRs from outside, non-bot authors:

- #14709: merged in about 1 hour.
- #13062: first review in about 2 hours.
- #12913: changes requested in about 3 hours.
- #12592: approved in about 8 hours, merged after 22 days.
- #12507: self-merged in 7 minutes.

I wrote the playbook's four sections: Response rules, Staleness thresholds (7 days for a first review, 14 for a stalled one), Nudge etiquette (one nudge, one ask, tagging only a maintainer who has already engaged), and Walk-away criteria.

From my Quality-time clone on fix-12823-network-error, with gh's default repo already set to ICTU/quality-time, monitor.py --state found PR #14940 without a NO PR YET stop. The full run took one Claude call. It wrote run-report.md (state BRANCH-STALE because of failing checks) and drafted-move.md, whose move was "Don't rebase or push now. Wait note only." I compared it with the live PR: still no maintainer activity, Copilot's quota message as the only review, and my November 2 comment already covering the failures. The draft was right, so I posted nothing.

Following the draft's note, I pulled the six failing job logs:

- Documentation / build: minimatch GHSA-g3hv-75wg-m249.
- Application tests (Docker) / build and Feature tests / build: Docker Hub "Username and password required."
- CI test for api_server: four pyasn1 0.6.4 advisories.
- CI test for frontend: glob GHSA-3g3m-2fjr-6pw4 and minimatch.
- CI test for renderer: minimatch.

All six match what my comment said. I also noticed that my local branch commit 7b13a86d8 sits on an older master commit than the PR head 23a49d85a. Same change, different parent. I left both alone because the monitor and my playbook both say not to rebase until a maintainer asks.

Toolkit
- Tool: Claude Code, Git, npm, Vitest, ESLint, Prettier, the Unit 7 ship-gate, GitHub CLI, and in Unit 8 the PR monitor.
- What it produced: A working local contribution environment, validated baseline checks, functioning GitHub safeguards, an accepted implementation plan, a one-commit fix with three regression tests, PR #14940, and a monitor run report with a wait-note draft.
- Difficulties: Git Bash required different Windows path and Python commands, and timezone and line-ending behavior disrupted tests and linting. Plan-check also rejected several drafts because the wording, tests, and scope did not fully agree with the maintainer's direction. During implementation, passing the names into the fetching effect raised an ESLint dependency rule that the plan had not anticipated.
- What changed: I adapted the commands without rewriting unrelated files, revised the plan to include exact expected messages and tests, and made extra report-wide HTTP error handling conditional. Plan-check finally returned accept with six passing checks. I resolved the dependency warning with a focused suppression instead of changing fetching behavior, and recorded it as a deviation.

## Pull Request
PR Link: https://github.com/ICTU/quality-time/pull/14940

PR Title: Clarify measurement fetch error notifications (#12823)

PR Description:

The pull request improves the error notifications displayed when Quality Time cannot retrieve measurements, following the maintainer's suggested wording in issue #12823.

- MetricDetails.jsx: The title names the metric and subject and asks the user to check network connectivity. HTTP error responses show a labeled status code and message, and rejected requests keep the original error message without an invented status code.
- PageContent.jsx: The report-wide title uses the same connectivity wording without naming a metric or subject.

The description explains the scope limits: report-wide ok === false responses still do not produce a notification, and the focused ESLint dependency suppression keeps measurement fetching unchanged. It lists the before-and-after test results, notes that no manual testing was performed against a running instance and that tests ran on Node.js 24.19.0, and describes how Claude Code contributed. It asks the maintainers whether the report-wide wording is appropriate and whether report-wide ok === false handling belongs in this PR or a separate issue.

The PR references the issue with:

Fixes #12823

Maintainer Feedback:
- November 1, 2026 (01:43 UTC November 2): I opened PR #14940. Seconds later, the automated Copilot reviewer posted that it "was unable to review this pull request because the user who requested the review has insufficient remaining quota." That isn't code feedback or a maintainer review, so I didn't reply to it.
- November 2, 2026: CI ran on head commit 23a49d85a, and 6 checks failed. I read the logs and posted one comment. It said the frontend tests and lint passed but CI failed on glob and minimatch dependency advisories, the documentation and API server checks reported dependency vulnerabilities, and the Docker-based workflows failed at Docker Hub authentication ("Username and password required"). It also said these failures appear unrelated to my four changed files.
- November 6, 2026: No maintainer comment, review, or merge yet. The PR has been open for 4 days, and its last activity was my November 2 comment. I checked the six failing jobs again: Documentation / build, Application tests (Docker) / build, Feature tests / build, and the CI test jobs for api_server, frontend, and renderer. They are still the dependency advisory and Docker Hub failures described in my comment, so I posted nothing new. Under my playbook, the earliest one-time nudge is November 9, 2026.

Status: Awaiting review. As of November 6, 2026, the PR is open, not a draft, and has no review decision. GitHub reports it as MERGEABLE with merge state BEHIND master. Checks show 6 failing, 24 passing, and 1 skipped.

Toolkit
- Tool: Claude Code, the voice guard, the Unit 7 ship-gate, and GitHub CLI to open the PR. In Unit 8, the PR monitor (monitor.py --state, then a full run) reads the PR and drafts my next move.
- What it produced: PR #14940, opened through Claude Code after the ship-gate passed all required checks. The monitor then produced a run report and a wait note telling me not to rebase, push, or post yet.
- Difficulties: The initial setup required adjustments for Windows compatibility. The PR description also had to explain a plan deviation and a deliberately excluded behavior so reviewers would not mistake them for oversights. After opening, CI failed in jobs unrelated to my change. The monitor can't tell unrelated failures from real ones, so it labeled the branch stale.
- What changed: I established Windows-compatible validation commands, confirmed that the hooks block incomplete GitHub operations, and opened the PR only after the ship-gate passed. Before the run, I added a playbook rule to explain unrelated failures only once. Following the monitor's note, I checked all six failing job logs myself and recorded the exact job names here instead of posting a duplicate comment.

## Learnings and Reflections
Technical Skills Gained

This project helped me become more familiar with how an existing React application handles API errors and how automated tests verify the application's behavior.

I learned how different components can handle similar requests in different contexts and why an error notification should only report information that the application actually knows.

I also gained experience using GitHub issues to understand maintainers' expectations, locating relevant source files, interpreting test assertions, and working with a large repository instead of a small standalone programming assignment.

During the Unit 7 setup, I learned how timezone settings can affect JavaScript tests, how Windows line endings can affect formatting tools, and how shell compatibility affects npm scripts.

I also learned how Claude Code hooks can help enforce project requirements before potentially significant GitHub actions are performed. Repeated plan-check reviews taught me to turn maintainer feedback into exact assertions and to distinguish a notification wording change from an additional error-handling feature.

In Unit 8, I learned to read a PR's state the way GitHub reports it. A BEHIND merge state isn't a conflict, an empty review decision on a 4-day-old PR is normal, and a red check needs its log read before it means anything. I used the GitHub CLI and search API to measure real maintainer response times for this repository, which turned my nudge threshold into a number I can justify.

Challenges Overcome

One challenge was identifying where measurement errors were handled because the application retrieves measurements at both the report and individual-metric levels.

Another challenge was reproducing the issue in a meaningful way. The existing tests passed because they verified the current behavior rather than the requested improvement. I learned that passing tests do not necessarily mean a feature satisfies new requirements.

The largest setup challenge was running the repository's quality checks on Windows. My initial full test run failed because of timezone-sensitive date comparisons, and linting produced thousands of formatting errors caused by differences in line endings.

Instead of modifying unrelated code, I investigated the environment and adjusted the commands. This allowed me to establish a reliable baseline before making changes to the application.

During implementation, passing the metric and subject names into the fetching effect conflicted with an ESLint dependency rule. Following the rule would have changed when measurements are fetched, so I used a focused suppression and documented it as a deviation instead of silently changing behavior.

After opening the PR, 6 of 31 checks failed on dependency advisories and Docker Hub authentication. These failures had nothing to do with my four files, but they made the PR look broken. The challenge was to resist "fixing" them by bumping dependencies or pushing again. I explained them once in the thread. In Unit 8, when the PR monitor labeled the branch stale because of those red checks, I checked every failing job log myself before accepting its wait note.

What I'd Do Differently Next Time

Next time, I would inspect the repository's development requirements and operating-system assumptions before running its tooling. This would help me identify possible compatibility problems earlier.

I would also separate environment problems from actual code problems before attempting modifications.

For future open-source contributions, I want to establish a repeatable test baseline, write down the expected behavior, and make the smallest change that satisfies the issue.

Writing tests that failed against the original implementation before changing the code gave me confidence that the tests actually checked the requested behavior.

After opening the PR, I would read failing CI logs before writing the explanation comment and name each failing job and advisory ID in it. My November 2 comment described the failures in general terms. When the monitor's draft pointed that out, I had to go back to the logs on November 6 to confirm each job. I would also write the playbook before opening the PR, so my nudge threshold comes from the repository's real response times instead of how impatient I feel. As of November 6, I have no maintainer feedback to reflect on. Waiting quietly is the correct state for this PR, not a missing step.

## Resources Used
- Quality Time issue #12823 — The original issue and maintainer discussion defining the requested notification improvement.
- Quality Time repository — Source code and existing frontend tests used to investigate the issue.
- Quality Time CONTRIBUTING.md — Repository contribution guidance.
- CodePath AI301 Unit 7 starter — Ship-gate installation instructions and project-checking tools.
- CLAUDE.local.md — Local repository instructions prepared in Unit 6 to help Claude Code understand relevant files and conventions.
- Vitest, ESLint, Prettier, and Vite — Tools used to run the frontend test suite, check code quality, and verify the production build.
- Quality Time PR #14940 and its GitHub Actions logs — The PR thread, checks, and failing job logs (minimatch GHSA-g3hv-75wg-m249, glob GHSA-3g3m-2fjr-6pw4, pyasn1 advisories, Docker Hub authentication) that I used to compare the monitor's draft with the PR's real state.
- Quality Time merged PRs #14709, #13062, #12913, #12592, and #12507 — The five recent outside-contributor PRs I used to measure first-response times for my playbook's staleness thresholds.
- CodePath AI301 Unit 8 starter (skill/INSTALL.md, move-forms.md, playbook-template.md) — PR monitor installation, the move forms it drafts from, and the four-section playbook template.
- My Unit 6 voice guide — Its "Pings I send" rule was the starting point for my playbook's Nudge etiquette.
- GitHub CLI (gh pr view, gh pr checks, gh run view --log-failed, gh api search) — Used to read the PR state, CI results, and repository history.