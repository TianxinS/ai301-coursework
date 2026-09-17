# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|-------|--------------------|-----------------|--------|
| Maintainer Active | Last push date, last 5 default-branch commits, and the maintainer first-response sample | A maintainer/owner/collaborator has committed, merged, or responded within roughly the last 3 months, and recent activity is not bot/dependency-bump-only | required |
| Scope Fits | The issue body, any suggested fix, and the comment thread | The issue describes one coherent unit of work with an understood problem and outcome — a single bug, a single feature, or a single docs topic — even if addressing it touches several files or has several sub-steps, as long as all of them serve that one task. A clear description of expected vs. actual behavior (from anyone, maintainer or not) is enough on its own: it does not need to name an exact line/function, include reproduction steps, or propose a fix. Suggested implementation approaches ("we could do X, or Y") are guidance, not unresolved scope. Fail when: the issue is explicitly a tracking/umbrella issue meant to be split across many contributors, or lists multiple unrelated work items; the thread shows a long-running (multi-year) unresolved debate over *what* to build with no settled spec; or the ask is a short feature wish whose shape still depends on a product decision nobody has made | required |
| Uncontested | Assignees field, linked PRs field, and any ownership claims in the comment thread | No assignee is currently set, and no PR is currently open against the issue — a formally linked or thread-mentioned PR that is closed or abandoned does not count as a live claim. No comment claims ownership that is still live: a claim is live if it's recent relative to the capture date, or a maintainer/bot is still treating it as active. An old claim that never produced a merged or still-open PR, with no follow-up activity since, does not block the issue | required |
| AI Contribution Allowed | The repo's stated contribution policy (CONTRIBUTING.md / AI policy files, or the "contribution policy" line in an eval bundle) | The policy does not outright ban AI-generated code, documentation, or contributions. Conditions — disclosure, human review, personal understanding, testing — are terms to follow, not a ban, and pass. Silence on AI/contribution tooling passes. Only an explicit statement that AI-generated contributions are not accepted fails | required |

## Verdict rule

- **accept**: every `required` check grades `pass`
- **reject**: any `required` check grades `fail`, or any `required` check grades `unclear` (unclear is treated as fail — a first issue you cannot verify is not one you should take)
- `preferred` checks (none defined yet) never affect the verdict; they only rank accepted issues by fit.

## Notes

- Always check the "linked PRs" field *and* scan the comment thread itself — some ownership claims (e.g., "opened PR #XXXX") show up only in comments, not in repo-facts metadata.
- For Uncontested, check the *state* of every linked/mentioned PR and the *recency* of every claim, not just their existence: a closed PR and a claim with no follow-up in a year or more is a dead end someone else walked away from, not a live claim on the issue.
- Recent-looking maintainer activity that's dominated by bots (e.g., dependabot auto-merges) should be weighted cautiously for Maintainer Active — look for actual human owner/collaborator engagement.
- Treat the issue title with suspicion; base every check on the repo-facts block, issue body, and comment thread.
