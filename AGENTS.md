# Writing skills

Use these conventions when creating or editing skills in this repository.

## Scope and discovery

Name each skill directory and its YAML `name` in lowercase kebab-case. The frontmatter `description` should state what the skill does and when it applies. Put detailed procedures in the body.

Keep names, prompts, links, and invocation policy consistent with `agents/openai.yaml` when present. Preserve existing triggers, authorization boundaries, and behavior unless the requested change includes them. Inspect the current content and diff before revising a skill.

## Organize the content

Use this general reading order:

1. Purpose and relevant context. Open with the outcome the skill delivers and the context needed to use it.
2. Instructions grouped by the decisions involved. Keep each rule, its conditions, and its exceptions together. Use ordered steps when execution order matters.
3. Examples beside the rules they illustrate. Use BAD/GOOD pairs where the distinction helps, or a single example or table when that is clearer.
4. Verification and expected handoff. Explain how to assess completion and what to return, using checks proportionate to the task.

Treat this as a useful order, not a fixed template. Keep content that already reads well. Omit empty sections and examples that add no guidance.

Keep short, essential workflows inline. Add a reference file only when substantial detail belongs to a distinct mode or decision, and link it where the agent needs it with a clear condition for reading it. Keep each instruction in one authoritative place.

## Markdown and examples

- Use one `#` title, `##` headings for actual sections, and `###` headings for real subsections. Use sentence case.
- Use **bold** for highlights, mode labels, and **BAD** / **GOOD** example labels within a section. Use headings for section content.
- Separate headings, paragraphs, lists, tables, blockquotes, and code fences with blank lines.
- Use inline code for identifiers, paths, commands, and literal values. Label fenced code blocks with their language; use `text` for directory trees and plain-text output.
- Use blockquotes for prose examples and code fences for code or layout examples. Keep paired examples about the same task and preserve its facts and constraints.
- Use lists for parallel choices or ordered steps, and tables for mappings and comparisons. Nest only when the content has a real hierarchy.
- Use relative links to files in this repository. Link a reference at the decision that requires it.

Examples illustrate the adjacent rule. Label illustrative layouts and defaults so they do not become universal requirements or override an existing project's conventions. Preserve user-supplied examples unless asked to edit them.

## Tone and instruction quality

Write in plain English with concrete verbs and direct instructions. Name the action and the criterion that guides it. Keep paragraphs focused and explain technical terms only when they affect a decision.

For an instruction about commit verification:

**BAD**

> Be thorough and follow best practices before committing.

**GOOD**

> Review the staged diff and confirm every change belongs to the requested commit.

Use straight quotes. Avoid em dashes, decorative emojis, generic praise, filler, conversational history, and repeated rules. Use colons to introduce lists or examples. Include constraints and exceptions that affect the work; avoid generic checklists and speculative edge cases.
