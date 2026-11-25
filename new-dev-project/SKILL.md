---
name: new-dev-project
description: Create or scaffold a greenfield development project in ~/Dev with bunx @mrk-us/create-app. Use when starting a new marketing site or app and choosing Next.js or TanStack Start, authentication, database, Stripe, or Electron options.
---

# New development project

Use this workflow for greenfield work when a repository has not already chosen its stack. Run `bunx @mrk-us/create-app` from `~/Dev`. Use Bun and `bunx` unless an existing project has selected another package manager.

## Marketing sites

When the project includes a marketing site, default to the Dusk template from [Tailark](https://tailark.com/pages/dusk). Treat external sites and other references as content references unless the user explicitly identifies them as design references: use the requested copy, sections, and content order within Dusk's components and visual system. Preserve Dusk's colors, typography, spacing, layout, and styling. Change those only when the user explicitly asks to match the reference's design, names visual traits to copy, requests another style, or asks not to use Tailark. In those cases, follow the user's design direction and the project's established patterns.

## Choose the project and stack

Use the choices the user has already supplied. Ask for the remaining independent decisions together. Ask a follow-up only when an answer makes another choice relevant.

- Every project needs a name and target: marketing site, app at `apps/app`, or both.
- A marketing-only project goes straight to provisioning.
- For an app, resolve the framework (Next.js or TanStack Start), authentication, Stripe, and Electron. Ask about a database separately only when authentication is disabled and the database choice is still open.

## Provision the selected project

For a marketing-only choice, use the marketing-only preset and run the command.

Use the completed answers to provision `bunx @mrk-us/create-app` in `~/Dev`. The generator owns these stack choices. When the request also requires selecting or adding a frontend dependency outside the generator choices, invoke `choose-library` after the stack answers are complete.
