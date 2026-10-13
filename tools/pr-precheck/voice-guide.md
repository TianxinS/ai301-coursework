# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

I'm a data scientist making my first open-source contributions, working through Path Review issues as part of a course. I'm new to this codebase, so I say plainly what I've checked and what I haven't. Readers can expect me to report back with evidence, not guesses.

## Rules I write by

### Rule: Promise the investigation, not the fix

Before I've reproduced anything, I only commit to looking into it and reporting back. No fixes, PRs, or dates.

- Wrong: "I'll have a fix for this up by Friday."
- Right: "I'm going to try to reproduce this locally and will post what I find here."

### Rule: Don't claim what I haven't seen

I only state as fact what I've actually observed. Anything else is labeled as a guess.

- Wrong: "The assertion is wrong because the expected value is outdated."
- Right: "I haven't run it yet, but the expected value in the assertion looks like it might be outdated. I'll check when I reproduce."

### Rule: Show the output, don't describe it

When I report a result, I paste the actual output instead of summarizing it.

- Wrong: "I ran the test and it failed like the issue says."
- Right: "Running `<command from the issue>` on commit `<sha>` gives: [pasted output]"

### Rule: Say what I don't know

If I'm unsure about something in the repo, I ask or flag it instead of guessing past it.

- Wrong: "Set up the environment and ran it."
- Right: "I followed the README setup; I wasn't sure whether the tests need the optional dependencies, so I installed them. Happy to re-run without if that matters."

### Rule: My own words, even on a shared issue

If someone else has already claimed or reproduced the issue, I still post my own claim and my own evidence.

- Wrong: "Same as above, can confirm."
- Right: "I also reproduced this on macOS 14 / Python 3.11. My output is below, and it matches the error in the original report."

## Things I never post

- A fix date, a PR promise, or "this should be easy."
- "Can confirm" or "+1" without my own output.
- A claim that I reproduced something when my output shows a different error.
- A root cause stated as fact before I've tested it.
- A comment written with AI help on a repo that requires disclosure, without that disclosure.
- Apologies for being new. I say what I know instead.
