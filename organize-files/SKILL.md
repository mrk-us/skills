---
name: organize-files
description: Reorganize explicitly requested code by feature and actual ownership. Simplify within existing boundaries, keep single-use logic local, and extract only for a real shared responsibility or a meaningful boundary. Preserve self-contained examples and avoid folder churn for hypothetical reuse.
disable-model-invocation: true
---

# Organize files

Start with the named area and the problem the organization should solve. Read current instructions, files, imports, and the working diff. Preserve unrelated work. A smaller file or a more elaborate folder tree is not an improvement by itself.

## Simplify locally first

Improve names, remove unnecessary indirection, clarify control flow, and name meaningful local constants before moving code. Keep helpers beside their only consumer when that is easiest to understand. A separate local file is useful when it gives a substantial concern a clear boundary; one consumer does not require inlining everything.

Do not restructure a component reference, example, or experiment merely because it resembles another. When copying independently is the purpose, keep each example self-contained. Shared implementation can undermine that purpose.

## Choose the owner

| Actual responsibility | Default location |
|---|---|
| One component or standalone example | Its existing file or nearby folder |
| Multiple consumers within one feature sharing the same behavior | That feature's existing components, hooks, or utility area |
| Multiple features relying on one domain rule or contract | The domain owner or an existing shared boundary |
| A deliberately reusable design-system primitive | The established UI package or primitive directory |

Use established repository names. A feature may own components, hooks, utilities, and types, but create only the folders needed. Keep legitimate domain dependencies explicit; do not move domain code to a generic `shared` directory just because several features import it.

Before extracting, identify the current consumers, the responsibility they share, and whether they should change together. A second call site is evidence of reuse, not an automatic instruction to share. Similar syntax, generic naming, and "might be useful later" are insufficient. A deliberate API, policy, or dependency boundary can justify a single-consumer abstraction when it solves a present problem.

Do not extract Tailwind class strings into shared constants merely to deduplicate them. Keep styling readable at its use site. Avoid new barrel files, forwarding wrappers, generic managers, and empty directory scaffolds unless an existing contract requires them.

## Make the change

1. State the concrete ownership or readability improvement. If the current structure already serves it, leave it in place and explain why.
2. Move only the files needed and update their actual consumers. Check imports, exports, routes, dynamic imports, configuration paths, generated-source inputs, and tests where applicable.
3. Preserve behavior, public interfaces, supplied values, and manual edits. Keep a file move recognizable in the diff; avoid unrelated formatting or refactoring.
4. Remove only directories made empty by this change. Check for missed references and use the relevant type, build, or behavior check. Do not add tests that only assert file locations.

Finish with what became easier to understand, the affected boundaries, and validation. Mention any consumers that could not be checked. Do not reorganize the rest of the repository to make the pattern uniform.
