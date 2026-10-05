# Evidence guide: where evidence lives in a plan package

A plan package has these sections: **Repo facts**, **Issue**, **Thread highlights**, **Repro evidence**, **Candidate plan**, and **Candidate plan comment**. Candidate plans label their own parts in different ways (Cause / Change with In and Out / Test, or Diagnosis / Scope / Approach / Test plan / Risks). Find each part by what it says, not by its label.

## Diagnosis and grounding

**Where it lives**
- Eval: the stated cause in the Candidate plan section (often labelled Cause or Diagnosis), read against the observed behavior in the Repro evidence section (its steps, "Actual" line, and any output: the error or wrong value, and the file, test, or view where it appeared). The Issue section gives the reported symptom; the Repro evidence section is what was actually observed.
- Live: the cause or diagnosis in the draft plan.md, read against the student's posted repro comment on the issue thread.

**What good looks like**
The stated cause explains the behavior the repro evidence actually shows: the same error or wrong value, in the same place, under the same steps. Anything the plan quotes from the repro matches the Repro evidence section. A diagnosis fails grounding when it explains a different symptom, cites behavior the repro doesn't show, or contradicts it (for example, blaming the code when the repro shows the code returning the correct value and the test expecting the wrong one).

## Scope

**Where it lives**
- Eval: the in-scope and out-of-scope statements in the Candidate plan section (often "In:" and "Out:" inside the Change part, or a separate Scope part), and the files the plan names.
- Live: the scope and files-to-touch parts of the draft plan.md.

**What good looks like**
The plan says where it will change code (files, or a component plus how the exact spot will be pinned), says what it will leave alone, and every change it commits to traces back to the stated cause. A bounded change touches the place the cause points to, plus any other site of the same defect that the issue or the plan names with its location; an audit that only reports findings is not a change. A drive-by rewrite adds refactors, renames, formatting, or "related" fixes the cause doesn't require, or leaves the edge open ("and anything else that needs updating").

## Executability

**Where it lives**
- Eval: the change or approach in the Candidate plan section, with the files, functions, callbacks, tests, or fixtures it names.
- Live: the approach and files-to-touch parts of the draft plan.md.

**What good looks like**
Each change has a concrete edit (what will be added, changed, or removed) and a location: a file and the function, callback, test, or fixture in it, or, when the exact function can only be found by tracing, a named component plus the method the plan will use to pin it (e.g. debug logs the author already has working). A stranger could start without asking the author a design question. "Fix the scorer logic" or "update tests as needed" is not executable; "in the push completion callback in pkg/gui/controllers/sync_controller.go, add the commits context to the post-push refresh scope" is.

## Test plan

**Where it lives**
- Eval: the test part of the Candidate plan section, read against the steps and the observed ("Actual") behavior in the Repro evidence section.
- Live: the test plan in the draft plan.md, read against the steps and output in the student's posted repro comment.

**What good looks like**
The test plan re-runs the repro steps (or a check that runs the real code with the repro's inputs) and states what will be observed after the fix, at a specific step, in a way that differs from the repro's failure (for example, "at step 3 the color must flip without leaving the view", or "the test passes and is no longer reported as xfail"). A vague test plan says "run the tests", "make sure it works", or adds a test without saying what it should show. A new automated test is not required.

## Honesty

**Where it lives**
- Eval: every claim the Candidate plan section states as fact (in its cause, change, and test parts), plus any risks or unknowns it lists, read against the Repro evidence section. A plan may have no risks part at all.
- Live: the whole draft plan.md, including any risks or unknowns and the ## Deviations section, read against the posted repro comment.

**What good looks like**
Nothing stated as certain is contradicted by the repro evidence, and the plan doesn't guarantee an outcome it can't know yet ("this cannot affect anything else", "this is the only place the bug occurs"). An inference that follows from the repro (a view not refreshing explains a color that only updates on re-entry) is not false confidence, and neither is a side effect the test plan says it will check. A plan doesn't need a risks section to pass. After the build, Deviations says what changed from the plan and why, or states in the author's words that nothing changed.

## Comms

**Where it lives**
- Eval: the Candidate plan comment section, read against the Thread highlights section (maintainer directions and other contributors' plans), the Candidate plan section, and the Repo facts section (bug-report or PR template asks, contribution policy, review limits, AI-use policy).
- Live: the draft comment.md, read against the issue thread on GitHub, the repo's CONTRIBUTING file and any AI-use policy, the house rules in scope.md, and plan.md.

**What good looks like**
The comment describes the same change as the plan, in the author's own words, and responds to what the thread and repo facts ask. It follows a maintainer's stated direction or explains why not, doesn't restate another contributor's plan as its own ("same approach as above"), and respects stated asks (for example, keeping the change minimal when Repo facts say review time is scarce). If Repo facts require disclosure of any AI usage, the comment carries a disclosure statement (tool and extent, or that none was used); silence fails. If Repo facts put a gate on the next step the comment announces (a vouch flow before PRs, issue-first rules), the comment acknowledges it. A convention that's only linked, with its text not shown, isn't held against the comment. Boilerplate looks like a generic "I'll fix this" that could be pasted onto any issue, or a comment that ignores a maintainer's instruction in the thread.
