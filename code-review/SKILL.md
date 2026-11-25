---
name: code-review
description: Review code or a PR and list all substantiated bugs in scope, with severity, evidence, and coverage. Read-only.
---

# Code review

Use this skill for a requested correctness review. Inspect the selected snapshot and report every substantiated in-scope bug. Keep the reviewed files unchanged. When called from a repair workflow, return findings to the owner for disposition. A cleanup or change summary is a separate deliverable.

## Establish the scope

Read repository instructions and the latest accepted requirements. Verify the checkout, branch, and working tree. Honor the requested file, component, or change before choosing a broader scope. Without a specified scope, use the current PR or, without one, attributable current-thread work.

- **Files or components:** inspect the named code and relevant callers.
- **PR:** verify the target, base and head SHAs, commits, and changed files. Review the full provider diff or `git diff <base-sha>...<head-sha>`. Use the actual PR head, which may differ from local HEAD.
- **Last X commits:** verify a positive X and sufficient history; review `git diff HEAD~X HEAD`. X counts first-parent commits. Compare against the empty tree when the selection includes the root commit.
- **Thread:** use messages, accepted decisions, tool history, and current files to identify attributable work across turns and repositories, including committed and uncommitted changes.

Exclude unrelated work and, in PR or commit mode, uncommitted changes unless requested. For a combined cleanup and review, include the cleanup produced for the selected target and identify the resulting working tree as part of the reviewed snapshot. Ask only when missing context prevents establishing scope. State remaining gaps; stop if there is no code or change to review.

## Identify review files and blast radius

Enumerate every scoped file and logical change, including named files with no diff. Trace affected callers, consumers, tests, configuration, and data contracts, including unchanged code. Inspect relevant neighboring code and follow inputs, state, side effects, and failure paths across those boundaries. Apply repository standards over general preferences.

Use the deeper [blast-radius procedure](../blast-radius/SKILL.md) for a requested full audit or a material assumption that crosses a boundary, such as persisted data compatibility or a change in who may call an operation. Follow any affected path beyond the diff while keeping findings within the selected scope.

## Review the code

Inspect every scoped file and logical change. Compare the implementation with accepted requirements and, for a change review, its prior behavior. Check required behavior, regressions, and unnecessary scope additions. Investigate security vulnerabilities first, then other correctness failures.

Use the areas below where they apply. For a requested full audit or a material cross-boundary risk, keep a coverage ledger for the audited scope: issue found, checked, not applicable, or unverified, with the evidence or reason. For ordinary reviews, summarize the scope, checks, and material gaps.

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
4. Check affected consumers before suggesting the smallest correction that would address the defect.
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

List every confirmed bug once, highest severity first. Give each a stable ID, severity, file and line, trigger, expected and actual behavior, and supporting evidence. Include a brief corrective direction where useful. Keep IDs across rechecks and identify the revision or working-tree changes covered.

State what was reviewed, which checks ran, and any material gaps. If no bug is confirmed, say so. For a partial review, identify what remains unassessed. Return the findings directly to the requester.
