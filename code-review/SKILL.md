---
name: code-review
description: >-
  Review code for correctness, simplicity, unnecessary comments, slop, file
  ownership, and repository standards when a code-quality assessment or
  actionable review findings are requested. Exclude investigations that only
  summarize functionality, explain behavior, or inventory changes in branches,
  PRs, commits, or files. Ordinary implementation and cleanup requests use implement.
---

# Code review

Find concrete improvements toward the simplest clear implementation that meets the requirements. Prefer removing unnecessary work and indirection. Fewer lines are useful when they preserve intent, readability, and needed safeguards. An adequate implementation needs no cleanup finding.

Apply fixes only when requested. A request to review and fix authorizes corrections within the selected scope.

## Decide whether a review is needed

Choose by the requested deliverable. Reading code or a diff, inspecting a PR, or investigating a branch does not by itself call for a code review. Use this skill for a requested code-quality assessment or a review required by repository instructions.

| Request | Handling |
| --- | --- |
| "What does this branch touch? What's changed?" | Explain affected areas and behavior changes without invoking this skill. |
| "Investigate this PR and summarize its functionality." | Trace and summarize the functionality without invoking this skill. |
| "Review this PR for bugs and unnecessary complexity." | Use this skill to evaluate the changes and report actionable findings. |

If this skill was loaded for an explanation-only request, stop applying the review workflow and answer the original question. For a combined summary and review request, apply the review workflow only to the requested assessment.

## Establish the scope

Verify the checkout, branch, and working tree. Honor the requested file, component, or change before choosing a broader scope. Without a specified scope, use the current PR or, without one, attributable current-thread work.

- **Files or components:** inspect the named code and relevant callers without expanding into unrelated cleanup.
- **PR:** verify the target, base and head SHAs, commits, and changed files. Review the full provider diff or `git diff <base-sha>...<head-sha>`. Use the actual PR head, which may differ from local HEAD.
- **Last X commits:** verify a positive X and sufficient history; review `git diff HEAD~X HEAD`. X counts first-parent commits. Compare against the empty tree when the selection includes the root commit.
- **Thread:** use messages, accepted decisions, tool history, and current files to identify attributable work across turns and repositories, including committed and uncommitted changes.

Exclude unrelated work and, in PR or commit mode, uncommitted changes unless requested. Ask only when missing context prevents establishing scope. State remaining gaps; stop if there is no code or change to review.

## Review the code

Read repository instructions, relevant neighboring code, and the latest accepted requirements. Apply repository standards over general preferences. Inspect every scoped file and logical change, following inputs, state, side effects, and failure paths through relevant callers, tests, and configuration. Check required behavior, regressions, and unnecessary scope additions alongside the quality criteria below.

Validate candidate findings against current code and their concrete consequences. For simplification findings, identify a smaller or clearer alternative and why it improves the actual flow. Discard unsupported findings and unrelated pre-existing issues.

### Simplicity and ownership

Look for unnecessary wrappers, forwarding functions, intermediate objects, repeated transformations, duplicated decisions, and abstractions, dependencies, or compatibility paths without a current purpose. Simplify locally before introducing another abstraction. Keep boundaries that carry real policy, domain meaning, or dependency isolation.

Keep single-use helpers near their consumer and responsibilities with their owner. Extract for a present shared responsibility or a meaningful boundary; similar syntax and smaller files alone do not justify it. Follow existing layout and preserve self-contained examples. Read [file-structure](../file-structure/SKILL.md) for guidance when ownership, placement, or extraction needs deeper review.

### Comments

Remove redundant narration, stale code, speculative TODOs, and references to the agent conversation or generation process. Keep concise explanations of non-obvious reasons, domain rules, invariants, platform quirks, security, performance, and compatibility constraints, including necessary safety justifications and actionable TODOs.

Rewrite useful information out of conversational wording. Product terms such as user, assistant, and chat are valid when they describe the application.

**BAD**

```ts
// We need this because, as you mentioned, Stripe can send the same event more than once.
```

**GOOD**

```ts
// Stripe may deliver the same event more than once; processing must remain idempotent.
```

### Types and names

Preserve useful inference and precise domain types. Parse untrusted data at the boundary. Flag assertion chains, `any`/`unknown` laundering, broad dictionaries replacing known contracts, and repeated shape checks in business logic. Keep legitimate boundary types, type guards, and necessary assertions; use narrowing, `satisfies`, or a better contract when they remove the need for an assertion.

Choose concise names from the surrounding domain. Rename only when meaning becomes clearer; familiar local names do not need mechanical expansion.

### Errors and tests

Flag swallowed errors, catch-and-rethrow without value, invented fallback state, unjustified defensive branches, and duplicate logging. Preserve useful context, boundary translation, cleanup, retries, and security-sensitive redaction.

Tests should protect observable project behavior with meaningful assertions and proportionate setup. Prefer real interfaces or lightweight fakes where practical. Flag excessive mocking, implementation-coupled assertions, redundant snapshots, and tests of trivial framework behavior. Preserve established test architecture unless it causes a concrete problem in scope.

## Verify and report

Scale verification and independent review to the scope and risk. Use additional reviewers only when independent inspection adds value and delegation is allowed. Share the same scope and evidence and reconcile their findings. Run focused non-destructive checks where useful, reuse applicable results, and recheck files or refs that change during review. Browser or computer control requires explicit authorization.

State the scope and present actionable findings ordered by impact. Each needs a file and line, evidence, consequence, and the smallest useful correction. Distinguish repository-rule violations from judgment calls. Use separate Standards and Spec sections only when they help.

**BAD**

> This helper is overengineered. Refactor it.

**GOOD**

> order-labels.ts:18 creates temporary objects only to read their label field in a second map. Map directly to the label to remove the extra allocation and transformation.

Finish with checks run and verification gaps. Say when there are no findings. Mark the review partial when any scoped file, required behavior, or material concern remains unassessed, and identify it. A passing build does not verify appearance or interaction. Keep detailed coverage notes conditional on the request or review complexity.
