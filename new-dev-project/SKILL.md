---
name: new-dev-project
description: Create or scaffold a greenfield development project in ~/Dev with bunx @mrk-us/create-app. Use when starting a new marketing site or app and choosing Next.js or TanStack Start, authentication, database, Stripe, or Electron options.
---

# New development project

Use this workflow for greenfield work when a repository has not already chosen its stack. Run `bunx @mrk-us/create-app` from `~/Dev`. Use Bun and `bunx` unless an existing project has selected another package manager.

## Marketing sites

When the project includes a marketing site, default to the Dusk template from [Tailark](https://tailark.com/pages/dusk). Treat external sites and other references as content references unless the user explicitly identifies them as design references: use the requested copy, sections, and content order within Dusk's components and visual system. Preserve Dusk's colors, typography, spacing, layout, and styling. Change those only when the user explicitly asks to match the reference's design, names visual traits to copy, requests another style, or asks not to use Tailark. In those cases, follow the user's design direction and the project's established patterns.

## Choose the project and stack

Ask the questions in stages. Finish each stage before showing the next one.

### Stage 1. Project and targets

Ask for:

1. Project name.
2. One or more app targets:
   - Marketing site
   - App at `apps/app`
   - Both

For a marketing-only choice, continue to [Provision the selected project](#provision-the-selected-project). When the choice includes the app, continue to Stage 2.

### Stage 2. Framework and authentication

Ask for:

1. App framework:
   - Next.js
   - TanStack Start
2. Authentication:
   - No
   - Yes

When authentication is enabled, continue to Stage 4. Otherwise continue to Stage 3.

### Stage 3. Database

Ask whether the project needs a database, then continue to Stage 4.

### Stage 4. Payments

Ask whether the project needs Stripe, then continue to Stage 5.

### Stage 5. Desktop app

Ask whether the project needs Electron.

## Provision the selected project

For a marketing-only choice, use the marketing-only preset and run the command.

Use the completed answers to provision `bunx @mrk-us/create-app` in `~/Dev`. The generator owns these stack choices. When the request also requires selecting or adding a frontend dependency outside the generator choices, invoke `choose-library` after the stack answers are complete.
