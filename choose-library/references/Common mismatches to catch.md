# Common mismatches to catch

- Replace handmade toasts or modal-based notifications with Sonner.
- Replace `div`-based dropdowns or dialogs with manual focus handling with Base UI.
- Replace numbers animated through text rerenders with NumberFlow.
- Virtualize lists with more than 1,000 rows using TanStack Virtual.
- Move shared state out of per-component `useState` prop chains into Zustand or Jotai.
- Replace deeply nested `className` ternaries with `cn`, or `cva` when the conditions form component variants.
