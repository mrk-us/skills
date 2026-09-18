---
name: add-component-reference
description: Add or duplicate a self-contained UI or animation example in the components-reference repository from supplied code, a named local component, or an approved reference. Preserve the source behavior, register its gallery entry, and generate its reusable agent prompt. Use for reference-gallery work, not ordinary product components.
---

# Add a component reference

Target `~/Dev/components-reference` unless the user identifies another checkout of that repository. Confirm the repository and read its current instructions, reference registry, generator, and nearest example before editing. Do not create a reference gallery in an unrelated project.

## Capture the requested variant

Read the named source and its supporting styles. Resolve the source from the supplied path or available local context. If the source is inaccessible, explain the missing input rather than inventing an approximation. Fetch a supplied web reference through an available read-only tool; browser or computer control needs explicit approval.

Keep the requested name, defaults, timing, rotations, sizing, colors, copy, and interactions. Treat later corrections as cumulative. If asked to preserve the "original," use the requested earlier revision or source, not an already edited copy. Retain both gallery entries when duplicating a variant.

## Build a portable example

- Put its entry in `apps/app/features/references/<slug>/index.tsx`. Keep supporting CSS, constants, hooks, and helpers within that reference's folder when needed. A short example can remain one file.
- Preserve independence between references. Similar animation values or markup do not justify a new shared abstraction. Reuse established design-system primitives where appropriate, but do not turn reference-specific internals into package APIs.
- Move confusing inline animation settings into named local constants when this improves readability. Keep Tailwind class names visible at their use site.
- Use the project's existing styling and animation tools. Expose overrides that the source or request makes useful, such as size, color, class name, duration, or easing. Keep the requested defaults; avoid a configuration system or demo controls for hypothetical options.
- Separate the reusable component from its gallery wrapper when it needs different layout or props. Preserve accessibility and reduced-motion behavior relevant to the example.

## Register and generate

Add the component, unique slug, title, short description, and imported generated prompt to `apps/app/features/references/registry.tsx`. Follow the current entry shape.

Run `bun run generate:references` from `apps/app`. It writes `agent.generated.ts` from each reference's source and uses a source hash to skip unchanged examples. Do not hand-edit the generated prompt. Inspect the changed prompt for the requested variant and its dependencies. Restore no unrelated changes blindly; investigate unexpected generation diffs first.

Check that every local dependency needed to recreate the example is included or mapped in the prompt. The current generator follows TypeScript sources within the references tree, but a CSS module import alone does not include that stylesheet's contents. If the new example needs missing source coverage, make a focused generator fix so the generated prompt is complete. Keep source hashing and unchanged-file skipping intact.

## Check and hand off

Use the repository's scoped formatting and type checks. Confirm the gallery registration resolves and the generated prompt contains the necessary source. If the generator changed, test source inclusion and unchanged-source skipping with a temporary fixture.

Only start a development server or use browser verification when authorized. If approved, check the actual gallery entry, its motion or interaction, and relevant themes or sizes. Otherwise state that visual behavior remains unverified.

Report the reference name, preserved or exposed options, and checks performed. Do not commit, push, or publish unless requested.
