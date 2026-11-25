---
name: trace-feature
description: Trace one feature in a repository to its implementation, dependencies, and reusable pieces.
---

# Trace feature

1. Pin the repository and revision. Start from the feature as the user encounters it: a screen, route, command, or documented example. Search its label, entry point, or registration and follow the active path to the code that implements the behavior.
2. Identify what the project implements, what a dependency supplies, and where control passes between them. Verify dependency claims at the import and call site; use the manifest or lockfile for version evidence. Distinguish an implementation in the repository from a demo that links, embeds, or calls a hosted service.
3. Lead with the direct answer. Cite the shortest source chain that establishes it, using revision-pinned file and line links when available. Name the reusable API, component, or package and the project-specific wiring it relies on. If the path ends at an external boundary, identify that boundary. If the implementation could not be located, report the revision and paths checked, and leave its availability unresolved.
