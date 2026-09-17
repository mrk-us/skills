---
name: commit
description: Commit local work to Git. Use whenever the user asks to commit work, including splitting changes into multiple commits.
---

# Commit

Create local commits for the requested work.

## Establish scope and authorization

Confirm the repository, branch, and working directory. Review staged and unstaged diffs and relevant untracked files to establish scope. A commit request authorizes local commits within that scope. Preserve unrelated work and deliberate partial staging.

A commit-only request does not authorize pushing, amending existing commits, or rewriting history. Honor authorization already given for those operations in the current task.

## Group and stage changes

Group changes by purpose, honoring the user's requested grouping. Use one commit for a coherent change; split unstaged work into separate commits when distinct messages clarify the history. Stage by file or hunk, keep required tests and documentation with each change, and commit dependencies first.

## Write the commit message

**Type and scope**

Use `type: description` or `type(scope): description`, following repository conventions for scopes and allowed types. Include a scope when it helps identify the affected area, such as `feat(sidebar):`. Use `!` before the colon for a breaking change and explain the incompatibility in the body.

Choose the type that matches the commit's main purpose. Common types are:

- `fix`: bug fixes.
- `feat`: new features or capabilities.
- `perf`: performance improvements.
- `refactor`: code restructuring that preserves behavior.
- `docs`: documentation changes.
- `test`: test additions or corrections.
- `build`: build tooling or dependency changes.
- `ci`: continuous integration configuration or scripts.
- `style`: code formatting, such as whitespace or semicolons.
- `chore`: maintenance that fits none of the more specific types.
- `revert`: undoing an earlier change.

**Subject**

Write a concise, human-readable subject that states the problem solved or the useful outcome. It should make sense without the commit body:

**BAD**

> feat: add sidebar slice logic and expanded state

**GOOD**

> feat(sidebar): keep sections compact with a five-item preview

**Body**

Add a body when the reason, a tradeoff, or a breaking change needs explanation. Open with the problem and what this commit changes to solve it. Use plain English and keep the explanation understandable without the chat context:

**BAD**

> Added a five-item slice to each sidebar section, tracked expanded state, and wired up a "View more" click handler.

**GOOD**

> Long sidebar sections pushed other sections out of view. Each section now shows up to five items, with a "View more" option to reveal the rest.

## Verify and commit

Before each commit, review the complete staged diff for scope and agreement with its message. Run appropriate checks, reusing passing results when they cover the same content and environment. Rerun when changes, failures, or unresolved concerns justify it, and complete repository-required checks. Commit with hooks enabled. Fix check or hook failures within scope and rerun; report blockers requiring unrelated changes.

## Hand off the result

Inspect the resulting commits and working-tree status. Report hashes, subjects, checks run, and uncommitted work.
