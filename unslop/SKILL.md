---
name: unslop
description: >-
  Autonomously clean up code slop in comments, types, naming, error handling,
  and tests when the user requests slop removal or code cleanup. Preserve
  required behavior and public contracts. Exclude explanation-only, summary,
  and bug-review-only requests. Use code-review for bug assessment.
---

# Unslop

Apply concrete non-bug improvements toward the simplest clear implementation that meets the requirements. Prefer removing unnecessary work and indirection. Fewer lines are useful when they preserve intent, readability, and needed safeguards. Adequate code needs no cleanup.

A cleanup request authorizes corrections within the selected scope. Honor read-only or proposal-only restrictions. Classify issues by their consequences, not just their category. Flag suspected bugs for review without silently fixing them. A swallowed error that causes false success is a bug; removing a redundant catch-and-rethrow can be cleanup.

## Decide whether cleanup is needed

Use this skill when slop removal or code cleanup is requested. Answer explanation and change-summary requests directly. A request to review code for bugs does not authorize this cleanup workflow.

For a combined cleanup and review request, complete cleanup first, then use [code-review](../code-review/SKILL.md) on the resulting code and follow its combined output format. A standalone cleanup does not require a separate bug review.

## Establish the scope

Read repository instructions and the latest accepted requirements. Verify the checkout, branch, and working tree. Honor the requested files, component, categories, or change. Without a specified scope, use the current PR or, without one, attributable current-thread work.

- **Files or components:** inspect the named code and relevant callers.
- **PR:** verify the target, base and head SHAs, commits, and changed files. Use the actual PR head and the full provider diff or `git diff <base-sha>...<head-sha>`.
- **Last X commits:** verify a positive X and sufficient history; use `git diff HEAD~X HEAD`, where X counts first-parent commits. Compare against the empty tree when the selection includes the root commit.
- **Thread:** use messages, accepted decisions, tool history, and current files to identify attributable committed and uncommitted work.

Preserve unrelated work. Exclude unrelated uncommitted changes from PR or commit cleanup. Apply edits to the selected snapshot; if it differs from the active checkout, use an isolated checkout at that target. Ask only when missing context prevents establishing scope. Stop if there is no code to clean up.

## Identify cleanup files and affected paths

Enumerate the scoped files and logical changes, including named files with no diff. Inspect relevant callers, consumers, tests, and configuration before editing. Distinguish code inspected for impact from code eligible for cleanup.

Trace dependent references before renaming or changing types, and preserve public names, serialized fields, behavior, and contracts. Update dependent references only as required for the scoped cleanup. Inspecting another file does not authorize unrelated cleanup in it.

Use [blast-radius](../blast-radius/SKILL.md) when a cleanup could affect callers or contracts beyond the edited files. Apply it only to impact analysis and fold the evidence into this cleanup's report.

## Apply cleanup

Assess Comments, Types, Naming, Errors, and Tests in that order, within the user's requested scope. Keep a record of each logical cleanup, its affected files, and whether it was applied.

### Comments

Keep or simplify comments that help future developers understand information not obvious from the code.

Preserve or improve comments that explain:

- why a non-obvious implementation is necessary;
- external API, browser, framework, platform, or protocol quirks;
- business or domain rules;
- invariants and assumptions;
- security or privacy constraints;
- performance trade-offs;
- compatibility requirements;
- unusual implementation choices;
- known limitations with a concrete reason;
- actionable TODO/FIXME items tied to real work;
- `SAFETY:` justifications for unavoidable type assertions.

Remove or rewrite comments that:

- reference the prompt or coding conversation that produced the code;
- say things such as "as requested", "as discussed", "you mentioned", "we decided", or "per your instructions";
- explain what the generating AI chose rather than why the code itself exists;
- narrate obvious syntax or control flow;
- restate the function or variable name in prose;
- contain tutorial-style explanations inappropriate for the surrounding codebase;
- contain speculative TODOs with no concrete requirement;
- describe changes relative to an earlier version instead of documenting the resulting implementation;
- tell a future AI what not to modify.

When an AI-shaped comment contains useful information, rewrite it rather than deleting it. Preserve product and domain terms such as "user", "assistant", and "chat" when they describe the application.

Example:

**BAD**

```ts
// We need this because, as you mentioned, Stripe can send the same event more than once.
```

**GOOD**

```ts
// Stripe may deliver the same event more than once; processing must remain idempotent.
```

### Types

Look for:

- chained type assertions;
- `as unknown as T`, `as any as T`, or equivalent laundering;
- known values widened to `unknown`, `any`, `object`, `{}`, broad `Record`, or anonymous containers and later narrowed again;
- unnecessary explicit annotations that discard useful inference;
- `Record<string, unknown>` or similarly broad dictionaries where a concrete owner/domain type exists;
- `unknown` propagated deep into application code instead of being parsed at an I/O boundary;
- ad-hoc `typeof`, `in`, or shape checking spread through business logic where a boundary parser would be clearer;
- assertions used instead of narrowing, parsing, inference, `satisfies`, or a better API contract;
- aliases that merely hide `unknown`, `any`, `object`, or broad dictionary types.

Prefer:

- inference;
- `as const` where literal preservation is intentional;
- `satisfies` where a value should be checked without widening;
- named domain/owner types;
- parsing untrusted data once at the boundary;
- preserving precise types through the full local flow.

Do not mechanically replace every `unknown`, `typeof`, or assertion. They are valid at real boundaries and in legitimate type guards. Judge them by information flow and context.

### Naming

Look for generic generated names such as:

- `data`, `item`, `result`, `handler`, `manager`, `processor`, `helper`, `utils`, `shape`, or `config` when a domain-specific name is available;
- long names that encode implementation steps rather than domain meaning;
- verbose error messages, docstrings, or UI copy created from chat-like prose.

Rename only when the surrounding code provides clear evidence of the intended domain term.

### Errors

Look for:

- swallowed errors;
- catch-and-rethrow without added context;
- generic "Something went wrong" errors replacing useful underlying information;
- impossible fallback values that let invalid state continue;
- redundant `try` blocks;
- defensive branches added without evidence;
- logging plus rethrowing that causes duplicate logs.

Apply these conventions in TypeScript files:

- Throw standard **`Error`** objects (don't throw strings).
- Handle expected failures in UI with user-friendly messaging; log only when actionable.

For application logging:

- `console.error`: failures that need investigation.
- `console.warn`: recoverable/expected-but-notable issues.
- Avoid `console.log` in committed app code.

Use equivalent levels when the project has a structured logger. Before recommending a conversion to `Error`, inspect consumers that compare or serialize the thrown value. Apply behavior-preserving logging cleanup; flag correctness or diagnostic defects for review under the bug boundary above.

Preserve intentional boundary translation, cleanup/finally behavior, retry policy, and security-sensitive redaction.

### Tests

Look for:

- excessive module mocking;
- `vi.mock`, `jest.mock`, or equivalent where a real dependency seam is practical;
- tests coupled to implementation details instead of observable behavior;
- assertions that cannot fail meaningfully;
- giant setup blocks created by generated abstractions;
- duplicated test setup that obscures scenarios;
- tests for trivial language/framework behavior rather than project behavior;
- snapshots used where a focused assertion is clearer.

Prefer real interfaces, injected dependencies, lightweight fakes, and behavior-level assertions.

Do not rewrite a stable, established testing architecture merely to satisfy a preference. Focus on slop introduced in the selected scope. Preserve meaningful behavioral coverage; do not weaken assertions or change expected results merely to make tests pass.

## Verify

Inspect the final diff to confirm each edit belongs to the requested cleanup and preserves required behavior and public contracts. Run checks proportionate to affected behavior and complete required repository checks. Reuse applicable results and recheck affected files or references that change during cleanup.

Fix or withdraw cleanup that introduces a regression. Preserve unrelated existing changes when doing so. Do not weaken tests to accommodate a broken cleanup. Record any checks that could not run and any behavior that remains unverified; a passing build does not prove browser or live-service behavior.

Reconcile applied changes with the final diff and update file links and line numbers. Mark the cleanup partial if scoped work remains unassessed.

## Output

For all changes and cleanup, use a short opening summary followed by **Slop**. Count unique files actually assessed and logical changes actually applied, not edited lines. Use the following format and link the affected file locations. Choose one primary category and include all affected locations. Keep the section when nothing needs changing.

````markdown
Assessed [N] files; made [M] changes.

| #                    | File      | Category                           |
| -------------------- | --------- | ---------------------------------- |
| [#](link-to-section) | File:Line | Comments/Types/Naming/Errors/Tests |

## Slop

[Very short summary of slop found and changes made.]

[Separate every change with a divider `---`]

[#]. [Single sentence description]

File:Line

**Problem**: What's wrong and why changing it improves the code.

```[code_format]
[offending code (truncated if long)]
```

**Change**: What was fixed.

```[code_format]
[fixed code]
```
````

When describing problems, be concrete.

**BAD**

> This helper is overengineered. Refactor it.

**GOOD**

> order-labels.ts:18 creates temporary objects only to read their label field in a second map.

For combined cleanup and review, pass these concerns and cleanup results into code-review's combined report instead of producing a second report.

```

```

```

```
