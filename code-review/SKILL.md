---
name: code-review
description: >-
  Review code for bugs, regressions, security issues, type-safety defects, and
  error-handling failures when a code-quality assessment or actionable review
  findings are requested. Report findings without editing by default. Exclude
  explanation-only and change-summary requests. Use unslop for autonomous
  non-bug cleanup.
---

# Code review

Find actionable defects in the selected code and explain their concrete consequences. Review correctness, security, type safety, error handling, and required behavior. Report bugs without modifying the reviewed code unless the user explicitly requests fixes.

A demonstrated loss of type checking, debugging information, or accurate user-facing error behavior can be a finding without an observed runtime crash. Cosmetic type preferences, comment hygiene, and naming cleanup belong to [unslop](../unslop/SKILL.md).

## Decide whether a review is needed

Choose by the requested deliverable. Reading code or a diff, inspecting a PR, or investigating a branch does not by itself call for a code review. Use this skill for a requested code-quality assessment or a review required by repository instructions.

| Request                                                | Handling                                                                          |
| ------------------------------------------------------ | --------------------------------------------------------------------------------- |
| "What does this branch touch? What's changed?"         | Explain affected areas and behavior changes without applying this workflow.       |
| "Investigate this PR and summarize its functionality." | Trace and summarize the functionality without applying this workflow.             |
| "Review this PR for bugs."                             | Review the code and report actionable findings.                                   |
| "Remove slop from these changes."                      | Use unslop for the requested cleanup.                                             |
| "Clean up and review these changes."                   | Run unslop first, then review the resulting code and produce the combined report. |

For a combined summary and review request, apply this workflow only to the requested assessment. Invoke unslop only when cleanup is part of the request, not merely because a review finds slop.

## Establish the scope

Read repository instructions and the latest accepted requirements. Verify the checkout, branch, and working tree. Honor the requested file, component, or change before choosing a broader scope. Without a specified scope, use the current PR or, without one, attributable current-thread work.

- **Files or components:** inspect the named code and relevant callers.
- **PR:** verify the target, base and head SHAs, commits, and changed files. Review the full provider diff or `git diff <base-sha>...<head-sha>`. Use the actual PR head, which may differ from local HEAD.
- **Last X commits:** verify a positive X and sufficient history; review `git diff HEAD~X HEAD`. X counts first-parent commits. Compare against the empty tree when the selection includes the root commit.
- **Thread:** use messages, accepted decisions, tool history, and current files to identify attributable work across turns and repositories, including committed and uncommitted changes.

Exclude unrelated work and, in PR or commit mode, uncommitted changes unless requested. For a combined cleanup and review, include the cleanup produced for the selected target and identify the resulting working tree as part of the reviewed snapshot. Ask only when missing context prevents establishing scope. State remaining gaps; stop if there is no code or change to review.

## Identify review files and blast radius

Enumerate every scoped file and logical change, including named files with no diff. Trace affected callers, consumers, tests, configuration, and data contracts, including unchanged code. Inspect relevant neighboring code and follow inputs, state, side effects, and failure paths across those boundaries. Apply repository standards over general preferences.

Use the [blast-radius](../blast-radius/SKILL.md) skill for impact analysis within this scope. Merge its evidence into this skill's output. If it or a referenced helper is unavailable, inspect affected paths directly and state any limits. Keep verification within the user's authorization; use temporary checks without changing reviewed files for a read-only review.

Identify affected paths before assessing bugs, and revisit the file selection when a finding exposes another affected consumer. Inspecting a dependency does not expand the review into unrelated pre-existing issues.

## Current and recommended changes

Apply every question below to both the code being reviewed and every change recommended to that code during this review. This includes suggested bug fixes and recommendations left unapplied. Evaluate each recommendation before reporting it and each authorized fix before applying it.

- Does this solve the actual problem and preserve required behavior?
- Does this leave the codebase better off than when you found it?
- Is this the simplest solution within the existing design?
- Can this be achieved in less code?
- Does each new abstraction, dependency, fallback, and test earn its place?

Judge simplicity by ease of understanding and maintenance. Recommend fewer lines only when they preserve intent and useful safeguards. Prefer the smallest correction that addresses the demonstrated defect; adequate code needs no finding.

## Review the code

Inspect every scoped file and logical change. Compare the implementation with accepted requirements and, for a change review, its prior behavior. Check required behavior, regressions, and unnecessary scope additions. Investigate security vulnerabilities first, then other correctness failures.

For every scoped file and relevant affected path, identify:

- All user inputs (request params, headers, body, URL components)
- All database queries
- All authentication/authorization checks
- All session/state operations
- All external calls
- All cryptographic operations

Assess every checklist item and the focused review areas below for applicability. Record each as **Issue found**, **Checked**, **Not applicable**, or **Unverified**, with brief supporting evidence or a reason. **Checked** means assessed using the stated evidence, not necessarily exercised at runtime.

### Security and business logic

- Injection: SQL, command, template, header injection
- XSS: All outputs in templates properly escaped?
- Authentication: Auth checks on all protected operations?
- Authorization/IDOR: Access control verified, not just auth? Tenant and ownership boundaries preserved?
- CSRF: State-changing operations protected?
- Race conditions: TOCTOU in any read-then-write patterns?
- Session: Fixation, expiration, secure flags?
- Cryptography: Secure random, proper algorithms, no secrets in logs?
- Information disclosure: Error messages, logs, timing attacks?
- DoS: Unbounded operations, missing rate limits, resource exhaustion?
- Business logic: Edge cases, state machine violations, numeric overflow?

### Type safety

Trace values from their source through validation, inference, domain types, and consumers. Inspect type slop for a concrete loss of checking or a mismatch with runtime values:

- chained assertions, `as unknown as T`, `as any as T`, non-null assertions, and suppressions that hide incompatible or absent values;
- known values widened to `any`, `unknown`, `object`, broad records, or aliases that erase a meaningful contract;
- incorrect generics, type guards, or annotations that claim stronger guarantees than the implementation provides;
- external data asserted into a trusted type without the required boundary validation;
- optional, nullable, union, and serialized values that consumers handle as if they were a narrower type.

For a finding, show the invalid value or operation the type allows, the useful check that was lost, or the violated contract and affected consumer. A minimal compiler example can establish a type-safety defect without a runtime reproduction. Keep legitimate boundary types, narrowing, assertions, and inference choices when their guarantees hold.

### Error handling and debugging

Trace failures from their origin through callers, cleanup, retries, logging, and the user-facing result:

- swallowed errors, false success responses, or fallback values that let invalid state continue;
- lost causes, stacks, error codes, or useful context that prevent diagnosing a concrete failure;
- rejected promises or unsuccessful external responses that the caller never observes or handles;
- incorrect error classification, retry behavior, cancellation, timeouts, or cleanup that masks the original failure;
- misleading user messages, incorrect status codes, missing recovery feedback, or errors that leave the UI stuck;
- duplicate reporting that has a concrete operational consequence, or sensitive details exposed to users or logs.

Apply these conventions in TypeScript files:

- Throw standard **`Error`** objects (don't throw strings).
- Handle expected failures in UI with user-friendly messaging; log only when actionable.

For application logging:

- `console.error`: failures that need investigation.
- `console.warn`: recoverable/expected-but-notable issues.
- Avoid `console.log` in committed app code.

Use equivalent levels when the project has a structured logger. Before recommending a conversion to `Error`, inspect consumers that compare or serialize the thrown value. Logging hygiene alone belongs to unslop; report concrete diagnostic failures, misleading feedback, or sensitive-data exposure here.

Judge technical diagnostics separately from public messages. A generic public error can be intentional when protected diagnostics retain useful context. Preserve security-sensitive redaction, boundary translation, cleanup/finally behavior, and intentional retry policy.

### Async behavior, data, and compatibility

Check the relevant failure modes in the affected flow:

- stale responses, overlapping operations, cancellation, lifecycle cleanup, and side effects that can run more than once;
- partial writes, transaction boundaries, retries, duplicate deliveries, and idempotency;
- API contracts, persisted data, migrations, configuration, and old consumers that must remain compatible;
- empty, missing, boundary, and malformed values, including numeric precision or time handling when the code depends on them.

Establish the actual ordering, constraints, and supported inputs before reporting an edge case. Avoid inventing callers or hypothetical compatibility requirements.

### Tests and behavioral evidence

Check whether the relevant tests exercise the affected behavior and failure paths. Inspect mocks, fixtures, assertions, and asynchronous completion for ways a test can pass while the behavior is broken. Compare changed expectations with requirements so a rewritten assertion does not hide a regression.

A missing test alone is not proof of a bug. Use existing tests and focused reproductions where they help establish the failure. Test-structure cleanup belongs to unslop unless the test defect demonstrably defeats a required safeguard.

## Validate findings

For each candidate finding:

1. Establish the trigger, affected execution path, expected behavior, actual consequence, and relevant file locations.
2. Check whether the issue is handled elsewhere, including unchanged middleware, callers, boundary validation, or database constraints.
3. Inspect existing tests and run a focused reproduction or check when it materially improves confidence. Use the actual pinned dependency or its source when the claim depends on library behavior.
4. Reconcile the proposed fix with the questions in **Current and recommended changes** and check its affected consumers.
5. Discard disproven, unsupported, duplicate, and unrelated findings. Keep unresolved material concerns as verification gaps rather than counting them as confirmed bugs.

For change reviews, distinguish defects introduced or exposed by the changes from unrelated pre-existing issues. For named-file reviews, assess the requested code even when it has no recent diff. Report each root cause once, listing its affected locations.

Run checks proportionate to scope and risk, complete required repository checks, and reuse applicable results. Recheck files or refs that change during review. A passing build or typecheck does not prove browser, interaction, or live-service behavior. Mark the review partial when any scoped file, required behavior, or material concern remains unassessed.

Assign severity from the demonstrated impact and reachable conditions:

| Severity | Meaning                                                                                                                |
| -------- | ---------------------------------------------------------------------------------------------------------------------- |
| Critical | Broad compromise, catastrophic data loss, or failure of a critical system with no reasonable mitigation.               |
| High     | Serious security exposure, data corruption, or failure of a core supported workflow.                                   |
| Medium   | A supported case fails or a concrete type-safety, diagnostic, or recovery defect materially weakens the affected flow. |
| Low      | A localized correctness or diagnostic defect with limited impact.                                                      |

An unsafe-looking pattern or theoretical worst case alone does not justify severity. If there are no substantiated bugs, say so and retain any verification gaps.

## Output

Use a short opening summary followed by **Bugs**. Count unique files actually reviewed, including relevant affected files, and each distinct confirmed bug once. Severity counts must equal the total. Keep the section when there are no findings.

```markdown
Reviewed [N] files; found [B] bugs ([C] Critical, [H] High, [Md] Medium, [L] Low).

## Bugs

Found [B] bugs across [F] files.

| #                    | File      | Severity                 | References                                    |
| -------------------- | --------- | ------------------------ | --------------------------------------------- |
| [#](link-to-section) | File:Line | Critical/High/Medium/Low | Supporting code, test, or applicable standard |

[Detailed breakdown of each bug seperated by a divider `---`]

[Brief scope, checklist coverage, checks run, and verification gaps.]
```

For each bug, use the following format. Link file locations and relevant supporting sources. Omit unsupported standards references rather than inventing them.

````markdown
[#]. [Single sentence description]

File:Line

**Problem**: What's wrong and its concrete consequence.

```[code_format]
[offending code (truncated if long)]
```
````

**Fix**: The smallest recommended correction, assessed against Current and recommended changes.

```[code_format]
[fixed code]
```

**Evidence**: The trigger, affected path, and evidence establishing the defect. State whether the evidence is static inspection, a compiler check, a test, or a live reproduction.

````

Keep scope and verification notes inside **Bugs**. Report coverage compactly using the recorded statuses; do not turn unverified areas into clean results. When fixes were explicitly requested, state what was applied and verified without presenting it as an unresolved bug.

### Combined cleanup and review

When the user requests both tasks, run unslop first and review the resulting code. Validate any suspected bugs from cleanup against that result. Produce one report using this structure and the respective per-entry formats:

```markdown
Reviewed [N] files; made [M] changes; found [B] bugs ([C] Critical, [H] High, [Md] Medium, [L] Low).

## Slop

| #                    | File      | Category                           |
| -------------------- | --------- | ---------------------------------- |
| [#](link-to-section) | File:Line | Comments/Types/Naming/Errors/Tests |

[Very short summary of slop found and changes made.]

[Autonomous-change entries from unslop.]

## Bugs

| #                    | File      | Severity                 | References                                    |
| -------------------- | --------- | ------------------------ | --------------------------------------------- |
| [#](link-to-section) | File:Line | Critical/High/Medium/Low | Supporting code, test, or applicable standard |

Found [B] bugs across [F] files.

[Bug entries ordered from Critical to Low.]

[Brief scope, checklist coverage, checks run, and verification gaps.]
````

Count unique files across both tasks, not the sum of overlapping file counts. Count logical changes actually applied, not edited lines or unapplied recommendations. Count bug-affected files once. Keep both sections when a count is zero. Keep cleanup verification alongside the other verification notes in **Bugs**.
