---
name: debug-loop
description: Run a bounded bug-fix and independent re-review loop when the user asks for a debug loop. Use existing findings or start with a review.
---

# Debug loop

The main agent owns repairs. One independent reviewer uses [code-review](../code-review/SKILL.md) to find and recheck bugs. Invoking this workflow authorizes local fixes to confirmed bugs in scope. Honor a read-only restriction and return findings if it applies. Commit, push, and PR updates follow the user's separate authorization.

1. **Pin the scope.** Identify the requested PR base and exact head, local revision range, or attributable working-tree changes, including relevant untracked files. Preserve unrelated changes. Keep the base fixed and give each review a reproducible snapshot. Record which checks pass, fail, or could not run.
2. **Choose the reviewer.** Use the requested model and effort. The default reviewer is the latest available GPT Sol. Resolve it from the host's capabilities. In T3, discover capabilities once and use a fresh review delegate for each round. Elsewhere, use a supported independent agent running the selected model. Report an unavailable selection to the user before substituting another.
3. **Start from the evidence.** If findings were supplied, keep their IDs, confirm they apply to this snapshot, and repair agreed bugs before the first recheck. For a partial or older review, identify any uncovered scope. If there is no applicable review, begin with a read-only review of the requested scope.
4. **Brief one reviewer per round.** Give them the intent, scope, checkout, base and head or working diff, relevant checks, and findings with their status. Request read-only findings, without further delegation or PR comments. Keep the snapshot stable until they return. Use the host's completion notification when available.
5. **Fix and recheck.** Confirm the trigger and impact of each finding. Make the smallest correction that fixes it and run the affected checks. Record fixed, disproved, or out-of-scope findings with evidence. For the next round send the delta, open IDs, and affected callers. Ask the reviewer to verify closure and look for regressions introduced by the repairs.
6. **Stop on evidence.** Finish when the independent review covers the current behavior, the applicable checks pass, and all confirmed in-scope bugs are resolved. The default budget is three reviewer rounds including the first review or recheck. Extend to five if the user requests it or a material security, data-loss, or concurrency issue remains open. Stop a repeated dispute after two replies and report both positions. A failed review, exhausted budget, or unreviewed final fix leaves an explicit gap.

Report the reviewer model and effort when known, rounds used, findings and dispositions, checks, and any remaining gaps. Claim independent approval only for the revision or working diff the reviewer actually cleared.

When an authorized push updates a PR after reviewer approval, record the round count and actual reviewer above the existing model attribution. Include effort and usage only when reported.
