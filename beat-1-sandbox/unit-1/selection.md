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

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68 

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64 

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69

**Verdict output**

Ranked read-out

  1. Issue #64 — "Relevance scorer 'partial overlap' test fixture actually has full query overlap" — accepted
  Smallest footprint: one file, one broken assertion, explicit repro command, no external library involved.

  2. Issue #69 — "Output parser crashes on a top-level JSON array fallback" — accepted
  Bounded, single-cause bug entirely in the project's own code (output_parser.py calls .items() on a list) — the failure
  is directly visible in the diff you'd write, no need to reason about a third-party library's internals.

  3. Issue #68 — "Keyword search raises ZeroDivisionError when the index is empty" — accepted
  Same shape as #69 (2 files, 2–4h estimate, xfail marker to drop) but the root cause lives inside rank-bm25's
  BM25Okapi, a third-party library boundary — slightly more indirection to trace than #69.

  Ranking caveat, unchanged from last run: scope.md's fit-profile section is still the unfilled placeholder, so this
  order is a neutral "smaller / more self-contained first" tiebreak, not a real fit-based ranking against your
  background.

  No rejected candidates — all three clear every required check.

  Per-check evidence (repo-wide checks — Maintainer Active, AI Contribution Allowed — apply identically to all three):
  - Maintainer Active: collaborator Aburke225 pushed 5 commits ending 2026-09-16T21:42Z, closed/commented on other
    issues as recently as 2026-09-16T21:50Z; non-bot.
  - AI Contribution Allowed: docs/CONTRIBUTING.md (full text) has no AI-use language; no AGENTS.md/AI_POLICY.md found
    (404) → silence passes.
  - Uncontested — #68: 0 comments, no assignee, 0 PR search hits; one cross-repo "referenced" event points to a
    different student's own selection.md notes in their personal coursework repo, not a claim in this repo — doesn't
    block per house rule either way. #64: 0 comments, no assignee, no timeline events beyond labels, 0 PR hits. #69:
    same as #64 — clean. 
  - Scope Fits — #68: single bug in KeywordSearcher.index(), 2 files, xfail H-01, 2–4h. #64: single test-fixture fix, 1
    file, explicit repro. #69: single bug in output_parser.py (.items() on a list), 2 files, xfail H-02, 2–4h.
```
  [
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64",
      "checks": [
        {"name": "Maintainer Active", "grade": "pass", "evidence": "Collaborator Aburke225 pushed/commented/closed 
  issues as recently as 2026-09-16T21:50Z, non-bot"},
        {"name": "Scope Fits", "grade": "pass", "evidence": "Single test-fixture fix, one file, explicit repro: `assert 
  1.0 < 0.9` fails"},
        {"name": "Uncontested", "grade": "pass", "evidence": "0 comments, no assignee, no timeline events beyond labels,
  0 PRs reference #64"},
        {"name": "AI Contribution Allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language; 
  no AGENTS.md/AI_POLICY.md found"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
      "checks": [
        {"name": "Maintainer Active", "grade": "pass", "evidence": "Collaborator Aburke225 pushed/commented/closed 
  issues as recently as 2026-09-16T21:50Z, non-bot"},
        {"name": "Scope Fits", "grade": "pass", "evidence": "Single bug: output_parser.py calls .items() on a list, 2 
  named files, xfail marker H-02, 2-4h estimate"},
        {"name": "Uncontested", "grade": "pass", "evidence": "0 comments, no assignee, no timeline events beyond labels,
  0 PRs reference #69"},
        {"name": "AI Contribution Allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language; 
  no AGENTS.md/AI_POLICY.md found"}
      ],
      "verdict": "accept"
    },
    {
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
      "checks": [
        {"name": "Maintainer Active", "grade": "pass", "evidence": "Collaborator Aburke225 pushed/commented/closed 
  issues as recently as 2026-09-16T21:50Z, non-bot"},
        {"name": "Scope Fits", "grade": "pass", "evidence": "Single bug in KeywordSearcher.index(), 2 named files, xfail
  marker H-01, 2-4h estimate"},
        {"name": "Uncontested", "grade": "pass", "evidence": "0 comments, no assignee, 0 PRs reference #68; cross-repo 
  'referenced' event is another student's own coursework notes, not a claim, and house rule says student claims don't 
  block"},
        {"name": "AI Contribution Allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language; 
  no AGENTS.md/AI_POLICY.md found"}
      ],
      "verdict": "accept"
    }
  ]
```

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
{
      "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/64",
      "checks": [
        {"name": "Maintainer Active", "grade": "pass", "evidence": "Collaborator Aburke225 pushed/commented/closed 
  issues as recently as 2026-09-16T21:50Z, non-bot"},
        {"name": "Scope Fits", "grade": "pass", "evidence": "Single test-fixture fix, one file, explicit repro: `assert 
  1.0 < 0.9` fails"},
        {"name": "Uncontested", "grade": "pass", "evidence": "0 comments, no assignee, no timeline events beyond labels,
  0 PRs reference #64"},
        {"name": "AI Contribution Allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI-use language; 
  no AGENTS.md/AI_POLICY.md found"}
      ],
      "verdict": "accept"
    }
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

item      gold    verdict  agree  note
issue-01  accept  accept   yes    
issue-02  reject  reject   yes    
issue-03  reject  reject   yes    
issue-04  accept  accept   yes    
issue-05  reject  reject   yes    
issue-06  accept  accept   yes    
issue-07  reject  reject   yes    
issue-08  reject  reject   yes    
issue-09  accept  accept   yes    
issue-10  reject  reject   yes    
issue-11  accept  accept   yes    
issue-12  reject  accept   NO     graded accept
issue-13  reject  reject   yes    
issue-14  accept  accept   yes    
issue-15  reject  accept   NO     graded accept
issue-16  accept  accept   yes    
issue-17  reject  reject   yes    
issue-18  reject  reject   yes    
issue-19  accept  accept   yes    
issue-20  reject  reject   yes    

categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 0/1  scope 3/4
agreement: 18/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)

**Issue analysis**

issue-12. Gold label: reject. My rubric's verdict: accept.
The repo-facts block for issue-12 quotes the contribution policy directly: "We do not accept AI-generated code or documentation. If you are unsure how something in BookWyrm works, please ask for help – we are keen to help other humans to understand and contribute to the project." This is an explicit ban, matching the rubric's own fail condition for AI Contribution Allowed: "Only an explicit statement that AI-generated contributions are not accepted fails." My run graded this check pass instead of fail. Under the verdict rule, "reject: any required check grades fail... or unclear," this single misgraded check flipped the overall verdict from reject to accept. The other three required checks (Maintainer Active, Scope Fits, Uncontested) were correctly evaluated as pass on this issue.

**Check rationale**

Quoted check (Uncontested, from rubric.md): "No assignee is currently set, and no PR is currently open against the issue — a formally linked or thread-mentioned PR that is closed or abandoned does not count as a live claim. No comment claims ownership that is still live: a claim is live if it's recent relative to the capture date, or a maintainer/bot is still treating it as active. An old claim that never produced a merged or still-open PR, with no follow-up activity since, does not block the issue."
Reasoning behind its current form: open-source issues frequently accumulate stale "I'll work on this" comments that never turn into anything. If any past claim — however old or abandoned — disqualified an issue, the rubric would reject perfectly available issues just because someone once expressed interest and vanished. The check is written to require recency and follow-through (a still-open/merged PR, or activity close to the capture date) before letting a claim count, so it filters dead interest from live ownership.


**Trade-offs**

This check assumes the visible evidence (repo-facts metadata, issue body, and shown comments) is sufficient to judge claim status. Issue-15 exposes where that assumption breaks: the eval bundle notes "97 total, first 40 shown," and the visible thread ends with a claim from souvik150 on 2024-02-16 with no unclaim event shown — captured over two years later, on 2026-08-05. My run graded Uncontested as pass based on that truncated view, when the honest grade was unclear given the missing 57 comments, and per the verdict rule unclear should have been treated as fail. The check gives up robustness to truncated threads — it has no mechanism to flag "evidence cut off" as distinct from "no claim found."

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit to interests and time: issue-64 was the clear pick on tractability — the skill's own comparison called it the smallest, most self-contained fix of the accepted candidates: one file, one broken test assertion, with an explicit repro command. Given limited time for Unit 1, minimizing surface area mattered more than matching a specific technical interest.
2. What the verdict got right vs. what I weighed beyond it: the verdict correctly confirmed all four required checks passed — active maintainer, bounded scope, no live claim, and an AI-permissive contribution policy. What the rubric couldn't weigh was relative effort between the two accepted candidates: it flagged #68 as touching a library boundary (rank-bm25) across two files versus #64's single file, single assertion — but scope.md's fit-profile section was still an unfilled placeholder, so the ranking was explicitly a size-based tiebreak, not a fit-based one. I weighed that size difference directly to pick the easier issue.
Anticipated difficulty in claiming: low — the issue has no assignee and no open PR, so claiming should be straightforward. The main risk is the same class of miss surfaced in the eval iterations above: confirming there's no more recent claim or contribution-policy change since the repo facts were captured, before posting a claim comment in Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
