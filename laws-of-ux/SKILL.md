---
name: laws-of-ux
description: >-
  Apply behavioral psychology and Laws of UX when designing, implementing, or reviewing user interfaces and user flows. Use for UI/UX design and frontend implementation involving navigation, information architecture, forms, onboarding, checkout, dashboards, settings, pricing, choice complexity, grouping, hierarchy, interaction targets, feedback, loading states, progress, error recovery, or perceived usability. Also trigger on Hick's Law, Fitts's Law, Miller's Law, Jakob's Law, Gestalt principles, cognitive load, decision fatigue, and Laws of UX. Do not use for backend-only work or visual assets with no interface behavior.
---

# Apply the Laws of UX

Use the laws as diagnostic lenses for interface decisions. They explain likely user behavior, but they are not a checklist and do not override user research, accessibility requirements, product constraints, or direct evidence.

## Work within the task

1. Identify the user's goal, the decisions they must make, and the controls or content involved in the requested work.
2. Read the relevant sections of [the laws reference](references/laws.md). Use only the laws that explain a concrete issue or support a concrete design choice.
3. Prefer the smallest change that reduces effort, uncertainty, delay, or error while preserving required behavior and the project's existing patterns.
4. Verify the affected path at the level authorized by the task. Do not expand a focused implementation into a broad UX audit.

For ordinary implementation, one to three strong principles are usually enough. Name a law in the user-facing explanation only when the name improves the decision. Otherwise, explain the user effect in plain language.

## Resolve competing principles

Laws can point in different directions. More visible choices may improve discoverability while increasing cognitive load. Familiar patterns may reduce learning time while limiting a specialized workflow. Resolve those tensions in this order:

1. Preserve accessibility, safety, and task completion.
2. Follow observed user behavior and product evidence.
3. Preserve the project's established component library, design system, terminology, and interaction patterns.
4. Reduce cognitive effort and unnecessary decisions.
5. Improve salience, momentum, and memorability without manipulation.

Do not use psychological principles to hide material information, manufacture urgency, obstruct cancellation, or steer users into an option against their interests.

## Treat thresholds as prompts

Values such as `7 +/- 2` items or a `400ms` response are memorable heuristics, not universal acceptance criteria. Consider task complexity, user expertise, device, input method, frequency of use, and measured behavior before turning a reference value into a requirement.

## Coordinate with other design guidance

This skill owns the behavioral rationale for an interface decision. Let narrower skills own their implementation details, including accessibility, layout, typography, color, copy, animation, and component mechanics. When another skill supplies a stricter requirement, follow it and use the relevant UX law only to explain the user impact.
