# Friction, prevention, and complex systems

Use this reference when reviewing a flow, preventing errors, writing for expert software, or implementing copy across multiple states.

## Find where words can help

Friction usually appears as delay, hesitation, misunderstanding, concern, error, or abandonment. Prefer evidence from:

- usability sessions and the phrases participants use;
- support questions and repeated complaints;
- analytics showing exits, retries, or recurring validation failures;
- the product rules, state machine, and edge cases in the implementation;
- a walkthrough from the perspective of someone seeing the flow for the first time.

At each decision, ask:

1. What is this?
2. What does it do?
3. Where do I find the requested information?
4. How do I use it?
5. Why do you need this from me?
6. What will happen if I continue?

Replace an unfamiliar term with a plain one when precision survives. Otherwise explain it beside the term. An explanation is successful only when it answers the likely question completely.

## Prevent the error before wording it

Use interaction constraints before instructions where possible. Accept common input formats, disable impossible dates, preserve entered values, show remaining character count, and validate while correction is easy.

When a rule must be written, put it before the error and keep it visible:

- required versus optional fields;
- accepted format and whether punctuation is needed;
- size, range, length, and file-type limits;
- password requirements and live completion status;
- irreversible effects and dependencies;
- real-world consequences after the person leaves the interface.

Explain the smallest set of constraints the person needs. A wall of preventive copy creates its own friction.

## Decide whether copy or UX owns the problem

Copy can name a concept, explain a necessary rule, set expectations, and show a recovery path. It cannot repair a misleading information hierarchy, an ambiguous control, a hidden option, or a process with needless steps.

If the explanation becomes long, check whether the interface can reveal information progressively, replace text with a clearer control, remove the constraint, or split the task. Keep information that determines success visible. Secondary education can live in a tooltip, disclosure, or direct link to the exact help topic.

## Write for complex and expert systems

Professional users need the terminology of their field. Keep accurate domain terms when those terms help them find features and trust the tool. Surround them with plain syntax, direct verbs, and concise explanations.

Optimize repeated workflows for recognition and speed. Novel jokes, rotating labels, and elaborate confirmations become friction on the thirtieth use. Use richer guidance for first use, rare configuration, risky changes, and unfamiliar features.

In complex systems:

- make menu items, field labels, table headings, graph titles, and statuses unambiguous;
- explain relationships and consequences that cannot be inferred from one control;
- write errors from the professional task's point of view, not the software architecture;
- use empty states to teach what will appear, when, and how to create it;
- link directly to the relevant help section when inline guidance cannot be short;
- modernize copy in changed areas without renaming established concepts casually.

Expertise does not justify dense or old-fashioned language. Write as a capable practitioner speaks at work, not as a system log or policy document.

## Cover the state model

When implementing or reviewing a component, inspect every state that changes what the person understands or can do:

| State | Copy must establish |
| --- | --- |
| Default | Purpose, available action, required context |
| Focus or expanded | Just-in-time guidance and constraints |
| Loading or pending | Current activity, safe next behavior |
| Empty | Why it is empty and how to proceed |
| Success | Result, next step, timing, reversibility |
| Recoverable error | Problem and exact correction |
| Unrecoverable error | Limitation, alternative, support route |
| Disabled | Why unavailable and what enables it, when useful |
| Permission request | Benefit, scope, data use, refusal effect |
| Destructive confirmation | Object, consequence, exact action, escape |

Include only states the component can actually enter. Do not invent product behavior to complete the table.

## Verify implemented copy

Use the verification methods authorized for the task. Check what can be observed and state what remains unverified.

- Trace the changed state transitions against the implementation.
- Confirm that copy matches actual validation, permissions, pricing, timing, and recovery behavior.
- Check variables, missing values, plural rules, date and number formats, and long localized strings.
- Confirm that labels and errors remain programmatically associated with their controls when accessibility is in scope.
- Check wrapping, truncation, and hierarchy at the narrowest supported layout when visual verification is authorized.
- Confirm that transient messages persist long enough for their purpose, or move essential information to a durable surface.

Do not claim a flow works from static inspection alone. Distinguish copy reviewed in source from behavior exercised in the running product.
