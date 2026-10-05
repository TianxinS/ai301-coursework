# Plan for #64: make the "partial overlap" fixture actually partial

## Diagnosis

The failing test's fixture does not contain a partial overlap, and the scorer is right to return 1.0 for it.

From my reproduction on #64 (commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`), running the test with the xfail marker ignored:

```
        query = "Python Django web framework"
        chunks = [
            {"text": "Django is a Python web framework for rapid development"},
        ]
    ...
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9
...
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
```

`RelevanceScorer.score` in `rag/evaluator/relevance_scorer.py` lowercases and whitespace-splits both texts, then scores each chunk as `len(query_tokens & chunk_tokens) / len(query_tokens)`. The query has 4 tokens (`python`, `django`, `web`, `framework`), and the fixture's chunk contains all 4, so the score is 4/4 = 1.0, matching `avg_score=1.0 ... query_len=4` in the repro output. The test's expectation (`0.3 < score < 0.9`) is what's wrong for this input, not the scorer. This is also what the issue says ("Fix the fixture so the overlap is genuinely partial").

In the default run, the failure is hidden: the test is marked `@pytest.mark.xfail(strict=True, reason="issue #64: ...")`, so `pytest tests/unit/test_relevance_scorer.py -q` reports `18 passed, 1 xfailed`.

## Scope

In scope:
- Change the chunk text in the `test_query_with_partial_overlap` fixture so it contains exactly 2 of the 4 query tokens.
- Remove the `@pytest.mark.xfail(strict=True, ...)` decorator from that test. With `strict=True`, a test that now passes would be reported as an unexpected pass and fail the run, so the marker has to go with the fixture fix.

Not in scope:
- `rag/evaluator/relevance_scorer.py`: no change. The scorer's behavior is correct for this input.
- The query string and the test's assertions: unchanged, so the test still checks what its docstring says ("partial overlap returns score between 0 and 1", in the 0.3–0.9 range).
- Any other test in the file. (I noticed `test_query_with_perfect_keyword_match` uses "frameworks" in its chunk against "framework" in its query, which this tokenizer doesn't match; that's a separate question and I'm not touching it here.)

## Files to touch

- `tests/unit/test_relevance_scorer.py` only.

Branch: `fix/64-partial-overlap-fixture` in my fork, following the repo's `fix/<issue-number>-<slug>` convention. plan.md and comment.md stay out of the commits.

## Approach

1. In `tests/unit/test_relevance_scorer.py`, delete the `@pytest.mark.xfail(strict=True, reason="issue #64: relevance scorer 'partial overlap' fixture actually has full overlap")` decorator above `test_query_with_partial_overlap`.
2. In the same test, replace the chunk text `"Django is a Python web framework for rapid development"` with `"Flask is a lightweight Python web toolkit"`. Its lowercase whitespace tokens include `python` and `web` but not `django` or `framework`, so the expected score is 2/4 = 0.5. The chunk has no punctuation attached to the matching words, since the tokenizer doesn't strip it.
3. Leave the query, the three assertions, and the docstring unchanged.

## Test plan

Re-run my #64 reproduction steps against the change, in the same environment (macOS 26.5.1, Python 3.11.16, pytest 9.1.1, `make setup` completed):

1. Score check through the real scorer, before the test change (already run while planning):
   `.venv/bin/python -c "from rag.evaluator.relevance_scorer import RelevanceScorer; print(RelevanceScorer().score('Python Django web framework', [{'text': 'Flask is a lightweight Python web toolkit'}]))"`
   Observed:
   ```
   2026-10-05 15:06:26 [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
   0.5
   ```
   0.5 is inside the test's 0.3–0.9 range.
2. The issue's command, repro step 6:
   `.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -v -rxX`
   Before (from my repro): `test_query_with_partial_overlap XFAIL` and `18 passed, 1 xfailed`.
   Expected after: `test_query_with_partial_overlap PASSED` and `19 passed`, with no XFAIL line.
3. Repro step 7, the run that showed the real failure:
   `.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -k partial_overlap --runxfail -q`
   Before: `FAILED ... assert 1.0 < 0.9`. Expected after: `1 passed`.
4. `make check && make test-unit`, which CONTRIBUTING requires to pass, to confirm lint, formatting, and type checks are clean and no other unit test changes result.

## Risks and unknowns

- I haven't checked whether anything outside this file refers to the xfail reason string or counts xfailed tests (e.g. CI config that expects a specific xfail count). I'll grep the repo for "issue #64" and "xfail" before committing.
- The new score (0.5) depends on the current tokenizer (lowercase + whitespace split). If the tokenizer later gains stemming or stopword removal, this fixture's score could move; at 0.5 it has room on both sides of the 0.3–0.9 range.

## Deviations

One change from the plan. I made the two edits the plan describes in `tests/unit/test_relevance_scorer.py` (deleted the `xfail(strict=True)` decorator and replaced the chunk text with "Flask is a lightweight Python web toolkit"), but the first commit attempt was blocked by the repo's pre-commit mypy hook:

```
tests/unit/test_relevance_scorer.py:59: error: Need type annotation for "chunks" (hint: "chunks: List[<type>] = ...")  [var-annotated]
```

Line 59 is `chunks = []` in `test_empty_chunks_list_returns_zero`, the next test in the file, which my change doesn't touch. The error was already there; it only shows up when this file is committed, because the hook runs mypy on the files being committed, while `make check` runs mypy only on `api/ core/ ingestion/ rag/ agent/ safety/`, not `tests/`. My plan said I wouldn't change any other test, but the file can't be committed under the repo's own hook without fixing this line. So I added a type annotation only (`chunks: list[dict] = []`), with no change to what the test does, and the commit then passed ruff, black, and mypy. Still one file touched; no other test changed.

The risk I said I'd check before committing turned out to be a non-issue: grepping the repo for "issue #64" and "xfail" found "issue #64" only in this test's own marker. The other xfail markers belong to other seeded bugs in other test files. A comment in `pyproject.toml` says fixing a seeded bug "removes the test's xfail marker" and, for bugs mypy catches, their mypy override entry; this bug is a test fixture, not a type error, so there was no override entry to remove.

The test plan ran as expected: the issue's command went from `18 passed, 1 xfailed` to `19 passed`, the `--runxfail` run went from `assert 1.0 < 0.9` to `1 passed`, and `make check` and `make test-unit` (376 passed, 52 xfailed) both passed.

## Deviations Comment

Update on my plan above: one small change during the build.

The fixture fix and the xfail removal went in as planned, but committing `tests/unit/test_relevance_scorer.py` was blocked by the repo's pre-commit mypy hook on a line my change doesn't touch:

```
tests/unit/test_relevance_scorer.py:59: error: Need type annotation for "chunks" (hint: "chunks: List[<type>] = ...")  [var-annotated]
```

That's `chunks = []` in `test_empty_chunks_list_returns_zero`. It was already there; it only shows up when this file is committed, since `make check` doesn't run mypy on `tests/`. My plan said I wouldn't touch other tests, so I'm flagging that I added a type annotation there (`chunks: list[dict] = []`) to get the commit through the hook. It doesn't change what that test does.

Results after the change: `pytest tests/unit/test_relevance_scorer.py` gives `19 passed` (was `18 passed, 1 xfailed`), the `--runxfail` run gives `1 passed` (was `assert 1.0 < 0.9`), and `make check` and `make test-unit` pass.
