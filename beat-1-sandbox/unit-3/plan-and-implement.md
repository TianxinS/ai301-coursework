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

TianxinS

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-6004283498

Plan for #64, building on my reproduction above.

The scorer isn't the problem: it scores a chunk as (query words found in it) ÷ (query words), and the "partial overlap" fixture's chunk contains all four words of "Python Django web framework", so 1.0 is the correct score. The test's own fixture is what's wrong, as the issue says.

What I'll change, all in `tests/unit/test_relevance_scorer.py`:
- Replace the chunk text in `test_query_with_partial_overlap` with "Flask is a lightweight Python web toolkit", which shares exactly 2 of the 4 query words, so it should score 0.5 (inside the test's 0.3–0.9 range).
- Remove the `xfail(strict=True)` marker referencing this issue, since a strict xfail on a test that now passes would fail the run.

Not changing: the scorer, the query, the assertions, or any other test.

I've read the two plans already posted here: @srithimahi's 3-of-4 chunk (0.75) and @Cael-Pairrett's 2-of-4 chunk (0.5). I worked out my chunk from the scorer's formula on my own repro, and landed on 2 of 4 like Cael-Pairrett because 0.5 sits in the middle of the test's 0.3–0.9 range, leaving room on both sides if the tokenizer ever changes. Running the new chunk through the real scorer:

```
$ .venv/bin/python -c "from rag.evaluator.relevance_scorer import RelevanceScorer; print(RelevanceScorer().score('Python Django web framework', [{'text': 'Flask is a lightweight Python web toolkit'}]))"
2026-10-05 15:06:26 [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
0.5
```

How I'll check it: re-run my repro commands. The issue's command should go from `18 passed, 1 xfailed` to `19 passed`, the `--runxfail` run from `assert 1.0 < 0.9` to `1 passed`, and `make check && make test-unit` (which CONTRIBUTING requires) should pass. One thing I'll check before committing: whether anything else in the repo depends on this test being xfailed.

I'm using an AI assistant to help organize my plan and comments; I've read the scorer and test code myself and will run every check above.

---

## Your branch

**Branch**

fix/64-partial-overlap-fixture

**Evidence**

Environment for both runs: macOS 26.5.1 (Apple Silicon), Python 3.11.16, pytest 9.1.1, fork of codepath/pathreview-ai301-fa26-s1. Before = commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`, as posted in my Unit 2 repro comment. After = branch `fix/64-partial-overlap-fixture`, with the two-line change to `tests/unit/test_relevance_scorer.py`.

**Repro step 6, the issue's command**

```
$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -v -rxX
```

Before:
```
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap XFAIL (issue #64: ...) [ 15%]
XFAIL tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - issue #64: relevance scorer 'partial overlap' fixture actually has full overlap
18 passed, 1 xfailed in 0.16s
```

After:
```
============================= test session starts ==============================
platform darwin -- Python 3.11.16, pytest-9.1.1, pluggy-1.6.0 -- /Users/tianxinsong/Documents/tianxin/codepath/pathreview-ai301-fa26-s1/.venv/bin/python
cachedir: .pytest_cache
hypothesis profile 'default'
benchmark: 5.3.0 (defaults: timer=time.perf_counter disable_gc=False min_rounds=5 min_time=0.000005 max_time=1.0 calibration_precision=10 warmup=False warmup_iterations=100000)
rootdir: /Users/tianxinsong/Documents/tianxin/codepath/pathreview-ai301-fa26-s1
configfile: pyproject.toml
plugins: hypothesis-6.168.3, cov-7.1.0, asyncio-1.4.0, benchmark-5.3.0, pytest_httpserver-1.1.5, platformdirs-4.12.1, anyio-4.15.1
asyncio: mode=Mode.STRICT, debug=False, asyncio_default_fixture_loop_scope=None, asyncio_default_test_loop_scope=function
collecting ... collected 19 items

tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_perfect_keyword_match PASSED [  5%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_zero_keyword_overlap PASSED [ 10%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap PASSED [ 15%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_empty_chunks_list_returns_zero PASSED [ 21%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_empty_query_returns_zero PASSED [ 26%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_multiple_chunks_aggregated PASSED [ 31%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_case_insensitive_matching PASSED [ 36%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_tokenization PASSED [ 42%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_empty_text_in_chunk PASSED [ 47%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_very_long_query PASSED [ 52%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_very_long_chunk PASSED [ 57%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_special_characters_ignored PASSED [ 63%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_multiple_keyword_matches PASSED [ 68%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_single_word_chunks PASSED [ 73%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_score_ranges_from_zero_to_one PASSED [ 78%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_common_words_not_preventing_scoring PASSED [ 84%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_chunk_without_text_key PASSED [ 89%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_whitespace_only_in_chunks PASSED [ 94%]
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_average_relevance_calculation PASSED [100%]

============================== 19 passed in 0.12s ==============================
```

**Repro step 7, with the xfail marker ignored**

```
$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -k partial_overlap --runxfail -q
```

Before:
```
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
------------------------------------------------- Captured stdout call -------------------------------------------------
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - assert 1.0 < 0.9
1 failed, 18 deselected in 0.14s
```

After:
```
.                                                                        [100%]
1 passed, 18 deselected in 0.10s
```

**The new fixture through the real scorer** (run while planning)

```
$ .venv/bin/python -c "from rag.evaluator.relevance_scorer import RelevanceScorer; print(RelevanceScorer().score('Python Django web framework', [{'text': 'Flask is a lightweight Python web toolkit'}]))"
2026-10-05 15:06:26 [info     ] relevance_scored               avg_score=0.5 chunks_count=1 query_len=4
0.5
```

**Full unit suite after the change** (last lines)

```
$ make test-unit
  Enable tracemalloc to get traceback where the object was allocated.
  See https://docs.pytest.org/en/stable/how-to/capture-warnings.html#resource-warnings for more info.

-- Docs: https://docs.pytest.org/en/stable/how-to/capture-warnings.html
================= 376 passed, 52 xfailed, 5 warnings in 4.95s ==================
```

The 52 xfailed are other seeded bugs' tests, each with its own marker; `test_query_with_partial_overlap` now counts as a normal pass. `make check` (ruff, black, mypy) also passed on the change.

## Eval iterations

**Run history**

1. Full run 1: **17/20**, below the bar. Categories: clear-accept 5/7, scope-creep 4/4, thread-convention 1/2, unbuildable 3/3, wrong-cause 4/4. Disagreements: pkg-02 (gold accept, my verdict reject, "failed: scope-bounded"), pkg-14 (gold accept, my verdict reject, "failed: scope-bounded, plan-executable"), pkg-20 (gold reject, my verdict accept).
2. Full run 2: **20/20**, PASS. Categories: clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause 4/4. This run is the committed `eval-run.txt`.

Between runs 1 and 2, I revised three checks: loosened `scope-bounded` (same-defect sites named by the issue or plan count as part of the fix; an audit that only reports isn't a change), loosened `plan-executable` (a component plus a concrete method to pin the exact function is an acceptable location, as long as the edit itself is decided), and tightened `comment-fits-thread` (a policy requiring disclosure of any AI usage fails a comment with no disclosure; a stated gate such as a vouch flow must be acknowledged).

Before run 1, I also graded the two calibration packages, which are never scored: calib-01 accept and calib-03 reject, both matching the staff verdicts.

**Package analysis**

**pkg-20 (ghostty-org/ghostty#11261).** Gold: **reject**. My rubric in run 1: **accept**. My rubric in run 2: **reject**.

The plan itself is strong. Its diagnosis matches the repro (the stale `prev` pointer after page growth, with a no-hyperlink control that "passes, isolating the growth-during-print as the trigger"), it follows the maintainer's direction to recompute `prev` "only if the underlying page capacity changed", and it states its unmeasured risk. The comment also lines up with the plan and the thread.

What's wrong is the comment against the repo's stated rules. The Repo facts say: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance", and that "first-time contributors go through a vouch flow before PRs are accepted". The comment has no disclosure statement at all, and it says "I'll run the #11249 corpus against the build before the PR" without acknowledging the vouch flow.

In run 1, my `comment-fits-thread` check required disclosure only "where the stated policy requires it". The grader read that as "only if the author used AI", found no evidence of AI use, and passed the comment. Nothing in the check covered a gate on the PR. So the rubric graded the comment's fit with the thread and missed its fit with the repo's rules. I changed the check so that when the stated policy requires disclosure of any AI usage, a comment with no disclosure fails ("because the grader cannot know AI was not used"), and so that a comment announcing a gated next step has to acknowledge the gate. In run 2, pkg-20 was rejected on this check and pkg-04, the other thread-convention package, stayed correctly rejected.

**Check rationale**

> | comment-fits-thread | The plan comment, read against the thread highlights, the plan itself, and the repo-facts block (contribution policy, AI-use policy, stated conventions) | The comment describes the same plan as the plan file; is in the author's own words (not "same approach as above" or a restatement of another commenter's plan); does not ignore or contradict a direction a maintainer gave in the thread; and follows the conventions the repo-facts block states. If the stated policy requires disclosure of any AI usage, the comment must contain a disclosure statement (the tool and extent of assistance, or a statement that none was used); a comment with no disclosure fails, because the grader cannot know AI was not used. If the repo facts state a gate on the next step the comment announces (e.g. a vouch or approval flow before PRs are accepted, an issue-first rule), the comment must acknowledge it rather than announce the PR as if the gate did not exist. A convention that is only linked and whose text is not shown is not held against the comment. | required |

It reads this way because of pkg-20. My first version said disclosure was needed only "where the stated policy requires it", which let the grader decide that no AI had been used and pass a comment with no disclosure on a repo whose policy covers "all AI usage in any form". A reader can't tell from the comment whether AI was used, so the only checkable rule is: if the policy requires disclosure of any AI use, the comment has to say something about it, either what was used or that nothing was. I added the gate clause for the same package, because its comment announced a PR on a repo with a vouch flow. I kept the last sentence from my unit 2 rubric on purpose: in unit 2, pkg-07 was held because a policy's details sat in a linked file the package didn't show, and I didn't want this check to repeat that false reject.

**Trade-offs**

The disclosure clause gives up leniency toward authors who really didn't use AI. On a repo whose policy requires disclosure of any AI use, a human-only comment that says nothing about AI now fails, even though it broke no rule. I accept that, because the grader can't tell the two apart and a short "no AI used" line costs the author nothing.

The two checks I loosened in the same revision could have let scope creep or unbuildable plans through, so I confirmed them on the full run 2 rather than a partial run: scope-creep stayed 4/4 and unbuildable stayed 3/3, so neither loosening flipped a package that was correct before.

One thing the check does not cover: house rules from the skill's `scope.md`, such as the `fix/<issue-number>-<slug>` branch name. My procedure doesn't read `scope.md`, so the live plan-check run on my own plan pointed that gap out. I left the procedure unchanged to keep it matching the committed eval run, and named the branch in my plan by hand instead.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
