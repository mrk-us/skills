---
name: implement
description: >-
  Implement features, bug fixes, refactors, and maintenance changes to code,
  tests, dependencies, or configuration. Exclude review-only, planning-only,
  and explanation requests.
---

# Implement

Complete the requested code change through implementation, relevant verification, and corrections within scope. Use the user's requested outcome and accepted decisions to establish what must work before handing it back.

## Working context

For a new task without an assigned task worktree, fetch `origin` and create a new worktree and branch from `origin/main`. Name the branch `<type>/<short-kebab-description>`, for example `feat/sidebar-preview` or `fix/invoice-total`. Choose an unused branch name and a worktree path outside the current checkout. Work from that directory and verify its branch before editing. Preserve existing changes. Reuse the existing worktree and branch when continuing the same task or when the host has already assigned them to this task.

Choose the type that matches the task's main purpose. Common types are:

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

## Approach

Use [file-structure](../file-structure/SKILL.md) when implementing or refactoring features, or when creating, moving, or extracting files and helpers. Apply its ownership guidance within the requested scope and existing repository conventions.

Apply the distinctions relevant to the change; a task can span several kinds of work.

- **Features**: deliver the requested behavior across the affected flow, including relevant failure states.
- **Bug fixes**: establish the cause and verify the failing case is resolved. Add a regression test when it provides a useful, repeatable safeguard.
- **Refactors**: preserve required behavior and public contracts while simplifying the affected code.
- **Maintenance**: address the concrete dependency, configuration, tooling, or cleanup need within the requested scope.

Read specialized guidance when the changed behavior needs it. Keep broader audits and independent reviews conditional on the request, repository requirements, or a concrete concern.

## Self-review

Ask of every change:

- Does this solve the actual problem and preserve required behavior?
- Does this leave the codebase better off than when you found it?
- Is this the simplest solution within the existing design?
- Can this be achieved in less code?
- Does each new abstraction, dependency, fallback, and test earn its place?
- Do comments explain useful reasons or constraints, with redundant narration and references to the coding conversation removed?
- Do files and helpers have clear owners and locations, with extraction justified by a present responsibility or useful boundary?

## Verification and completion

Run checks appropriate to the changed behavior and complete the repository's required checks. Once they pass, repeat or broaden verification only when new changes, failures, or unresolved concerns justify it.

Fix failures caused by the change and rerun affected checks. If completion is blocked by unrelated work or unavailable services, explain the blocker and what remains unverified.

Summarize the changed behavior, checks run, and remaining gaps. When committing is part of the authorized task, follow [commit](../commit/SKILL.md). For an authorized push, PR, merge, or local synchronization, follow [wrap-up](../wrap-up/SKILL.md) through the requested endpoint.
