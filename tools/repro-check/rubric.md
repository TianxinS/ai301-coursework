# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| claim-specific | The claim comment, read against the issue's title and body | The claim names at least one detail specific to this issue (the failing behavior, the file/test/function, or the command the issue gives) so that it could not be pasted onto a different issue unchanged, and it is written in the author's own words rather than deferring to another commenter ("same as above", "+1, can confirm"). Fail on a generic "I'd like to work on this" or a piggyback. | required |
| claim-promises-investigation | The claim comment, read against voice-guide.md and against the repro report if one is in the package | The claim commits only to investigating/reproducing and reporting back. Fail if it promises a fix, a PR, or a date, or asserts a result (e.g. "I've confirmed the bug") that no repro report in the package backs. | required |
| env-recorded | The repro report's environment record, read against the repo-facts block and the repo's setup docs | The report states the environment the result depends on: OS, language/runtime version, the repo commit or version tested, and any dependency or config the issue's behavior involves, specifically enough that a stranger could rebuild the same setup. Fail on "latest", "my machine", or a missing commit/version. | required |
| steps-rerunnable | The repro steps in the report, read as if starting from a fresh clone of the repo | A stranger starting from a fresh clone could reach the observed result by following the report, with no command, input file, argument, or config value they would have to guess. Judge whether anything the result depends on is missing, not how many steps there are. | required |
| behavior-matches-issue | The output excerpt/artifacts in the repro report, read against the error or wrong behavior the issue describes | The report shows actual output (pasted terminal output, test result, traceback, screenshot text), and that output displays the same failure the issue describes: the same error, wrong value, or symptom in the same place. Fail if the output shows an adjacent problem (a setup error, a different test, a different exception) or if the behavior is only described in prose ("it failed") without output shown. | required |
| outcome-honest | The report's stated conclusion, read against its own artifacts | The stated outcome (reproduced / could not reproduce / partially reproduced) is exactly what the artifacts show. An evidenced cannot-reproduce (what was run, what was observed instead) passes. Fail if the report claims reproduction its artifacts do not show, or states a root cause or fix as fact without evidence. | required |
| conventions-disclosure | The repo-facts block (contribution policy, AI-assistance policy, comment conventions), read against every comment in the package | If the repo's stated policy requires disclosing AI assistance, every comment in the package contains that disclosure in the form the policy asks for, and any other stated comment conventions are followed. If the repo-facts block states no such policy, pass. If the policy is referenced but its text is not visible (truncated or linked only), grade unclear. | required |

## Verdict rule

Checks apply by package part: claim-specific, claim-promises-investigation, and conventions-disclosure apply to every package. env-recorded, steps-rerunnable, behavior-matches-issue, and outcome-honest apply only when the package contains a repro report; on a claim-only draft they are reported as "not yet applicable" and do not count toward the verdict.

Accept (ready) if every applicable required check passes. Reject (hold) if any applicable required check fails or is unclear: evidence that is not visible in the package is not proof, so unclear counts as fail. There are no preferred checks, so no check is ignored in the verdict.
