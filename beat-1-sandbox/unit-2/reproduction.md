# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

### GitHub username

TianxinS

## Posted upstream

### Claim comment

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5881547892

Hi! I'd like to take this one on as a first contribution.

The issue reports that `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py` uses the query "Python Django web framework" against a chunk that contains all four query terms, then asserts the score is below 0.9. According to the issue, the scorer returns 1.0 for full keyword coverage, so running `pytest tests/unit/test_relevance_scorer.py -q` fails with `assert 1.0 < 0.9`. I haven't run it yet.

My next step is to set up the repo from a fresh clone following the README, run that command, and confirm whether I see the same failure. I'll post a reproduction report here with my environment, the exact steps, and the output I get, including if I can't reproduce it.

### Reproduction comment

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64#issuecomment-5882177252

Reproduction report for #64. Reproduced, with one difference from the issue's command noted below.

**Environment**
- macOS 26.5.1 (build 25F80), Apple Silicon (arm64)
- Python 3.11.16 (Homebrew `python@3.11`), pytest 9.1.1
- git 2.50.1, Docker 29.8.1, Docker Compose v5.5.1, Node v26.10.0
- Repo: fork of codepath/pathreview-ai301-fa26-s1 at commit `f89c06fc3ff292df2a04a39ac51319d32a76b779`

**Steps** (from a fresh clone, following docs/SETUP.md)
1. `git clone https://github.com/TianxinS/pathreview-ai301-fa26-s1.git` and `cd pathreview-ai301-fa26-s1`
2. macOS only ships Python 3.9 as `python3`, and the Makefile's setup target runs `python -m venv .venv || python3 -m venv .venv`, so I put Homebrew's Python 3.11 first on PATH: `export PATH="$(brew --prefix python@3.11)/libexec/bin:$PATH"` (`python --version` → 3.11.16)
3. `cp .env.example .env` (no API key set)
4. `docker compose up -d`, then `docker compose ps` to confirm postgres and redis were up
5. `make setup` (finished with "Setup complete")
6. `.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -v -rxX`
7. `.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -k partial_overlap --runxfail -q`

**Observed**

Step 6, the issue's command: the test is reported as an expected failure, not a failure.
```
tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap XFAIL (issue #64: ...) [ 15%]
XFAIL tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - issue #64: relevance scorer 'partial overlap' fixture actually has full overlap
18 passed, 1 xfailed in 0.16s
```

That's because the test is marked in `tests/unit/test_relevance_scorer.py`:
```python
@pytest.mark.xfail(
    strict=True,
    reason="issue #64: relevance scorer 'partial overlap' fixture actually has full overlap",
)
```

Step 7, with the xfail marker ignored: the same failure the issue describes.
```
        query = "Python Django web framework"
        chunks = [
            {"text": "Django is a Python web framework for rapid development"},
        ]
    ...
>       assert 0.3 < score < 0.9  # Partial overlap should be in middle range
E       assert 1.0 < 0.9

tests/unit/test_relevance_scorer.py:58: AssertionError
------------------------------------------------- Captured stdout call -------------------------------------------------
[info     ] relevance_scored               avg_score=1.0 chunks_count=1 query_len=4
FAILED tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - assert 1.0 < 0.9
```

**Result**

Reproduced. On this commit, the issue's command exits without a failure because the test is marked `xfail(strict=True)` referencing this issue; with `--runxfail`, it fails with `assert 1.0 < 0.9`, matching the issue. The chunk in the fixture contains all four query words (Python, Django, web, framework), and the scorer logs `avg_score=1.0` for it, which lines up with the issue's description that the fixture has full rather than partial overlap. I haven't changed any code.

Since the marker is `strict=True`, a fix to the fixture would also need the xfail marker removed, or the test would then fail as an unexpected pass. I'm noting that for whoever picks up the fix, not proposing a change here.

## Eval iterations

### Run history

1. Full run (all 20 scored packages): **19/20**, bar PASS. Categories: clear-accept 7/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4. The only disagreement was pkg-07 (gold accept, my verdict reject, "failed: conventions-disclosure"). This run was saved with `--save-run` and is the committed `eval-run.txt`.

No further runs: I did not revise the rubric after this run (see Trade-offs).

### Package analysis

**pkg-07 (processing/p5.js#7168).** Gold label: **accept**. My rubric: **reject**, "failed: conventions-disclosure".

The report itself is strong. Its environment names "p5.js 1.11.7 (CDN single file), Chrome 139.0 on macOS 14.6" plus the browser language order; its steps run from a blank page; its Japanese-first console output reproduces the issue's exact error ("TypeError: Cannot read properties of undefined (reading 'replaceAll')"); and an English-first control shows the FES message rendering normally, which rules out a broken setup.

The miss is in my conventions check. The repo-facts block states the policy as: "fully AI-generated contributions are not accepted; assistive AI use is allowed, and the contributor must understand and take responsibility for every change (details in AI_USAGE_POLICY.md)". That text limits fully AI-generated work; it does not require a disclosure. The claim comment discloses anyway: "I used an AI assistant to help me organize this report; I ran and verified every step myself and I understand what I'm reporting."

My check held the package because of two strict clauses. It grades unclear when "the policy is referenced but its text is not visible", and the policy's details sit in AI_USAGE_POLICY.md, which the package does not include. It also asks that "every comment in the package contains that disclosure", and the repro report does not repeat the claim comment's disclosure. My verdict rule counts unclear as fail. In both cases the rubric treated "a policy exists and has more detail elsewhere" as "a disclosure is required and must appear everywhere", which is not what the visible policy says. The gold label reads the policy as written: assistive use, understood and owned by the contributor, and disclosed voluntarily, so accept.

### Check rationale

> | conventions-disclosure | The repo-facts block (contribution policy, AI-assistance policy, comment conventions), read against every comment in the package | If the repo's stated policy requires disclosing AI assistance, every comment in the package contains that disclosure in the form the policy asks for, and any other stated comment conventions are followed. If the repo-facts block states no such policy, pass. If the policy is referenced but its text is not visible (truncated or linked only), grade unclear. | required |

It reads this way because the lecture's proof families include "the words respect the repo's conventions", and the eval set has exactly one disclosure-wall package: a repo whose policy requires disclosing AI assistance, with comments that don't disclose. A rubric without this check cannot see that category at all. I made the check grade against the repo's stated policy rather than against whether AI is mentioned, so that a repo with no policy passes and a repo that requires disclosure is held to it. I added the "referenced but not visible → unclear" clause so the grader never passes a comment against a policy text it cannot actually read. I also made it apply to every comment in the package, because the claim comment and the repro report are posted separately and each has to stand on its own.

### Trade-offs

This check trades recall on permissive policies for safety on strict ones. The case I accept it will miss is pkg-07: a repo that allows assistive AI and links its full policy elsewhere, where my "referenced but not visible → unclear" clause and my "unclear counts as fail" verdict rule held a package the gold label accepts.

I left the check as it is, and nothing else changed, for this reason: the fix would loosen `conventions-disclosure`, the only check that matched the single `disclosure` package ("disclosure 1/1" in `eval-run.txt`). If loosening it flipped that package, the category floor would fail and the run would fail the bar at any score. At 19/20 with every category matched, a loosening revision had nothing to gain on the bar and one category to lose. A false reject also costs less than a false accept here: a false reject means revising a good comment, while a false accept means posting a comment that breaks a repo's AI policy.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
