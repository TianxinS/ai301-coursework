# Contribution: Add test coverage for rafter brief

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** [Tianxin Song]
**GitHub Username:** [TianxinS]
**Issue:** https://github.com/Raftersecurity/rafter-cli/issues/28
**Pull Request:** [GitHub PR link, once opened]

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

**Unit 5: Built an issue scout for agent-related Python issues and tightened its rubric after the first scan**

I wrote an issue-scout search strategy with four gh search issues queries aimed at agent-related Python projects, plus ranking filters that drop assigned, stale, and feature-sized issues and put the easiest, least-discussed issues first. My first scan accepted an issue written in Indonesian and a five-step Tracekit task that needed a live Open Interpreter session, and one query returned zero results because an unquoted phrase broke its OR expression. I added English Language and Easy Environment Setup checks to my rubric, quoted the broken query, and added title filters for "call for contributors" and tracking posts. When I graded my three picks from the Issue Selection site, the new setup check rejected two of them (a multi-container Docker setup and an Agda/Nix project), and the grader flagged that my Uncontested check doesn't say whether "can I be assigned?" requests count as claims.

**Unit 6: Installed the voice guard with my upstream rules and wrote a repo-memory file for rafter-cli**

I added an Upstream section to my voice guide (two claim rules, two question rules, one ping rule), installed the voice guard as a Claude Code hook, and posted my claim on rafter-cli#28 through it. Two things broke: Repro-check failed my claim draft for promising a report-back date, which exposed that my voice guide contradicted itself (one rule banned dates, my new claim rule required one), so I deleted the date and passed. For the memory file, rafter-cli ships both CLAUDE.md and AGENTS.md, so I dropped the AGENTS.md import to avoid loading the same conventions twice, and kept the file to distilled conclusions: the dual-implementation rule, the PR checklist, the AI disclosure policy, and the finding that `brief.ts` exports none of its helpers while Python's `TestInternalFunctions` tests seven of them directly.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I built my issue scout around my fit profile: I'm strongest in Python, I like agent-related projects, and I want narrow, clear, easy-to-set-up issues first. My first scan returned 489 issues, my filters cut them to 241, and the scout graded the top five. That scan showed me more about my tool than about any one issue, so I picked my real candidates from the course's Issue Selection site and graded three of them: rafter-cli#28, OWASP/Nest#4833, and formal-ledger-specifications#222. After this week's scope patch, the scout accepted only rafter-cli#28, with a "moderate" difficulty note. It rejected Nest#4833 as a repo-wide cleanup that about nine people had already asked for, with a required Docker setup, and formal-ledger#222 as a codebase-wide convention that was never settled, in an Agda/Nix project. Rafter is a security tool for AI agents, which fits my interest in agent tooling, and a maintainer had already narrowed #28 to one remaining gap: Node↔Python parity for the `brief` command's internal helpers (PR #212 closed the other gap). CONTRIBUTING.md also says AI-assisted contributions are welcome if disclosed. The scout's caveat is real: most of the work is TypeScript (exporting helpers from `node/src/commands/brief.ts` and writing Vitest tests), not Python. I chose it anyway because the Python `TestInternalFunctions` tests already exist as a spec I can read and mirror. I hope to learn how a project keeps two implementations in sync, and to get comfortable writing tests in a TypeScript codebase.

My scout held up at the end but needed real fixes along the way. The first scan accepted an issue written in Indonesian and a five-step task that needed a live Open Interpreter session, so I added English Language and Easy Environment Setup checks; the setup check then rejected two of my three picks. One query returned nothing, and my ranking mostly rewarded newness, which pushed brand-new repos with zero stars to the top. My rubric still has no check for whether anyone uses a repo. The scout also has two judgment gaps I haven't fixed: Uncontested doesn't say whether "can I be assigned?" requests count as claims, and the setup check passed rafter-cli even though it needs Node/pnpm as well as Python. I also posted a claim before installing my voice guard, deleted it, and reposted it after it passed repro-check and the guard.

Claim comment: https://github.com/Raftersecurity/rafter-cli/issues/28#issuecomment-6783300683

**Toolkit**

- Tool: issue-scout (my search strategy and rubric). I ran one wild scan with my own queries and ranking filters, then graded my three Issue Selection picks twice: once with my original rubric, and once after this week's scope patch.
- What it produced: a first scan of 489 → 241 issues with five verdicts (3 accepted, 2 rejected), then grades on my picks: rafter-cli#28 accepted with a "moderate" difficulty note (mostly TypeScript, across Node and Python), and Nest#4833 and formal-ledger#222 rejected on scope and setup.
- Difficulties: the first rubric accepted a non-English issue and a task needing a live third-party session; one query returned 0 results; ranking rewarded newness, surfacing 0-star repos; my unit-1 Scope Fits check was strict enough to reject real issues with open edges; and the rubric has no rule for assignment requests or for my own claim.
- What changed: added English Language and Easy Environment Setup checks; added title filters for umbrella posts and non-Latin titles; replaced my unit-1 Scope Fits check with the course's scope-feasible check and added a difficulty note. I reworded its evidence column to include maintainer comments that narrow an issue, because #28 only passes scope through Rome-1's comment. Next I'd add an adoption check (stars or contributor count) and clarify how Uncontested treats assignment requests.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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
