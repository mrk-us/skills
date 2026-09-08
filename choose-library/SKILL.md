---
name: choose-library
description: Apply preferred libraries while building, changing, optimizing, or reviewing code in a web or app development project. Use for components, loading states, data fetching, forms, state, styling, animation, sounds, AI or chat interfaces, charts, drag and drop, rate limiting, virtualization, and any request to choose, compare, install, or replace a package.
---

# Choose a library

Use these curated defaults in development projects when the user and repository have not already chosen a library.

1. Identify the capability behind the request. For implementation, optimization, or review work that does not name a library, inspect the affected code for relevant dependencies, components, and handmade patterns.
2. Read every matching reference below. Each reference owns its library choices and nearby tradeoffs.
3. Choose one mapped library when the reference gives a clear answer. Preserve an established competing dependency unless the user asks to replace it.
4. When implementation is requested, use the repository's package manager and existing integration patterns. Ask before a library choice that materially changes the architecture. Proceed with established or easily reversible implementation details.

| Reference | Read when |
| --- | --- |
| [Marketing sites](<references/Marketing sites.md>) | Tailark or a marketing-site template |
| [UI components & primitives](<references/UI components & primitives.md>) | Creating or changing UI components, debug panels/controls, shadcn, dialogs, menus, selects, command palettes, toasts, OTP inputs, control panels, tables, hotkeys, or Markdown |
| [Motion, transitions, animations](<references/Motion, transitions, animations.md>) | Loading states, spinners, Motion, transitions, springs, enter or exit effects, shimmer, animated numbers or text, globes, OG images, or syntax highlighting |
| [Sounds](references/Sounds.md) | UI audio or interaction sounds |
| [AI & chat interfaces](<references/AI & chat interfaces.md>) | AI SDKs, chat components, questionnaires, agent interfaces, or agent harnesses |
| [Charts](references/Charts.md) | Static, interactive, real-time, streaming, or dashboard charts |
| [Interaction](references/Interaction.md) | Drag and drop |
| [State & styling](<references/State & styling.md>) | CSS, Tailwind, client or server state, conditional classes, variants, themes, or dark mode |
| [Data and performance](<references/Data and performance.md>) | Fetching or loading data, forms, throttling, debouncing, rate limiting, virtualization, long lists, or large tables |
| [Common mismatches to catch](<references/Common mismatches to catch.md>) | Optimizing or reviewing a component, or replacing a handmade control, animation, list, class-name helper, or shared-state implementation |

Read only the branches the task reaches. When the curated references do not cover the capability, make a normal project-aware choice and state that it falls outside the curated list.
