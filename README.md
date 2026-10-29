# Skills

Reusable agent skills for development, code review, writing, and interface design.

## Installation

```bash
bunx skills add https://github.com/mrk-us/skills
```

## Available skills

| Skill | Description |
| --- | --- |
| [add-component-reference](add-component-reference/SKILL.md) | Add self-contained UI or animation examples to the components-reference gallery, including registration and reusable agent prompts. |
| [blast-radius](blast-radius/SKILL.md) | Trace what a change could break beyond the diff and verify the key safety assumption by running real code. |
| [choose-library](choose-library/SKILL.md) | Choose preferred libraries for web and app development, including UI, state, data fetching, animation, and AI. |
| [clean-copy](clean-copy/SKILL.md) | Cut redundant labels, unnecessary hints, and fixed-bug warnings when writing or reviewing interface copy. |
| [code-review](code-review/SKILL.md) | Review code for actionable bugs, regressions, security issues, type-safety defects, and error-handling failures without editing by default. |
| [commit](commit/SKILL.md) | Review, group, and commit local Git changes with clear messages and relevant checks. |
| [component-canvas](component-canvas/SKILL.md) | Build a persistent, dev-only canvas page for exploring existing UI components and variants with pan, zoom, and per-component isolation. |
| [debug-motion](debug-motion/SKILL.md) | Diagnose and fix animations that replay, reset, jump, flash, or stutter while preserving intended motion and layout. |
| [file-pr](file-pr/SKILL.md) | Create or update concise pull requests that explain the problem, resulting behavior, and validation. |
| [file-structure](file-structure/SKILL.md) | Organize code around features and actual ownership, keeping local logic nearby and sharing only where justified. |
| [implement](implement/SKILL.md) | Complete features, bug fixes, refactors, and maintenance changes through relevant verification. |
| [laws-of-ux](laws-of-ux/SKILL.md) | Apply behavioral psychology and Laws of UX to interface design, user flows, implementation, and reviews. |
| [microcopy](microcopy/SKILL.md) | Write, review, and implement interface text such as labels, buttons, hints, errors, and empty states. |
| [new-dev-project](new-dev-project/SKILL.md) | Scaffold new apps and marketing sites in `~/Dev` with `bunx @mrk-us/create-app` and guide stack choices. |
| [park-that](park-that/SKILL.md) | Defer a specific line of work with a clear continuation reminder while preserving the active task. |
| [sol-loop](sol-loop/SKILL.md) | Review code changes in a bounded GPT Sol loop when the user says "sol loop". |
| [unslop](unslop/SKILL.md) | Clean up comments, types, naming, error handling, and tests while preserving required behavior and public contracts. |
| [wrap-up](wrap-up/SKILL.md) | Finish authorized Git work by pushing branches, managing pull requests, merging, and synchronizing local checkouts. |
| [write-better](write-better/SKILL.md) | Draft or revise prose in a grounded narrative nonfiction style when explicitly invoked. |
