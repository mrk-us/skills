---
name: code-review
description: Review the current PR, the last X commits, or a specified thread for slop, correctness, and unnecessary complexity. Use when asked to "review" code, PR's, commits, or general web development work.
---

# Code review

Ask of every change:

- Does this solve the actual problem and preserve required behavior?
- Does this leave the codebase better off than when you found it?
- Is this the simplest solution within the existing design?
- Can this be achieved in less code?
- Does each new abstraction, dependency, fallback, and test earn its place?

Judge simplicity by ease of understanding and maintenance. Recommend fewer lines only when they preserve intent and useful safeguards. Report concrete improvements; an adequate implementation needs no cleanup finding. Apply fixes only when requested.

## Process

### 1. Set the scope

Verify the checkout, branch, and working tree. Use the requested mode, defaulting to the current PR or, without one, attributable current-thread work. Derive comparison refs yourself.

- **PR:** verify the target, base and head SHAs, commits, and changed files. Review the full provider diff or `git diff <base-sha>...<head-sha>`. Use the PR head, which may differ from local `HEAD`.
- **Last X commits:** verify a positive X and sufficient history; review `git diff HEAD~X HEAD`. With merges, state that X counts first-parent commits. Compare against the empty tree if the selection includes the root commit.
- **Thread:** use messages, accepted decisions, tool history, and current files to identify all attributable work, including earlier turns, involved repositories, and committed or uncommitted changes. Separate pre-existing and unrelated edits using starting-state and hunk evidence.

Exclude uncommitted work from PR and commit modes unless requested. Record the file inventory and refs or thread evidence. Ask for missing context if scope cannot be established; report any remaining gaps. Stop if the scope is empty.

### 2. Gather requirements and standards

Read repository instructions, coding standards, and relevant neighboring code. Establish requirements from the latest accepted decisions, supplied specs, PR description, and linked issues. Resolve material conflicts; mark Spec unassessed if requirements are unavailable.

Invoke [organize-files](../organize-files/SKILL.md) as review guidance for file ownership and placement. Apply documented repository rules over general preferences. Let tooling handle mechanical formatting and lint checks.

### 3. Review and verify

Run two passes, using parallel sub-agents when available. Give both the same scope and source evidence:

- **Standards:** work through every subsection and individual criterion under Review checks, including its exceptions and preservation guidance. Apply repository rules and organize-files guidance.
- **Spec:** check for missing behavior, incorrect implementation, regressions, and unnecessary scope additions. Allow supporting work needed to meet requirements.

Keep coverage notes keyed to the Review checks criteria. For each criterion, record `checked` with inspected files or evidence and any finding, `not applicable` with a scope-based reason, or `unassessed` with the blocker. A category-level tick alone does not establish coverage of its criteria.

For each logical change, answer all five opening questions in the review notes; group related hunks across files when they implement one change. Compare the implementation with a concrete simpler alternative where one exists, and record why to simplify or retain it. Assess each introduced abstraction, dependency, fallback, and test by the current requirement or useful safeguard it serves. Route behavior and scope findings to Spec, and simplicity and maintainability findings to Standards.

Account for every scoped file. Trace inputs, state changes, side effects, and failure paths through relevant callers, tests, and configuration. Verify candidate findings against current code and concrete consequences; discard unsupported, resolved, or unrelated pre-existing issues.

Run focused non-destructive checks where useful. Browser or computer control requires explicit authorization. Recheck files or refs that change during review. Record coverage gaps and unverified behavior.

Before reporting, reconcile the file inventory, criterion coverage, and opening-question assessments. If delegating, collect this evidence from both passes and resolve omissions yourself. Complete the review only when every scoped file and logical change is accounted for and every criterion is checked or justified as not applicable. If blockers leave anything unassessed, report the review as partial and identify the gaps.

### 4. Report findings

State the scope, then present separate `Standards` and `Spec` sections ordered by impact. Each finding needs a file and line reference, evidence, consequence, and the smallest useful correction. Cite the rule or requirement and distinguish violations from judgment calls. Cross-reference overlapping findings; report tooling failures once.

Say when a pass has no findings or could not be assessed. Include a compact coverage summary for every Review checks subsection, the outcome of the opening-question assessments, checks run, and verification gaps. Keep detailed coverage notes out of the findings list; a completed check does not require a finding. A passing build does not verify appearance or interaction.

## Review checks

### Structure and simplicity

Look for unnecessary wrappers, forwarding functions, intermediate objects, repeated transformations, duplicated branches, speculative compatibility code, and extensibility without a current requirement. Prefer deleting unnecessary work or simplifying locally before introducing another abstraction.

Keep these code-smell heuristics from the existing review baseline, but require evidence of a real cost:

- **Mysterious name:** the name hides the responsibility or domain meaning.
- **Duplicated code:** repeated logic represents the same responsibility and should change together. Similar syntax alone does not justify sharing.
- **Feature envy:** behavior depends on another owner's data and would be clearer beside that owner.
- **Data clumps:** fields repeatedly travel together because they represent one concept.
- **Primitive obsession:** broad primitive values obscure a meaningful domain contract.
- **Repeated switches:** the same decision is maintained in several places and could have one clear owner.
- **Shotgun surgery:** one responsibility requires scattered edits because ownership is fragmented.
- **Divergent change:** one module mixes unrelated responsibilities.
- **Speculative generality:** parameters, hooks, or abstractions serve hypothetical requirements.
- **Message chains:** callers navigate internal details they should not need to know.
- **Middle man:** a layer delegates without adding a useful policy, boundary, or meaning.
- **Refused bequest:** inheritance forces implementations to ignore or undo the inherited contract.

Suggest extraction, sharing, or composition only when it makes the actual flow simpler. Keep abstractions that encode a real boundary, policy, dependency seam, or domain concept.

### Type slop

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

### Comment hygiene

Keep comments that help future developers understand information not obvious from the code.

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

- mention the prompt, user, assistant, chat, conversation, previous request, or instructions;
- say things such as "as requested", "as discussed", "you mentioned", "we decided", or "per your instructions";
- explain what the generating AI chose rather than why the code itself exists;
- narrate obvious syntax or control flow;
- restate the function or variable name in prose;
- contain tutorial-style explanations inappropriate for the surrounding codebase;
- contain speculative TODOs with no concrete requirement;
- describe changes relative to an earlier version instead of documenting the resulting implementation;
- tell a future AI what not to modify.

When an AI-shaped comment contains useful information, rewrite it rather than deleting it.

Example:

```ts
// We need this because, as you mentioned, Stripe can send the same event more than once.
```

becomes:

```ts
// Stripe may deliver the same event more than once; processing must remain idempotent.
```

### Testing slop

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

Do not rewrite a stable, established testing architecture merely to satisfy a preference. Focus on slop introduced in the selected scope.

### Error-handling slop

Look for:

- swallowed errors;
- catch-and-rethrow without added context;
- generic "Something went wrong" errors replacing useful underlying information;
- impossible fallback values that let invalid state continue;
- redundant `try` blocks;
- defensive branches added without evidence;
- logging plus rethrowing that causes duplicate logs.

Preserve intentional boundary translation, cleanup/finally behavior, retry policy, and security-sensitive redaction.

### Naming and prose slop

Look for generic generated names such as:

- `data`, `item`, `result`, `handler`, `manager`, `processor`, `helper`, `utils`, `shape`, or `config` when a domain-specific name is available;
- long names that encode implementation steps rather than domain meaning;
- verbose error messages, docstrings, or UI copy created from chat-like prose.

Rename only when the surrounding code provides clear evidence of the intended domain term.
