# State & styling

| Task | Library |
| --- | --- |
| CSS styling | [Tailwind CSS](https://tailwindcss.com/docs) |
| State management | [TanStack Query](https://tanstack.com/query/latest), [Zustand](https://zustand.docs.pmnd.rs), or [Jotai](https://jotai.org/docs) |
| Conditional `className` construction | [cn](https://github.com/shadcn-ui/cn) |
| Type-safe Tailwind variants | [cva](https://cva.style) |
| Theme switching and flash-free dark mode | [next-themes](https://github.com/pacocoursey/next-themes) |

Use TanStack Query to handle state directly wherever possible. Use Zustand or Jotai when it does not fit.

Migrate projects from `clsx` and `tailwind-merge` to `cn` with `bunx --bun shadcn@latest migrate cn`.

Use `cn` for ad hoc conditional classes. Use `cva` when a component has size, intent, state, or other variants that deserve a typed API. They compose because `cva` accepts `cn`-style inputs.
