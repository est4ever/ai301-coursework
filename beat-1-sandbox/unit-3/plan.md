# Plan for Issue #69

## Diagnosis

The output parser crashes on valid JSON whose top-level value is an array. My Unit 2 reproduction showed:

> AttributeError: 'list' object has no attribute 'items'

`json.loads` returns a Python list, but `_parse_json_output` assumes a dictionary and calls `data.items()`. Both the fenced-JSON and raw-JSON paths can pass decoded JSON to `_parse_json_output`.

## Scope

### In scope
- Update `rag/generator/output_parser.py` so a top-level array does not reach the dict-only `.items()` logic.
- Apply the same handling to fenced JSON and raw JSON.
- Preserve existing behavior for top-level JSON objects.
- Remove the Issue #69 `xfail` marker from `tests/unit/test_output_parser.py` after the fix.
- Run the focused regression test and the full output-parser test file.

### Out of scope
- Redesigning the parser.
- Changing `FeedbackSection`.
- Refactoring unrelated code.
- Inventing new structured semantics for array elements.

## Files to Change

- `rag/generator/output_parser.py`
- `tests/unit/test_output_parser.py`

## Branch

`fix/69-json-array-fallback`

## Approach

1. Check the value returned by each `json.loads` call in `parse_review_output`.
2. Continue sending dictionaries to `_parse_json_output`.
3. Detect a top-level list before it reaches `_parse_json_output` and route it through the existing plaintext fallback.
4. Apply this to both fenced-JSON and raw-JSON paths.
5. Remove the `xfail` marker from `test_json_array_fallback`.
6. Run the focused and full parser tests.

## Test Plan

Before the fix:

`python -m pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q --runxfail`

Expected: `AttributeError: 'list' object has no attribute 'items'`.

After the fix:

`python -m pytest tests/unit/test_output_parser.py::TestOutputParser::test_json_array_fallback -q`

Expected: the test passes and `parse_review_output` returns a list without crashing.

Then run:

`python -m pytest tests/unit/test_output_parser.py -q`

Expected: the complete output-parser test file passes, including the former Issue #69 regression test.

Also rerun the direct Unit 2 array input `json.dumps(["First feedback item", "Second feedback item"])` and verify it no longer crashes. I will also test a fenced top-level JSON array.

## Risks and Unknowns

- Issue #69 requires arrays not to crash but does not define new semantics for each array element, so I will use the existing plaintext fallback unless the tests require otherwise.
- The existing regression test covers raw JSON, so I will separately verify the fenced-JSON path.
- Other non-dictionary JSON values may also be incompatible with `.items()`. I will keep the change bounded to Issue #69 unless the smallest safe type guard naturally covers them without changing existing behavior.

## Deviations

The implementation followed the posted plan. No material deviations were needed. The parser now routes top-level JSON arrays through the existing plaintext fallback in both fenced and raw JSON paths, and I added the planned fenced-array regression coverage.
