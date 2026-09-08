---
name: file-pr
description: File a concise pull request. Use when the user asks to file, open, or create a PR.
---

# File PR

Before filing, confirm the repository, head branch, and intended base branch. Check for an open PR with that head and base; update it if one exists. Fetch the base branch from its remote, then review the full PR diff locally against that base. Confirm that every changed file belongs to the requested scope and that the diff delivers the intended change.

Default to [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/). Include a scope when it helps identify the affected area, such as `feat(sidebar):`. Use `!` before the colon for a breaking change.

Choose the type that matches the PR's main purpose. Common types are:

- `fix`: bug fixes.
- `feat`: new features or capabilities.
- `perf`: performance improvements.
- `refactor`: code restructuring that preserves behavior.
- `docs`: documentation changes.
- `test`: test additions or corrections.
- `build`: build tooling or dependency changes.
- `ci`: continuous integration configuration or scripts.
- `style`: code formatting, such as whitespace or semicolons.
- `chore`: maintenance that fits none of the more specific types.
- `revert`: undoing an earlier change.

Write a concise, human-readable title that states the problem solved or the useful outcome. The title may become the commit message, so it should make sense without the PR description:

**BAD**

> feat: add sidebar slice logic and expanded state

**GOOD**

> feat: keep sidebar sections compact with a five-item preview

Open the description with a simple summary, including the initial problem and what this PR added to solve it. Do not mention the prompt, user, assistant, chat, conversation, previous request, or instructions. Use ASD-STE100 Simplified Technical English:

**BAD**

> Added a five-item slice to each sidebar section, tracked expanded state, and wired up a "View more" click handler.

**GOOD**

> Long sidebar sections pushed other sections out of view. Each section now shows up to five items, with a "View more" option to reveal the rest.

Open a real PR, not a draft. Drafts do not get review-bot coverage.
