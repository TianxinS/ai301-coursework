---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer one question: is this ready
to submit? A PR package is a candidate pull request: its title, its
description, its commits, its diff, and its test evidence. You read it
against the plan it claims to implement (deviation notes included) and
the issue that plan belongs to. Do not answer any other question. Do
not review the code's design beyond what the rubric asks, do not
suggest a better fix, and do not grade the issue or the plan for their
own sake. They are the yardstick the PR is measured against. Grade
exactly one package per run. If a request names more than one, stop
and ask which one to grade. You do not answer from gut feel. You
answer by executing `procedure.md`, which applies the checks in
`rubric.md` to evidence gathered per `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If the request names a package bundle (a file
in `eval/packages/`, or an id such as `pkg-07` or `calib-01`), use
eval mode. If it asks you to grade the student's own draft PR for an
issue URL, use live mode. If you can't tell, ask. Do not guess.

**Live mode**: the student's own submission, checked before it goes
out. Run from the top folder of the student's fork clone, on their
branch. Gather every one of these inputs:

1. **Plan**: `plan.md` in the working directory, including its
   `## Deviations` section if there is one. Deviations are part of the
   plan and are graded like the rest of it.
2. **Diff**: the committed changes the branch makes relative to the
   repo's default branch. Produce it with `git diff main...HEAD`
   (three dots), and the file list with `git diff main...HEAD --stat`.
   Read the commits with `git log main..HEAD --oneline` and the branch
   name with `git branch --show-current`. Uncommitted or untracked
   changes are not part of the PR: do not grade them, and do not treat
   working files (`plan.md`, `comment.md`, `pr_draft.md`,
   `test_evidence.md`) as part of the diff.
3. **Draft PR title and description**: `pr_draft.md`. The first line
   is the title. Everything below it is the description.
4. **Test evidence**: `test_evidence.md`, the pasted output of the
   repo's checks (`make test-unit`, `make test-integration`,
   `make lint`, `make typecheck`) and the before/after repro runs.
5. **Issue-side evidence**, fetched from the real repo named in
   `scope.md` (via `gh`, the GitHub API, or the web): the issue thread
   at the given URL (body and comments), the repo's PR template at
   `.github/PULL_REQUEST_TEMPLATE.md`, and its stated policy in
   `docs/CONTRIBUTING.md` (including the AI-use rules).

A house-chain student reads the house plan and the house repro pack
in place of their own plan and repro output. The same checks grade
the same things there.

In live mode, refuse to grade and name exactly what is missing if:
`plan.md` or `pr_draft.md` does not exist, the current branch is
`main`, `git diff main...HEAD` is empty, or the issue thread cannot be
read. A missing or empty `test_evidence.md` is not a refusal. It is
evidence, and the testing checks grade it.

**Eval mode**: the package bundle is the whole world. Read only the
named bundle file. Every fact comes from the bundle text. Fetch
nothing, run no git commands, read no other file, and bring in no
outside knowledge of the repo. Eval mode always grades a complete
package: run every check and apply the full verdict rule. A section
missing from the bundle is evidence for the checks that read it, not
a reason to refuse. Refuse only if the named bundle cannot be found or
opened, and say which file.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before any git command or fetch. It names the repo where the
student's PR must live and the house rules of that environment. Apply
those house rules when reading evidence and running checks. If the
issue URL, or the repo the branch targets, is outside the scoped repo,
refuse to grade and say which repo the scope allows. If the `Repo:`
line still carries an unfilled placeholder, stop without grading and
tell the student to fill the `Repo:` line in `scope.md` with their
section's Path Review repo. Never guess a scope. In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, after reading the inputs, read `voice-guide.md`: the
student's own rules for how they write upstream. Hold the outgoing PR
text (the title and the description in `pr_draft.md`) against those
rules. In the summary, list each rule the draft breaks: quote the rule,
then quote the line that breaks it. The voice guide never changes the
verdict on its own. It is personal style and carries no gold labels,
so it changes the verdict only if `rubric.md` has a check that reads
it. In eval mode, ignore `voice-guide.md` entirely. Universal
communication-quality checks live in the rubric.

## Component reads

Read these components from this skill directory, and connect them this
way:

- `rubric.md` is the brain. Its table defines every check (name,
  evidence, pass condition, weight), and its verdict rule says how
  check grades combine into `accept` or `reject`. `required` checks
  gate the verdict. `preferred` checks never change it.
- `references/evidence-guide.md` is the map. For each evidence family,
  it says where that evidence lives in a package bundle and in a live
  PR, and what good looks like. Gather evidence where it says.
- `procedure.md` is the hands. Execute it as written, in order: read
  order, evidence gathering, check execution, verdict assembly. Do not
  skip, reorder, or add steps.

When the procedure is silent on something you need to do, do not
improvise around it. Take the narrowest reading that still lets the
rubric's check run, and name the gap in your summary as a procedure
gap. If the gap means a check can't be decided, grade it `unclear`.

If `rubric.md` has no checks written, or `procedure.md` has no steps
written, stop and say so. Instruction comments alone do not count as
content. This tool cannot grade without both, and an empty tool that
invents checks at runtime produces noise that looks like judgment.

## Verdict and output

The verdict is binary: `accept` means ready to submit, and `reject`
means hold. There is no third verdict, no "accept with reservations",
and no score. Reservations go in a check's evidence line, never in the
verdict.

Before the JSON block, print a readable summary: one line per check
(name, grade, the deciding fact). In live mode, add any voice-guide
breaks, then a short "fix before opening" list with required failures
first. Then end your reply with this fenced JSON block, valid and
last, with nothing after it. For `item`, use the bundle id in eval
mode. In live mode, use the PR URL if the PR is open, otherwise the
issue URL. List every rubric check in `checks`, in rubric order, with
its name exactly as the rubric writes it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: a file and line in the diff, a section of the
  description, a line of test output, a sentence of the plan. "Looks
  fine" is not evidence.
- **Grade the thing, not the polish.** A terse, complete PR can be
  ready, and a long, confident one can be hiding drift. Read the diff
  itself against the plan, the issue, and the stated standards.
  Never take the description's word for what the diff or the tests
  show. Check it.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary if you like. The fix belongs in the rubric, not in this run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an unverifiable claim
  is a failing one. A PR you cannot verify from the package is a PR
  that is not ready to submit.
- **Report, don't repair.** Never edit code, commit, push, or rewrite
  the student's drafts during a run. Say what is wrong and where.
