# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the se-->

Test proof, everywhere in this rubric, means a run that names what was
run and states the specific result observed. That can be pasted
output, a note or narration of what was seen ("after: searches
normally, no error"; "toast '...' shown, no prompt"; "exit code 0"),
or a one-line report that names the command and its outcome
("`cargo test -p ignore` passes (214 tests)"). A bare verdict that
names no command and no observed result ("tests pass", "it works",
"verified") is a claim, not proof. Diff proof means a line in the
diff. "The plan" always includes its Deviations section.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| files-match-plan | The diff's changed files, read against the plan's file list, scope, and Deviations | Every file the diff changes is named in the plan, by path or by its folder or component. A plan item like "a changelog entry" or "a test" names the matching changelog or test file, and snapshot or fixture files that a planned test generates count as part of that test. A change at another site of the same defect, named by the issue or by the plan, is part of the fix. Fail on any changed file the plan doesn't cover and the Deviations don't disclose. | required |
| plan-implemented | The diff's hunks, read against the plan's approach and steps, plus the Deviations and the description's notes | The diff makes the change the plan's approach describes. Where the plan named a component plus how it would pin the exact spot, an edit inside that component counts. Every planned change appears in the diff, or its omission is disclosed with a reason. Fail if a planned change is missing and not disclosed, or if the diff hides the symptom instead of making the planned fix: adding a skip or xfail, loosening or deleting an assertion, catching and silencing the error, or special-casing the repro's exact input. | required |
| description-matches-diff | Each factual sentence in the title, summary, and changes list about what the code does, read against the diff | Judge only claims about what the diff changes (files, functions, lines, tests or options added or removed) and fidelity claims ("implements the plan exactly", "no changes beyond the plan", "only"). Fail if such a claim is contradicted by the diff, if the description claims a change that has no hunk, or if a fidelity claim is false because the diff does more or less than the plan. Claims about effects or behavior ("rendering is unchanged", "strict parsers no longer error") are not graded here; the test-evidence checks grade those. A negative claim ("no options added") passes if nothing in the diff contradicts it. | required |
| repro-before-after | The test evidence, read against the issue's repro steps and expected behavior | The repro steps (or a check that runs the real code with the repro's inputs) are re-run on the fixed code, and the evidence states the specific result observed after the fix, as pasted output or as a description of what was seen. That result is the issue's expected behavior. The before is shown by a before run or by the repro evidence the plan built on. Fail if there is no after run of the repro at all, if the after result is a bare verdict that says nothing about what was observed ("fixed", "works now"), or if the observed result is not the expected behavior. | required |
| test-plan-covered | The plan's test plan, read against the diff's test files and the test evidence | Every case the plan's test plan names (controls, edge cases, a second input, a smoke check, a suite to run) is reported in the test evidence with its outcome, even as one line ("no-ampersand control unchanged", "`go test ./...` passes"), or its omission is disclosed with a reason. A case does not need its own test file or pasted output unless the plan says so. Fail if a named case is not mentioned in the test evidence at all and not disclosed, if a test the plan says it will add isn't in the diff, or if a test the description claims to add isn't in the diff. | required |
| repo-checks-run | The test evidence, read against the checks the repo's PR template lists (the repo-facts block in a bundle) | The repo's own test suite, or the part of it that covers the change, was run, and the evidence names the command and its outcome (pasted output, or a one-line report such as "`go test ./...` passes"). An honestly reported failure or "no tests ran", with the reason, passes. Template checklist items that are not commands (code formatted, docs updated) are not graded here. If the repo states no checks or suite, a run of the plan's tests suffices. Fail if the evidence shows no run of the repo's tests at all, or only a bare verdict that names no command ("all tests pass"). | required |
| test-claims-backed | Every testing claim in the description, ticked checkboxes included, read against the test evidence | Fail only if a ticked checkbox or a testing claim in the description is contradicted by the evidence (the evidence shows a failure the description calls a pass, or a box claims a test the diff doesn't add), or if the description claims something was tested that the test evidence never mentions. A claim the test evidence reports, even in one line, passes. | required |
| no-debris | The diff's hunks | The diff contains no debug output (print, console.log, breakpoints), no commented-out code, no stray or generated files the fix doesn't need, and no whitespace or formatting churn outside the lines the fix changes. | required |
| focused-on-issue | The diff's hunks, read against the issue and the plan | Every hunk serves the issue's fix. Fail on any unrelated change (a drive-by refactor, rename, or cleanup the issue doesn't need), even if the description explains or discloses it. | required |
| template-complete | The description, read against the repo's PR template (the repo-facts block in a bundle; `.github/PULL_REQUEST_TEMPLATE.md` live) | Every section the template has is present and filled with content about this change. "N/A" passes only with a reason. Fail on a missing, empty, or placeholder section. A template that is only linked, with its text not shown, is not held against the PR. If the repo states it has no PR template, this check passes. | required |
| issue-linked | The description's issue reference, read against the issue the plan belongs to | The title, description, or a commit message references the plan's issue (`#N`, `Closes #N`, `Fixes #N`, or the issue URL). If the repo's PR template or contribution docs require a specific form (such as a `Closes #N` line in an Issue section), that form is required. Fail if there is no reference to the issue, it names a different issue, or a required form is missing. | required |
| ai-use-disclosed | The description, read against the repo's stated AI-use policy (repo-facts block; `docs/CONTRIBUTING.md` live) | If the repo states no AI policy, this check passes. If the stated policy asks for AI-use disclosure, the description contains a disclosure statement about this change (the tool and extent of assistance, or a statement that none was used), where the policy asks for it. A PR with no disclosure fails, because the grader cannot know AI was not used. Generic boilerplate that says nothing about this change fails. A policy that is only linked, with its text not shown, is not held against the PR. | required |
| repo-standards-met | The package, read against every other PR requirement the repo states (repo-facts block and thread highlights; `docs/CONTRIBUTING.md`, the issue thread, and `scope.md` house rules live) | Every stated requirement that applies to this PR is met: for example, a changelog entry if the repo requires one, a test for a bug fix if required, a branch name in the stated format, or a stated gate (assignment, vouch, or approval before a PR) acknowledged rather than ignored. The PR does not ignore or contradict a direction a maintainer gave in the thread. Grade only requirements the repo or a maintainer actually states. A convention that is only linked, with its text not shown, is not held against the PR. | required |

## Verdict rule

Accept (ready to submit) if every required check passes. There are no
preferred checks.

Reject (hold) if any required check fails or is unclear. Grade a check
unclear only when the evidence it needs is missing or cut off in the
package, such as a PR with no test evidence section or a truncated
diff. When the evidence is present and the call is just hard, grade
pass or fail and give the reason. Do not use unclear to avoid a
judgment.

Honest outcomes are not holds. A shortfall disclosed with a reason in
the description or the plan's Deviations passes the check it would
otherwise fail: a fix that needed a file the plan didn't name
(files-match-plan), a planned change left out (plan-implemented), a
test case left out (test-plan-covered), or a repo check that failed or
ran no tests (repo-checks-run). Disclosure never rescues no-debris,
focused-on-issue, or description-matches-diff. Debris and unrelated
hunks still bury the change when explained, and a description can't
disclose its way out of being false.

Category map (the harness's scoring categories):
- Silent drift: files-match-plan, plan-implemented, description-matches-diff
- Not tested: repro-before-after, test-plan-covered, repo-checks-run, test-claims-backed
- Unreviewable: no-debris, focused-on-issue
- Standards wall: template-complete, issue-linked, ai-use-disclosed, repo-standards-met


<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict;
unclear counts as fail." -->
