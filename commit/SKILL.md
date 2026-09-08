---
name: commit
description: Commit local work to Git. Use whenever the user asks to commit work, including splitting changes into multiple commits.
---

# Commit

Confirm the repository, branch, and working directory. Review staged and unstaged diffs and relevant untracked files to establish scope. A commit request authorizes local commits within that scope. Preserve unrelated work and deliberate partial staging.

Group changes by purpose, honoring the user's requested grouping. Use one commit for a coherent change; split unstaged work into separate commits when distinct messages clarify the history. Stage by file or hunk, keep required tests and documentation with each change, and commit dependencies first.

Use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/): `type: description` or `type(scope): description`, following repository conventions for scopes and allowed types. Match the type to the commit's purpose: `fix`, `feat`, `perf`, `refactor`, `docs`, `test`, `build`, `ci`, `style`, `chore`, or `revert`. Use `style` for code formatting and `chore` for maintenance with no more specific type. Mark breaking changes with `!` before the colon and explain the incompatibility in the body.

Write a concise subject that states the problem solved or useful outcome. Add a body when the reason or a tradeoff needs explanation.

**BAD**

> feat: add sidebar slice logic and expanded state

**GOOD**

> feat(sidebar): keep sections compact with a five-item preview

Before each commit, review the complete staged diff for scope and agreement with its message. Run appropriate checks, then commit with hooks enabled. Fix check or hook failures within scope and rerun; report blockers requiring unrelated changes.

Inspect the resulting commits and working-tree status. Report hashes, subjects, checks run, and uncommitted work. Pushing, amending existing commits, and rewriting history require separate authorization.
