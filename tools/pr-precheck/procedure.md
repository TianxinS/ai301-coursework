# Procedure: how this tool grades a PR package

## Read order

Read the parts in this order, and write down the listed notes before grading anything. The order matters. The yardstick (issue, standards, plan) goes into your notes before you open the PR, so the PR is read against what it promised and not the other way round. The diff is read before the description, so the description's claims can't shape what you think the diff does. This mirrors the plan-check rule that the evidence is fixed before the plan explains it.

1. **Issue** section (title, body, repro steps, expected behavior). Note the reported bug in one line, the issue number, and the issue's expected behavior or output, copied exactly.
2. **Thread highlights** section. Note every direction a maintainer gave (e.g. "keep the fix in the fixture", "add a test for X", "don't change the API") and any gate on opening a PR (assignment, vouch, approval). "(no comments)" means there is nothing to follow or contradict.
3. **Repo facts** section. Note the PR template's sections and the checks it lists, the AI-use policy and where it asks for disclosure, and every other stated PR requirement (changelog, tests for bug fixes, branch naming, title convention). Mark any policy whose text is only linked and not shown.
4. **Plan context** section, including the repro evidence at its top (note its command and observed output; this is the before state). Note the stated cause, the approach, every file or component the plan names (often after "Files:"), what it says it won't change, its test plan (every named case and its expected output), and its Deviations if any. Plans label their parts differently; map each part by what it says, not by its label.
5. **Candidate PR → Diff and Commits** subsections. Read the changed-file list first, then every hunk, then the commit list. Do not grade yet; just read it through once.
6. **Candidate PR → Test evidence** subsection. Read every command and its printed output. Note which lines are real printed output and which are sentences or notes after `#` describing output.
7. **Candidate PR → Title and Description** subsections. Read them last, after the diff and evidence, so you can compare claims against what you've already seen.

In live mode, the same parts come from the locations in references/evidence-guide.md: the issue page and its comments; `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and `scope.md` house rules; `plan.md`; `git diff main...HEAD` (with `--stat` for the file list); `git log main..HEAD --oneline` and `git branch --show-current`; `test_evidence.md`; and `pr_draft.md` (title on line 1).

## Evidence gathering

For each check, pull the evidence below and record it as a short quote with where it came from. Most gathers are pairings: write the two things side by side. Gather everything before grading any check.

1. **files-match-plan:** list every file in the diff. Next to each, write the plan line, folder, or component that covers it, or the Deviation that discloses it, or "not covered". Mark which files are tests, fixtures or snapshots for a planned test, and changelog entries. Mark any file at another site of the same defect, and whether the issue or plan names that site.
2. **plan-implemented:** list every change the plan's approach and steps commit to. Next to each, write the diff hunk that makes it (file and the key added or removed line), or "missing" plus any disclosure in Deviations or the description. Then list any hunk that adds a skip or xfail, loosens or deletes an assertion, catches and silences an error, or special-cases the repro's exact input. Record "none" if there are none.
3. **description-matches-diff:** list every sentence in the title and description that claims what the diff changes (files, functions, lines, tests or options added or removed), plus every fidelity claim ("exactly", "only", "no changes beyond the plan") and negative claim ("no options added"). Skip claims about effects or behavior ("rendering is unchanged"); the test checks grade those. Next to each, write the diff line that shows it, the diff line that contradicts it, or "no hunk" for a claimed change the diff doesn't make. A negative claim with nothing in the diff against it is "shown". For a fidelity claim, compare against the result of gather 1.
4. **repro-before-after:** quote the after run of the issue's repro (or a check running the real code with the repro's inputs) and the result it reports, whether pasted output, a `#` note, or narration of what was seen. If the after result says nothing about what was observed ("fixed", "works now"), record "bare verdict". If there is no after run of the repro, record "no after run". Put the observed result next to the issue's expected behavior. For the before, quote a before run, or the repro evidence from Plan context.
5. **test-plan-covered:** list every case the plan's test plan names. Next to each, quote the line in the test evidence that reports it and its outcome (one line is enough), or record "not mentioned" plus any disclosure. Note whether the plan says it will add a test file for that case, and if so, the diff file that adds it or "not in diff". Then list every test the description claims to add, and next to each, the diff file that adds it or "not in diff".
6. **repo-checks-run:** note the repo's test commands from Repo facts or the plan (Path Review: `make test-unit`, `make test-integration`, `make lint`, `make typecheck`). Quote each line in the test evidence that names one of those commands, or a suite covering the change, with its outcome (pasted output or a one-line report). A failure or "no tests ran" with the reason is "reported". If no repo test run appears, record "no repo run". If only a verdict with no command appears ("all tests pass"), record "bare verdict". Skip template checklist items that aren't commands.
7. **test-claims-backed:** list every ticked checkbox and every testing claim in the description. Next to each, write the test-evidence line that reports it, the line that contradicts it, or "never mentioned".
8. **no-debris:** go through every hunk once. Quote any debug output, commented-out code, stray or generated file the fix doesn't need, or whitespace or formatting churn outside the changed lines. Record "none" if there is none.
9. **focused-on-issue:** in the same pass, quote any hunk that doesn't serve the issue's fix (a refactor, rename, or cleanup the issue doesn't need), and note whether it is disclosed. Record "none" if there is none.
10. **template-complete:** list every section of the PR template from Read order step 3. Next to each, write "filled" with a few words of its content, "N/A with reason", or "missing", "empty", or "placeholder".
11. **issue-linked:** quote every reference to an issue in the title, description, and commits (`#N`, `Closes #N`, `Fixes #N`, or a URL), or record "none". Note whether N matches the issue from Read order step 1, and whether the repo requires a specific form.
12. **ai-use-disclosed:** record whether repo facts ask for AI-use disclosure, and where. Quote the description's disclosure statement, or record "no disclosure". Note whether it is about this change or generic.
13. **repo-standards-met:** list every other stated requirement and maintainer direction from Read order steps 2 and 3, plus scope house rules in live mode. Next to each, write the fact in the package that meets or misses it. Mark any convention that is only linked as "linked only".

If a part a check needs is not in the package at all, or is visibly cut off, record "missing: <part>" for that check instead of searching other parts to fill the gap.

## Check execution

1. Run the checks in the rubric's table order: files-match-plan, plan-implemented, description-matches-diff, repro-before-after, test-plan-covered, repo-checks-run, test-claims-backed, no-debris, focused-on-issue, template-complete, issue-linked, ai-use-disclosed, repo-standards-met. Drift comes first because the description and the tests are judged against what the diff actually does.
2. For each check, apply its pass condition from rubric.md to the evidence recorded for it in Evidence gathering. Grade from those notes. You may grade a check without re-reading the package when its gathered list is complete. Re-read only to confirm a quote, never to look for new evidence.
3. Apply the disclosure rule from rubric.md's verdict rule while grading files-match-plan, plan-implemented, test-plan-covered, and repo-checks-run: a shortfall disclosed with a reason passes. Never apply it to description-matches-diff, no-debris, or focused-on-issue.
4. Grade **pass** or **fail** whenever the evidence is present, even if the call is hard. Write one line giving the reason, with the quote that decided it.
5. Grade **unclear** only when the evidence was recorded as "missing: <part>". Write which part is missing.
6. Judge what the diff and evidence show, not the description's length, polish, or headings. A terse description that is true, complete, and backed by output can pass every check.
7. If an early check fails, still grade every remaining check, so every failure is reported.

## Verdict assembly

1. List all thirteen checks with their grades and one-line reasons, in rubric order.
2. If any check is fail or unclear, the verdict is **reject (hold)**. Every check is required, so a single fail or unclear decides it. Unclear counts the same as fail.
3. Otherwise the verdict is **accept (ready)**.
4. The deciding check is the first check, in rubric order, graded fail or unclear. Quote it first in the summary, under "Deciding check", with the quote that decided it (or the missing part). Then list every other failing or unclear check with its quote. For an accept, there is no deciding check; give a one-line reason for each check.
5. In live mode only, after the check list, add the voice-guide notes (each broken rule quoted, with the line that breaks it) and a "fix before opening" list: one line per failing check saying the specific change that would make it pass. Neither changes the verdict.
6. End with the JSON block the skill's SKILL.md specifies: the 13 checks in rubric order, with the names, grades, and evidence lines from step 1, and the verdict from steps 2–3. Nothing follows the block.