---
name: microcopy
description: >-
  Write, review, and implement microcopy for digital interfaces. Use for UX work and when writing UX copy or designing, building, reviewing, or implementing an app, website, user flow, screen, component, form, settings panel, onboarding, checkout, or other UI that contains or introduces user-facing text, including labels, buttons, hints, errors, confirmations, empty states, loading states, permissions, and notifications. Apply only to the requested or changed surface unless the user asks for a wider copy review.
---

# Microcopy

Treat the interface as a conversation. Give people the information they need at the moment they need it, in language that fits the product and helps them act with confidence.

This skill is an original, practical synthesis of principles from Kinneret Yifrah's *Microcopy: The Complete Guide*. It applies those principles to current product design and implementation work.

## Work from context

Before drafting, inspect the requested flow and nearby interface copy. Establish:

- what the person is trying to achieve;
- what action the product needs from them;
- what they may not know, trust, understand, or remember at this point;
- what changes after the action, including cost, commitment, privacy, timing, and reversibility;
- which product terms, voice rules, localization conventions, and interaction patterns already exist.

Use product research, support language, and users' own words when available. Never invent research, benefits, social proof, urgency, policies, or product behavior. When evidence is thin, preserve established terminology and use a clear, neutral voice.

For voice, audience, tone, and ethical motivation decisions, read [references/voice-and-motivation.md](references/voice-and-motivation.md).

## Shape the interaction

Map the relevant moment before polishing individual strings:

1. **Before:** Set expectations, explain value, answer likely concerns, and prevent foreseeable mistakes.
2. **During:** Use clear labels and nearby guidance so the person can complete the task without remembering hidden instructions.
3. **After:** Confirm what happened, explain what comes next, and provide recovery when the outcome differs from what they expected.

Prefer an interface change when copy is compensating for a confusing control or avoidable constraint. Add words only where they remove real uncertainty. During focused tasks, clarity and completion take priority over personality.

Write for the person, not the system. Use natural spoken language, active voice, concrete verbs, and the fewest words that preserve meaning. A stable product voice can use different tones for success, routine work, delay, risk, and failure.

For component and state-specific guidance, read [references/interface-patterns.md](references/interface-patterns.md) only for the patterns in scope.

## Put copy where the decision happens

- Place essential guidance beside the control, field, or decision it affects.
- Keep rules visible while they are being followed. A disappearing placeholder cannot carry a label, requirement, or format rule.
- State consequences before consequential actions. Name the exact action on its control.
- Explain unfamiliar terms, controls, and requested information where the question arises.
- Use progressive disclosure for secondary detail. Keep information required for success visible.
- Let routine controls use familiar labels. Spend richer copy on uncertain, high-value, or high-risk moments.

For friction analysis, preventive copy, complex systems, and implementation checks, read [references/friction-and-systems.md](references/friction-and-systems.md).

## Preserve agency and trust

Motivate with relevant value and honest reassurance. Invite action instead of pressuring it. Make refusal, cancellation, and reversal understandable. Humor earns a place only when it fits the product, the meaning remains immediate, and the person is not under stress.

Never shame refusal, hide a consequence, disguise an ad, create false scarcity, or make the unwanted choice harder to understand. In errors, security, payments, privacy, data loss, health, and urgent situations, be calm and literal.

## Deliver the work

When drafting, provide final, implementation-ready strings grouped by screen, component, or state. Include placement or trigger notes only when needed to make the copy unambiguous. Offer variants only when they represent a real tone or product decision.

When implementing, keep microcopy changes within the requested UI scope. Preserve the product's source of truth for terminology and translations. Cover the relevant default, loading, empty, success, error, disabled, and permission states rather than treating the happy path as the whole interaction.

When reviewing, report each actionable issue with its location, current copy, proposed replacement, and the user consequence. Consolidate repeated issues. Separate verified behavior from checks that were not run.

## Final check

Before finishing, confirm that every changed string:

- tells the truth about the product and the resulting state;
- answers the question a person is likely to have there;
- uses the same terms as the rest of the flow;
- is understandable without internal or technical context;
- appears early enough to guide the decision or prevent the error;
- remains clear at narrow widths and with dynamic values, plural forms, and translations when applicable;
- has been checked in every changed state that can be inspected with the user's authorized verification methods.
