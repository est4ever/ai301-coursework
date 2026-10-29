Contribution: Improve toaster message on network connection
Contribution Number: [1]

Student: Jesse Lin

GitHub Username: est4ever

Issue: https://github.com/ICTU/quality-time/issues/12823

Pull Request: Not opened yet

One repository, built across units 5 to 8, telling the whole story of my real open-source contribution.
Toolkit
Unit 5: Built and improved an issue-scout tool for GitHub issues

I upgraded my issue-scout tool to search for, rank, and evaluate open-source GitHub issues using a grading rubric. My initial scan returned 200 issues, but the ranking favored newer repositories with limited discussion, so I adjusted my search strategy. I also found that the original scope check was too strict for real issues, so I replaced it with scope-feasible and added difficulty notes. After regrading my candidates and checking for existing claims or related pull requests, I selected Quality Time #12823 as a manageable contribution.

Unit 6: Installed a voice guard and created local repository memory

I installed a voice guard to check my GitHub comments against my writing rules before posting them. I also created a CLAUDE.local.md file for the Quality Time repository, documenting its contribution conventions, relevant frontend files, and test commands. The initial template lacked repository-specific information, so I used Claude Code to inspect the measurement-fetching subsystem and added those findings to the memory file. I excluded the file using .git/info/exclude and verified that Git ignored it.

Unit 7: Configured a ship-gate and passed plan-check for issue #12823

I installed the Unit 7 ship-gate and created a manifest for the frontend tests, lint checks, and build. Windows timezone, shell, and CRLF line-ending differences initially caused failures, so I adjusted the commands without modifying unrelated files. I verified the ship-gate and voice guard independently, then used plan-check to review my proposed fix against the maintainer's comments. After several revisions to the notification wording, test expectations, and scope, plan-check returned accept with all six checks passing; the full branch ship-gate has not been run yet.

Unit 8: Pending — PR monitoring and maintainer feedback

To be completed during Unit 8 after opening the pull request.
Why I Chose This Issue
I initially considered contributing to NVDA, but after evaluating different issues, I decided to look for something more manageable. I used my issue-scout to directly grade three candidates: Quality Time #12823, simpler-grants-gov #12056, and Red-DiscordBot #6794. Quality Time ranked as the best fit because the maintainer had already explained the requested change, the repository was active, and nobody had claimed the issue when I evaluated it.

My issue-scout evaluated this issue on October 22, 2026, and gave it an accept verdict with a moderate difficulty rating. It passed all four checks: maintainer-active, repo-active, scope-feasible, and unclaimed. The scout considered it moderate because the maintainer had already specified the desired wording, but I still needed to locate the relevant error-handling code. It also noted that identifying a specific metric or subject would be difficult because some measurement requests retrieve data for all metrics at once.

I chose this issue because I wanted more experience working with real software projects, especially debugging existing code and improving how applications handle errors. Even though I have programming experience, I haven't worked much with React or large open-source frontend projects. This issue gives me an opportunity to understand how API requests, error notifications, and automated tests work together. I also learned that my original issue-scout rubric was too strict for real-world GitHub issues, so I updated it to better evaluate whether an issue was manageable.

Claim Comment: https://github.com/ICTU/quality-time/issues/12823#issuecomment-6333904364

Toolkit
- Tool: issue-scout, which I used to search for, evaluate, and compare potential GitHub issues.
- What it produced: A set of graded issues, including Quality Time #12823, that helped me decide which contribution to pursue.
- Difficulties: The original scope check was too strict for real GitHub issues. I also found that the unclaimed check could miss signs of ongoing internal work mentioned in previously closed pull requests.
- What changed: I replaced the original scope check with scope-feasible, added difficulty notes, and recorded the limitation of the unclaimed check for future improvements.
Understanding the Issue
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
Reproduction Process
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
Solution Approach
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

No source-code implementation deviations have occurred yet. The planning draft was revised after feedback to make the additional PageContent.jsx HTTP failure path conditional on maintainer approval. Any future implementation deviation will be recorded here and in plan.md.

Toolkit
- Tool: Claude Code plan-check, used to grade plan.md and plan-comment.md against the issue discussion, repository code, reproduction evidence, and the course rubric.
- What it produced: An approved implementation plan. The latest plan-check returned accept, with all six required checks passing.
- Difficulties: The first draft did not follow the April 15 maintainer wording or give precise expected test results. Later drafts conflicted about whether a new report-wide ok === false notification was included in scope, causing repeated rejections.
- What changed: I used the maintainer's requested metric-specific wording, specified concrete tests for rejected requests and HTTP failures, and made the new report-wide HTTP error path and Test Case D conditional on explicit maintainer approval. I kept plan-comment.md as an unposted draft.
Testing Strategy
My testing strategy combines reproducing the existing behavior, verifying the new notification requirements, running the frontend test suite, checking code quality, and building the application.

1. Baseline Reproduction

Before implementing the fix, I ran the existing tests for PageContent.jsx and MetricDetails.jsx. All 29 tests passed, confirming that the existing error messages matched the current expectations.

These tests establish the baseline behavior. They do not prove that the maintainer's requested improvement has been implemented.

2. Focused Tests for the Fix

I will update or add tests to verify that:
- A report-wide measurement failure produces a clear message without falsely identifying an individual metric.
- An individual-metric failure includes available identifying context when appropriate.
- An unsuccessful individual-metric HTTP response displays useful status information. A corresponding report-wide HTTP test is conditional on maintainer approval.
- A rejected network request without an HTTP response is handled without assuming that an HTTP status exists.
- The application continues displaying an error notification with the correct severity.
I will compare the notifications before and after the fix and record the exact results.

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

I will rerun the full test suite after implementing the fix and compare the results.

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

I will rerun these checks after implementing the fix.

5. Production Build

I ran:

npm --prefix components/frontend run build

The Vite build completed successfully and reported 2,003 modules transformed.

Vite displayed a warning that some generated chunks exceeded 500 kB after minification. This warning did not prevent the build from succeeding.

6. Final Verification

After implementing the fix, I will save the following in test_evidence.md:
- The original notification behavior and the updated behavior.
- The reproduction commands and their actual results.
- Focused test output.
- Full frontend test-suite results.
- Lint and formatting results.
- Production build results.
The final results will be recorded here after the implementation is complete.

Toolkit
- Tool: Vitest, ESLint, Prettier, npm, and Git Bash, used to establish baseline test and quality-check results. The ship-gate was installed to automate these checks before opening the PR.
- What it produced: A baseline of 1,108 passing tests, a successful frontend build, and working Windows-compatible lint and formatting checks.
- Difficulties: The initial test suite had timezone-dependent failures, while ESLint and Prettier encountered shell and line-ending compatibility problems on Windows.
- What changed: I used UTC for date-sensitive tests and separated linting from formatting with an automatic line-ending setting. This allowed verification without modifying unrelated source files.
Implementation Notes
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

Implementation of issue #12823 — Pending

The actual source-code modifications, new tests, and implementation commits will be documented here after they are completed.

Toolkit
- Tool: Claude Code, Git, npm, Vitest, ESLint, Prettier, and the Unit 7 ship-gate installer.
- What it produced: A working local contribution environment, validated baseline checks, functioning GitHub safeguards, and an accepted implementation plan for issue #12823.
- Difficulties: Git Bash required different Windows path and Python commands, and timezone and line-ending behavior disrupted tests and linting. Plan-check also rejected several drafts because the wording, tests, and scope did not fully agree with the maintainer's direction.
- What changed: I adapted the commands without rewriting unrelated files, revised the plan to include exact expected messages and tests, and made extra report-wide HTTP error handling conditional. Plan-check finally returned accept with six passing checks.
Pull Request
PR Link: Not opened yet.

PR Description:

The planned pull request will improve the error notifications displayed when Quality Time cannot retrieve measurements.

The changes will focus on the measurement-fetch error-handling logic in PageContent.jsx and MetricDetails.jsx, together with their relevant tests.

The description will include the actual implementation, before-and-after notification behavior, evidence from the tests, and a statement describing how AI tools contributed to the work.

The PR will reference issue #12823 using:

Fixes #12823

This section will be updated with the final PR description after implementation and validation.

Maintainer Feedback:
- October 28, 2026: No pull request has been opened yet, so no PR review feedback has been received.
- Pending: Record any maintainer comments and responses after opening the PR.
Status: Not opened yet.

Toolkit
- Tool: Claude Code, the voice guard, ship-gate, and GitHub CLI, prepared for use during PR creation.
- What it produced: The ship-gate and voice guard were verified independently. A final PR draft and successful branch validation are still pending.
- Difficulties: The initial setup required adjustments for Windows compatibility. No actual PR submission has occurred yet.
- What changed: I established the validation commands and confirmed that the hooks can block incomplete GitHub operations. The final PR creation results will be documented after the pull request is opened.
Learnings and Reflections
Technical Skills Gained

This project helped me become more familiar with how an existing React application handles API errors and how automated tests verify the application's behavior.

I learned how different components can handle similar requests in different contexts and why an error notification should only report information that the application actually knows.

I also gained experience using GitHub issues to understand maintainers' expectations, locating relevant source files, interpreting test assertions, and working with a large repository instead of a small standalone programming assignment.

During the Unit 7 setup, I learned how timezone settings can affect JavaScript tests, how Windows line endings can affect formatting tools, and how shell compatibility affects npm scripts.

I also learned how Claude Code hooks can help enforce project requirements before potentially significant GitHub actions are performed. Repeated plan-check reviews taught me to turn maintainer feedback into exact assertions and to distinguish a notification wording change from an additional error-handling feature.

Challenges Overcome

One challenge was identifying where measurement errors were handled because the application retrieves measurements at both the report and individual-metric levels.

Another challenge was reproducing the issue in a meaningful way. The existing tests passed because they verified the current behavior rather than the requested improvement. I learned that passing tests do not necessarily mean a feature satisfies new requirements.

The largest setup challenge was running the repository's quality checks on Windows. My initial full test run failed because of timezone-sensitive date comparisons, and linting produced thousands of formatting errors caused by differences in line endings.

Instead of modifying unrelated code, I investigated the environment and adjusted the commands. This allowed me to establish a reliable baseline before making changes to the application.

What I'd Do Differently Next Time

Next time, I would inspect the repository's development requirements and operating-system assumptions before running its tooling. This would help me identify possible compatibility problems earlier.

I would also separate environment problems from actual code problems before attempting modifications.

For future open-source contributions, I want to establish a repeatable test baseline, write down the expected behavior, and make the smallest change that satisfies the issue.

I will update these reflections after implementing the fix and receiving any maintainer feedback.
Resources Used
- Quality Time issue #12823 — The original issue and maintainer discussion defining the requested notification improvement.
- Quality Time repository — Source code and existing frontend tests used to investigate the issue.
- Quality Time CONTRIBUTING.md — Repository contribution guidance.
- CodePath AI301 Unit 7 starter — Ship-gate installation instructions and project-checking tools.
- CLAUDE.local.md — Local repository instructions prepared in Unit 6 to help Claude Code understand relevant files and conventions.
- Vitest, ESLint, Prettier, and Vite — Tools used to run the frontend test suite, check code quality, and verify the production build.