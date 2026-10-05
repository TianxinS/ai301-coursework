# Procedure: how this skill grades a plan package

## Read order

Read the parts in this order, and write down the listed notes before grading anything. The order matters: the repro evidence has to be fixed in your notes before you read the plan, so the plan's own explanation can't shape what you think the evidence shows.

1. **Issue** section (title and body). Note the reported bug in one line: what fails, where, and the error or wrong value the issue quotes.
2. **Thread highlights** section. Note every direction a maintainer gave (e.g. "the fix should be in the fixture, not the scorer", "please open a PR against X", "don't change the API"), and any other contributor's plan on the same issue, with its author. "(no comments)" means there is nothing to follow or contradict.
3. **Repo facts** section. Note the contribution policy, the AI-use policy, and any stated asks or conventions (templates, review limits, comment or PR expectations). Mark any policy whose text is only linked and not shown.
4. **Repro evidence** section. Note the exact steps run, the environment, and the observed output: the error message or wrong value, and the file and line, test, or view it occurred in. Copy the key observed line exactly.
5. **Candidate plan** section. Read it straight through once without grading. Plans may label their parts differently (e.g. Cause / Change / Test, or Diagnosis / Scope / Approach / Test plan / Risks); map each part to the rubric's evidence by what it says, not by its label.
6. **Candidate plan comment** section. Read it last, after the plan, so you can compare the two.

In live mode, the same parts come from the locations listed in references/evidence-guide.md: the issue page and its comments, the repo's CONTRIBUTING and policy files, the posted repro comment, plan.md, and comment.md.

## Evidence gathering

For each check, pull the evidence below and record it as a short quote with where it came from. Gather everything before grading any check.

1. **diagnosis-matches-repro:** quote the plan's stated cause. Put it next to the observed output line you copied in Read order step 4.
2. **scope-bounded:** list every place the plan commits to changing (files, or a component plus how it will be pinned), and quote what it says it will not change. For each change, note whether it is at the cause's site, at another site of the same defect (and whether the issue or the plan names that site), or something else. Note any audit or investigation the plan describes and whether it commits to changing what it finds. If it has no not-in-scope statement, record "no out-of-scope statement".
3. **approach-fixes-cause:** quote the approach's description of the change. Note whether it touches the thing the diagnosis names as the cause, or something else (an assertion, a test marker, an error handler, the specific repro input).
4. **plan-executable:** for each change in the approach, record the concrete edit described and the location named (file and function, test, or fixture; or a component plus the stated method for pinning the exact function). Record any change whose edit is undecided, or whose location is open-ended with no method to find it.
5. **test-plan-observable:** quote the test plan's commands and its stated expected output. Put them next to the repro evidence's steps and observed output.
6. **unknowns-honest:** quote any risks or unknowns the plan lists (record "none listed" if there are none; that is not a fail by itself). Then list every claim the plan states as certain, and for each note whether the repro evidence supports it, contradicts it, or doesn't address it, and whether the test plan says it will check it.
7. **comment-fits-thread:** quote the plan comment's description of the change. Put it next to the plan's approach, the maintainer directions from Read order step 2, any other contributor's plan, and the conventions from Read order step 3. Then record two things explicitly: (a) whether Repo facts require disclosure of any AI usage, and if so, quote the comment's disclosure statement or record "no disclosure"; (b) any gate in Repo facts on the next step the comment announces (e.g. a vouch flow before PRs), and whether the comment acknowledges it.

If a part a check needs is not in the package at all, or is visibly cut off, record "missing: <part>" for that check instead of searching other parts to fill the gap.

## Check execution

1. Run the checks in the rubric's table order: diagnosis-matches-repro, scope-bounded, approach-fixes-cause, plan-executable, test-plan-observable, unknowns-honest, comment-fits-thread. The diagnosis comes first because approach-fixes-cause and scope-bounded are judged against it.
2. For each check, apply its pass condition from rubric.md to the evidence recorded for it in Evidence gathering. Grade from those notes; re-read the package only to confirm a quote, not to look for new evidence.
3. Grade **pass** or **fail** whenever the evidence is present, even if the call is hard. Write one line giving the reason, with the quote that decided it.
4. Grade **unclear** only when the evidence was recorded as "missing" (the part is absent or cut off). Write which part is missing.
5. Judge what the plan commits to doing, not its length, polish, or headings. A short plan that names the cause, the file, the change, and the expected output can pass every check.
6. If the plan's diagnosis fails, still grade the remaining checks against the cause the plan states, so every failure is reported.

## Verdict assembly

1. List all seven checks with their grades and one-line reasons.
2. If any check is fail or unclear, the verdict is **reject (hold)**. Every check is required, so a single fail or unclear decides it.
3. Otherwise the verdict is **accept (ready)**.
4. In the output, quote the evidence for each deciding check: for a reject, every check that failed or was unclear, with the quote that decided it (or the missing part); for an accept, a one-line reason for each check.
5. End with the JSON block the skill's SKILL.md specifies, with the verdict and per-check grades matching the list from step 1.
