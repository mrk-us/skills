# Laws of UX reference

Adapted from [Laws of UX](https://lawsofux.com/) by Jon Yablonski. Apply a law only when it explains a real problem or supports a concrete design decision.

## Cognitive load and decision making

### Hick's Law

Decision time increases with the number and complexity of choices.

- Apply when navigation has too many items, forms have too many fields, dashboards are cluttered, or users make frequent choices.
- Consider reducing options, using progressive disclosure, grouping choices, or setting sensible defaults.

### Miller's Law

People can keep only a limited number of items in working memory. The classic formulation uses `7 +/- 2`, but treat that as a historical framing rather than a target.

- Apply when designing navigation, onboarding steps, feature lists, or data tables.
- Consider chunking information, adding perceptual groups, or moving information out of memory and into the interface.

### Choice overload

Too many options can cause decision paralysis and lower satisfaction.

- Apply to pricing pages, product catalogs, filter systems, or settings pages.
- Consider curating recommendations, adding meaningful filters or comparisons, and limiting the options shown initially.

### Cognitive load

An interface consumes mental resources when users must understand it, remember state, or coordinate several tasks.

- Apply when a flow is complex, new users struggle, or errors are frequent.
- Consider simplifying the interface, using familiar patterns, reducing simultaneous tasks, and chunking content.

### Law of Prägnanz

People tend to perceive ambiguous forms in the simplest way available.

- Apply when icons are ambiguous, layouts are unclear, or visual relationships are confusing.
- Consider simpler shapes, familiar symbols, and clearer spatial relationships.

### Selective attention

People notice only part of the available information, usually what appears relevant to their current goal.

- Apply when important information is missed or a primary action is overlooked.
- Consider contrast, position, hierarchy, and restrained motion to direct attention.

## Visual perception and grouping

### Law of proximity

Objects near one another are perceived as related.

- Apply when spacing relationships are unclear or unrelated elements appear grouped.
- Keep related elements closer together and separate distinct groups with more space.

### Law of similarity

Elements with a similar appearance are perceived as a group.

- Apply when related items do not look related or different item types look interchangeable.
- Give the same type of element a consistent treatment and differentiate elements with different roles.

### Law of common region

Elements inside the same boundary are perceived as a group.

- Apply when grouping form fields, card content, navigation sections, or related settings.
- Use a card, panel, fieldset, or another boundary when spacing alone does not communicate the relationship.

### Law of uniform connectedness

Visually connected elements are perceived as related.

- Apply to process steps, related data points, or controls that operate together.
- Use lines, shared containers, or another deliberate connection when the relationship needs to be explicit.

### Serial position effect

People tend to remember the first and last items in a series better than those in the middle.

- Apply to navigation menus, onboarding flows, pricing tiers, or feature lists.
- Put critical items where they are easy to find. Do not rely on order alone when an action must be discoverable.

## Interaction and feedback

### Fitts's Law

The time needed to reach a target depends on its size and distance.

- Apply when buttons are small, targets are far from the cursor or thumb, or click areas are precise.
- Enlarge the interactive area, keep related actions close, and account for touch reach and accessibility requirements.

### Doherty threshold

Fast feedback helps people stay engaged with a task. The common `400ms` reference is a design prompt, not a guarantee.

- Apply when actions feel sluggish, loading feedback is absent, or perceived performance is poor.
- Respond immediately with state feedback, use optimistic UI only when failure can be handled safely, and use progress or skeleton states when work takes longer.

### Goal-gradient effect

Motivation tends to increase as people feel closer to a goal.

- Apply to multi-step flows, onboarding, checkout, or long-running setup.
- Show meaningful progress and make remaining work understandable without disguising its true extent.

### Zeigarnik effect

Incomplete tasks tend to remain mentally active.

- Apply to onboarding completion, profile setup, or task systems.
- Show unfinished work and the next useful action without creating nagging or artificial incompleteness.

## Familiarity and system complexity

### Jakob's Law

People expect an interface to work like other interfaces they already know.

- Apply when considering novel navigation, unconventional interactions, or custom controls.
- Start with established patterns. Depart from them only when the benefit justifies the learning cost and can be tested.

### Mental models

People form expectations from prior experience with similar systems.

- Apply when introducing a novel feature, redesigning a familiar flow, or changing established behavior.
- Align the interface with existing expectations or provide clear orientation when the model must change.

### Tesler's Law

Every system contains some irreducible complexity. That complexity can be moved, but not eliminated.

- Apply when simplifying a genuinely complex task.
- Let the system absorb complexity where it can do so reliably. Expose complexity when hiding it would remove control or create surprises.

### Postel's Law

Accept reasonable variation in user input and produce consistent output.

- Apply to input fields, file upload, parsing, and validation.
- Accept safe, unambiguous input formats, normalize them without destroying intent, and return consistently formatted data.

## Motivation, memory, and perception

### Aesthetic-usability effect

People often perceive an aesthetically coherent design as easier to use.

- Apply when evaluating trust, first impressions, and perceived product quality.
- Use visual quality to support comprehension. Do not let polish conceal unclear interaction or poor accessibility.

### Peak-end rule

People often judge an experience strongly by its most intense moment and its ending.

- Apply to onboarding, checkout success, error recovery, or offboarding.
- Make consequential moments clear and ensure the final state confirms what happened and what comes next.

### Von Restorff effect

An element that differs from its peers is more likely to be noticed and remembered.

- Apply when highlighting a primary action, selected plan, featured item, or warning.
- Make one element distinct for a reason. If everything stands out, nothing does.

### Pareto principle

A small portion of features or flows often accounts for most use or value.

- Apply when prioritizing features, improvements, or an initial product scope.
- Use product evidence to find the high-value paths and optimize those first. Do not assume an `80/20` split without data.

### Flow

Sustained focus depends on an appropriate challenge, clear goals, and timely feedback.

- Apply to creative tools, games, or workflows that require concentration.
- Remove unnecessary interruptions and provide feedback without breaking the user's focus.

### Paradox of the active user

People often begin using a product instead of reading instructions first.

- Apply to onboarding, complex features, or new interaction patterns.
- Make the next action understandable in context and use inline guidance or progressive disclosure where needed.

### Parkinson's Law

Work tends to expand to fill the time available.

- Apply when a task benefits from a meaningful time box or a clear estimate.
- Set honest expectations and helpful constraints. Do not create false urgency.

### Chunking

Information is easier to process when related pieces are grouped into meaningful units.

- Apply to long numbers, addresses, complex forms, dense tables, or long text.
- Break information into perceptual groups while preserving copy, paste, search, and assistive-technology behavior.
