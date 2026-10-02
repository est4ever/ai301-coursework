# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

est4ever

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5945889797

I reproduced Issue #69 and traced the crash to `rag/generator/output_parser.py`.

Reproduction evidence: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69#issuecomment-5842777727

`json.loads` can return a list, but `_parse_json_output` assumes a dictionary and calls `.items()`, causing `AttributeError: 'list' object has no attribute 'items'`.

I plan to preserve the existing object path, detect a top-level array before it reaches the dict-only parser, and route that case through the existing plaintext fallback. I will apply the same handling to fenced and raw JSON, remove the Issue #69 `xfail` marker once the regression test passes, and run the focused test, the full output-parser test file, the direct Unit 2 array reproduction, and a fenced-array case.

I do not plan to redesign the parser, change `FeedbackSection`, or refactor unrelated code. I will build this on `fix/69-json-array-fallback`.

---

## Your branch

**Branch**

fix/69-json-array-fallback

**Evidence**

Before:

```text
$ python -m pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q --runxfail
FAILED tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback - AttributeError: 'list' object has no attribute 'items'
1 failed
```

After:

```text
$ python -m pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q
1 passed
```

Fenced-array regression test:

```text
$ python -m pytest tests/unit/test_output_parser.py::TestOutputParser::test_fenced_json_array_fallback -q
1 passed
```

Full output-parser regression suite:

```text
$ python -m pytest tests/unit/test_output_parser.py -q
20 passed
```

Direct Unit 2 array reproduction after the fix:

```text
$ python -c 'import json; from rag.generator.output_parser import parse_review_output; print(parse_review_output(json.dumps(["First feedback item", "Second feedback item"])))'
[FeedbackSection(section_name='general_feedback', content='["First feedback item", "Second feedback item"]', confidence=0.7, suggestions=[])]
```

---

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Full Run 1: 18/20. This was also my submitted final full run.

The two disagreements were pkg-05 and pkg-20. pkg-05 was a clear-accept package that my rubric rejected on "Risks and unknowns are handled." pkg-20 was a thread-convention package that my rubric accepted.

The run matched every category at least once:
clear-accept 6/7, scope-creep 4/4, thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4.

Because this run already met the assignment target of 18/20 with every category matched, I kept the rubric rather than loosening checks and potentially flipping packages that were already correct.

**Package analysis**

Package: pkg-20

My rubric verdict: accept  
Gold verdict: reject

pkg-20 was in the thread-convention category. My rubric accepted it because the plan itself was technically actionable and matched its diagnosis and test approach, but my "Plan comment matches the plan and thread" check did not reject the specific convention problem strongly enough. This explains why the technical checks could all pass while the overall package still disagreed with the gold label.

I kept the check because it still correctly catches thread and repository-convention mismatches in other cases, and the final evaluation had at least one correct match in the thread-convention category.

**Check rationale**

| Test plan proves the fix | The plan's test plan read against the reproduction steps and expected behavior | The test plan re-runs the reproduced failure through the real affected code and states an observable expected result that distinguishes the fixed behavior from the original bug. | required |

I wrote this check this way because a plan saying only "run the tests" does not demonstrate that the reproduced bug will actually be tested. Requiring the original failure to be exercised through the affected code and requiring an observable post-fix result makes the plan distinguish between the broken and fixed behavior.

**Trade-offs**

This check is intentionally strict and can reject a plan that has a plausible implementation but an underspecified test plan. It favors observable regression evidence over flexibility. A developer might have a valid alternative test method that is not identical to the original reproduction, so the check could reject a good plan if that alternative is not explained clearly enough.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
