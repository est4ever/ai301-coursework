# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69

**Verdict output**


Mode: live. Candidate is in codepath/pathreview-ai301-fa26-s3, the scoped repo. Path Review house rule noted (classmate claims don't block) — moot here, the thread is empty.

Issue #69 — "Output parser crashes on a top-level JSON array fallback"

- maintainer-active — pass. Aburke225 (COLLABORATOR) pushed 3 commits to main on 2026-09-16 and left closing comments on issues #52 and #43 the same day — 3 days before today.
- repo-active — pass. pushed_at 2026-09-16, not archived, 71 open issues; repo created 2026-09-10 and actively developed since.
- newcomer-scope — pass. Concrete defect with a named exception (AttributeError: 'list' object has no attribute 'items'), two named files, and an explicit completion condition (remove the @pytest.mark.xfail for manifest id H-02). Zero comments, so no unresolved design debate or abandoned attempt; labeled good first issue, estimated 2-4 hours.
- unclaimed — pass. Assignees: none. Timeline shows only the four labeling events from 2026-09-10 — no assignment, no cross-referenced PR. PR search for "69" in the repo returns total_count: 0. No comments at all.

All four required checks pass → accept. It also lands squarely on your fit profile: Python, a pytest-covered bug in a RAG generator's output parser — AI/ML-adjacent, clearly scoped, no low-level systems work or architectural change.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
  "checks": [
    {
      "name": "maintainer-active",
      "grade": "pass",
      "evidence": "Collaborator Aburke225 pushed to main on 2026-09-16 and commented on issues #52/#43 the same day (3 days before today, 2026-09-19)."
    },
    {
      "name": "repo-active",
      "grade": "pass",
      "evidence": "Repo pushed_at 2026-09-16, archived: false, 71 open issues — activity well within the past 6 months."
    },
    {
      "name": "newcomer-scope",
      "grade": "pass",
      "evidence": "Body names the exact defect ('AttributeError: list object has no attribute items'), the two files, and the done-condition (remove the xfail for H-02); 0 comments, so no open design question."
    },
    {
      "name": "unclaimed",
      "grade": "pass",
      "evidence": "assignees: none; timeline has only 4 label events from 2026-09-10, no linked/cross-referenced PR; PR search for '69' returns total_count 0; 0 comments."
    }
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

`agreement: 18/20 scored items  (bar: 18/20: PASS)`

This was my complete evaluation run. The rubric matched the gold labels on 18 of the 20 scored issues and passed the required bar.

**Issue analysis**

`issue-01  accept  reject   NO     failed: newcomer-scope`

For `issue-01`, my rubric returned `reject`, while the gold label was `accept`. The rejection came from the `newcomer-scope` check. My rubric requires the issue to have a concrete desired outcome or identifiable defect and to let a contributor begin without first resolving an open product, design, or API decision. Based on the evidence presented for this issue, the check treated its scope as unsuitable for a first contribution, even though the gold label considered it acceptable.

**Check rationale**

`| newcomer-scope | Issue body, labels, and comment thread, including unresolved design questions and prior implementation attempts | Pass if the issue states a concrete desired outcome or identifiable defect and a contributor can begin work without first resolving an open product, design, or API decision. Do not fail only because the task spans several files, lacks step-by-step instructions, or involves performance or concurrency. Fail if the thread shows unresolved design/API behavior, substantial abandoned implementation attempts, or that the requested behavior is still being negotiated. | required |`

I wrote this check to distinguish an issue that is merely technically challenging from one whose requested behavior has not yet been decided. I did not want file count, lack of detailed instructions, performance work, or concurrency alone to cause rejection. Instead, the check focuses on whether a newcomer can start implementing a defined result without first making a product, design, or API decision.

**Trade-offs**

`issue-01  accept  reject   NO     failed: newcomer-scope`

`issue-19  accept  reject   NO     failed: newcomer-scope`

This check gives up some recall in exchange for being conservative about ambiguous scope. In the final evaluation, it changed the result of `issue-01` and `issue-19`: both had gold labels of `accept`, but my rubric rejected them because of `newcomer-scope`. I accept that this rule can reject some issues that are considered reasonable first contributions when their scope looks more open-ended under my threshold.

---

## Selection rationale

**Selection rationale**

1. This issue fits my interests because it involves Python and a parser in a RAG-related project, which is related to my interests in AI/ML. 

2. The verdict correctly identified that the issue has a specific error, named files, a clear completion condition, and no current assignee or linked pull request. Beyond the rubric, I also considered whether the type of debugging involved matched my Python experience and whether I could understand the affected code quickly.

3. I expect claiming the issue to be fairly manageable because it is currently unassigned and has no active pull request. The main challenge will likely be reproducing the JSON array failure and understanding why the parser expects an object instead of a list.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
