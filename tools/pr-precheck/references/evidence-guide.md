# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->
Test proof, everywhere in this guide, means a run that names what was
run and states the specific result observed: pasted output, a note or
narration of what was seen, or a one-line report naming the command
and its outcome. A bare verdict naming no command and no observed
result ("tests pass", "it works") is a claim, not proof. Diff proof
means a line in the diff.

A bundle's sections are: `## Repo facts`, `## Issue`,
`## Thread highlights`, `## Plan context`, and `## Candidate PR`, with
the subsections `### Title`, `### Description`, `### Commits`,
`### Diff`, and `### Test evidence`.

## Plan fidelity (harness category: silent-drift)

<!-- Where the plan states its scope, boundary, and deviation notes,
and where the diff shows what actually changed. What it means for a
diff to match the plan, for an honest deviation to re-tie a mismatch,
and what silent drift looks like in each direction (more than the
plan, or less with no note). The description's fidelity claims read
against the diff live here too: a description claiming more or less
than the diff delivers is silent drift, not a comms problem. -->

Checks: files-match-plan, plan-implemented, description-matches-diff.

**Where it lives (eval bundle):** the plan is in `## Plan context`,
after "Plan:". Its boundary is the scope pair, the "Files:" list
and the "Not in scope:" line. Its steps are in the plan text, and
any deviation notes are in the same block. What actually changed is in
`### Diff`: the file headers (`--- a/...`, `+++ b/...`) give the
changed files, and the `+`/`-` lines give the changes. `### Commits`
gives each commit's stated purpose. The fidelity claims are in
`### Title` and `### Description`.

**Where it lives (live):** `plan.md`, including its "Files" or scope
lines and `## Deviations`. The changed files come from
`git diff main...HEAD --stat`, the hunks from `git diff main...HEAD`,
and the commits from `git log main..HEAD --oneline`. Only committed
changes count. The claims are in `pr_draft.md`: the title on line 1,
then the Summary and Changes sections.

**What good looks like:** every changed file falls inside the plan's
"Files:" list (by path, folder, or component; "a changelog entry" or
"a test" covers the matching changelog or test file, fixtures, and
snapshots) or is named in a deviation note with a reason. Every step
the plan commits to appears as a hunk that makes the planned fix, not
one that hides the symptom (a new skip or xfail, a loosened assertion,
a swallowed error, or a special case for the repro input). An honest
deviation note re-ties a mismatch: a file outside the plan, or a step
left out, passes once the note says what changed and why. Silent drift
runs both ways. **More:** calib-03's diff changes `property.js`, which
the plan never names, while the description says "exactly, with no
changes beyond it". **Less:** a planned step or file has no hunk and
no note. Every claim in the description has a diff line behind it, and
a description that claims more or less than the diff delivers is drift
here, not a comms problem. calib-01 is the clean shape: "Files:
`themes.gitconfig`", one hunk in that file that deletes the shadowed
line, and a description that says exactly that.

<!-- Where the PR shows its proof: the test-evidence section's
before/after against the plan's test plan and the reproduction's own
steps, and the outcome of the repo's own checks or suite. What
decisive looks like (an observable behavior named, the expected-after
stated, the checks' outcome visible) next to "tests pass". -->

## Test evidence (harness category: not-tested)

Checks: repro-before-after, test-plan-covered, repo-checks-run,
test-claims-backed.

**Where it lives (eval bundle):** the proof is in `### Test evidence`.
The plan's test plan is in `## Plan context`, after "Test plan:", and
names each case and its expected result. The before state is in the
repro evidence at the top of `## Plan context` (the accepted report's
command and output). The issue's expected behavior is in `## Issue`.
The repo's own checks are whatever the PR template or contribution
docs in `## Repo facts` list. Test files added by the PR are in
`### Diff`, usually under `tests/`. Testing claims and ticked
checkboxes are in `### Description`.

**Where it lives (live):** `test_evidence.md` holds the before run
(on `main`, or your reused unit 2 output), the after run on your
branch, and the output of the four checks Path Review's template
lists: `make test-unit`, `make test-integration`, `make lint`, and
`make typecheck`. The test plan is in `plan.md`. Test files are in
`git diff main...HEAD` under `tests/`. Testing claims and checkboxes
are in the Testing section of `pr_draft.md`.

**What good looks like:** the repro is re-run on the fix, and the
evidence states what was observed, which is the issue's expected
behavior: pasted output (calib-01: `parsed OK`; pkg-19: the round-trip
plus `IDENTICAL`), or a concrete note of what was seen ("after:
searches normally, no error"; "toast '...' shown, no prompt; file
still listed"; "`{ code = 0 }`"). The before is shown by a before run
or by the plan's repro evidence. Every case the test plan names is
reported with its outcome, even in one line ("no-ampersand control
unchanged"), or its absence is disclosed. The repo's suite, or the
part of it covering the change, is reported by command and outcome
("`cargo test -p ignore` passes (214 tests)"; "`go test ./...`
passes"). A failure or "no tests ran", reported as is with the reason,
is decisive evidence (Path Review's `make test-integration` prints "no
tests ran"). The weak shapes are no after run of the repro at all, a
named test-plan case that never appears in the evidence, no run of the
repo's tests, or a bare verdict naming no command ("all tests pass",
"verified it works").

## Diff quality (harness category: unreviewable)

<!-- Where the change itself lives: the unified diff and the commit
list. What a reviewable change looks like (the fix visible, nothing
unrelated riding along) and the debris tells: debug leftovers, dead
code, commented-out blocks, formatting churn, drive-by edits. -->

Checks: no-debris, focused-on-issue.

**Where it lives (eval bundle):** `### Diff`, hunk by hunk, read
against the bug in `## Issue` and the approach in `## Plan context`,
plus `### Commits` for anything the commit list shows that the
description doesn't.

**Where it lives (live):** `git diff main...HEAD` and its `--stat`,
plus `git log main..HEAD --oneline`. Before you push, `--stat` lists
only the files your fix changes, and none of your working files
(`plan.md`, `comment.md`, `pr_draft.md`, `test_evidence.md`).

**What good looks like:** every hunk is the fix, a test for it, or an
artifact the repo requires (a changelog entry), so a reviewer sees the
change and nothing riding along. calib-01's single one-line deletion
is the model. The debris tells are added `print`, `console.log`, or
breakpoint lines; dead or commented-out code; stray files (lockfiles,
build output, editor files); whitespace or formatting churn outside
the changed lines; and drive-by edits (a refactor, rename, or cleanup
the issue doesn't need). An unrelated hunk is still unreviewable when
the description explains it.

## Standards and comms (harness category: standards-wall)

<!-- Where the repo states its asks (the repo-facts block's PR
template sections, contributing instructions, and stated policy,
including AI-use disclosure) and where the PR honors them: the
stated sections filled with real content, the disclosure present,
explicit maintainer direction in the thread engaged. What compliant
looks like next to boilerplate or a visibly ignored ask. (Whether
the description's claims match the diff is plan fidelity, above.) -->

Checks: template-complete, issue-linked, ai-use-disclosed,
repo-standards-met.

**Where it lives (eval bundle):** the repo's asks are in
`## Repo facts`: the "pull requests:" line (template sections, or "no
PR template", plus asks like "stay small and reference the issue")
and the "contribution policy" line (including any AI-use policy, or
"no stated AI policy"). Maintainer direction and any gate on opening a
PR are in `## Thread highlights`. The PR's compliance is in `### Title`
and `### Description`.

**Where it lives (live):** `.github/PULL_REQUEST_TEMPLATE.md` and
`docs/CONTRIBUTING.md` in the Path Review repo, plus the house rules
in `scope.md`. Maintainer direction is in the issue thread. The PR
text is `pr_draft.md`, and the branch name comes from
`git branch --show-current`.

**What good looks like:** every section the template states is filled
with content about this change. For Path Review, that's Summary, Issue,
Changes, Testing, Screenshots / Demo, and Notes for Reviewers, with
"N/A" only alongside a reason. The issue is referenced in the form the
repo asks for (`Closes #N` with the plan's issue; calib-01 references
`(#2211)`, which meets delta's "reference the issue" ask). Where the
policy asks for AI-use disclosure, the description says how AI was
used on this change, with the tool and extent, or that none was used.
Path Review's goes in Notes for Reviewers. Every other stated ask is
met (changelog, tests for bug fixes, a branch name like
`fix/64-short-description`, one PR per issue), and any maintainer
direction in the thread is followed or engaged with. The failing shape
is a stated section left blank or filled with boilerplate, a missing
disclosure where one is asked for, or a maintainer direction ignored.
When `## Repo facts` states no template or no AI policy (calib-01),
there is no ask to fail.