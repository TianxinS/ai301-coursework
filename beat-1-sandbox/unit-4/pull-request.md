# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/177

**Branch**

fix/64-partial-overlap-fixture

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run: `agreement: 13/20 scored items`. The categories line was `clear-accept 0/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. Every reject category matched, but every gold-accept package was rejected, by over-strict checks: test-plan-covered, test-claims-backed, issue-linked, and description-matches-diff.
2. Full run: `agreement: 16/20 scored items`, with `clear-accept 3/7` and every reject category still matched. This came after I narrowed those four checks to fail only on a real problem. pkg-08, pkg-11, pkg-13, and pkg-19 still failed, all on test-evidence checks.
3. Full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This came after I recalibrated the test checks from "pasted output only" to "a specific observed result vs. a bare verdict". This is the run in the committed `eval-run.txt`.

**Package analysis**

pkg-11 (ripgrep: a `.gitignore` line ending in an escaped space fails to parse). Gold label: `accept`. My rubric's verdict: `reject` in runs 1 and 2 (run 2's table: `pkg-11  clear-accept  accept  reject  NO  failed: repro-before-after`), then `accept` in run 3.

The package's test evidence re-runs the repro, but its after results are notes after `#`, not printed output:

```
$ rg hi                      # after: searches normally, no error
$ rg --files                 # after: 'foo ' not listed (ignored),
                             # matching git check-ignore -v 'foo '
```

In the class activity we treated a `#` note as non-proof, so my repro-before-after check required pasted printed output, and it failed pkg-11. The gold label showed that the line isn't pasted output vs. a note. It's whether the evidence states a specific observed result. "searches normally, no error" and "'foo ' not listed (ignored)" name exactly what was seen, and that is the plan's expected result: "expect no parse error and `foo ` to be ignored". That's different from calib-03's "`# after: matches the issue's expected output byte for byte`", which asserts a verdict without saying what was observed. Once repro-before-after graded "a specific observed result vs. a bare verdict", pkg-11 passed.

**Check rationale**

From `tools/pr-precheck/rubric.md`:

> | repro-before-after | The test evidence, read against the issue's repro steps and expected behavior | The repro steps (or a check that runs the real code with the repro's inputs) are re-run on the fixed code, and the evidence states the specific result observed after the fix, as pasted output or as a description of what was seen. That result is the issue's expected behavior. The before is shown by a before run or by the repro evidence the plan built on. Fail if there is no after run of the repro at all, if the after result is a bare verdict that says nothing about what was observed ("fixed", "works now"), or if the observed result is not the expected behavior. | required |

It reads this way after two revisions. My first version required "each with its printed output" for both the before and the after run, and failed anything "only described". That rejected calib-01, whose before is only `# before: configparser.DuplicateOptionError: ...`, even though the plan's repro evidence already showed that failure. So I let the before come from the repro evidence the plan built on. It also rejected pkg-11 (a `#` note) and pkg-08 (a narrated TUI session with no text output to paste). So I replaced "printed output" with "the specific result observed ... as pasted output or as a description of what was seen". I kept the bare-verdict failure explicit ("fixed", "works now") so the loosening couldn't let unobservable claims through. The not-tested category is defined as evidence that "proves nothing observable", so the condition judges what was observed, not its format.

**Trade-offs**

Loosening repro-before-after and the other test checks risked flipping not-tested packages to accept, because those are exactly the checks the not-tested packages fail on. Instead of a partial canary run, I ran the full set, so all four not-tested packages were re-graded under the loosened checks. pkg-04, pkg-07, pkg-10, and pkg-14 all still came back `reject` (`not-tested 4/4`), which is how I know the loosening didn't let unobservable evidence through. The case I accept the rule will get wrong is a vague description that sounds specific, such as "after: works as expected, no errors": the line between "a specific observed result" and "a bare verdict" is a judgment call, and a grader could read that line either way.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
