---
name: wrap-up
description: >-
  Finish Git work by pushing branches, creating or updating PRs, merging when
  authorized, and synchronizing local checkouts. Use for requested Git follow-through.
---

# Wrap up

Carry the current task through the requested Git endpoint. Reuse scope, destinations, and authorization established in the conversation. Complete necessary intermediate steps without asking again.

## Choose the requested endpoint

A specific request controls which operations to perform. A push-only or sync-only request stays that size. When explicitly invoked as `$wrap-up` without a narrower endpoint, commit the current task's changes, push its branch, and open or update a non-draft PR. Merging, switching branches, deployment, and cleanup remain conditional on the requested scope.

Keep reset, stash, force-push, and branch/worktree deletion outside the workflow unless specifically authorized.

For a request to push the current task branch:

**BAD**

> Push the branch, merge its PR, and delete the worktree.

**GOOD**

> Push the intended branch and confirm the remote commit.

## Establish the target

Inspect the repository, remotes, current branch and HEAD, working-tree changes, and relevant worktrees. Identify the task's changes and intended PR base or synchronization source from the request and repository guidance. Preserve unrelated work and use the existing task branch or worktree where appropriate.

Refresh relevant remote refs and inspect ancestry and ahead/behind state. Distinguish updating a local clone from promoting changes between remote branches. Resolve a materially ambiguous destination before changing it; do not assume every repository uses `main` or deploys from its default branch.

## Complete the requested operations

- **Commit:** use [commit](../commit/SKILL.md) for scoped staging, grouping, messages, checks, and hooks. Create a task branch first when the current branch is not an allowed commit destination.
- **Push:** publish the intended branch to its verified remote and confirm the remote SHA. If the remote has advanced, fetch and inspect the divergence before reconciling it under the repository's policy and current authorization.
- **PR:** use [file-pr](../file-pr/SKILL.md), including contribution guidance and required evidence. Collect evidence with authorized tools; if capture is blocked, continue independent preparation and identify the missing evidence or permission. Update an existing PR for the intended head and base instead of creating a duplicate. Leave it open when filing is the requested endpoint, reporting pending checks without implying merge readiness.
- **Review and merge:** use [code-review](../code-review/SKILL.md) when a review is requested or required. Merge only when authorized and the required checks and reviews pass for the current PR head. Use a head-SHA precondition where supported so another push cannot substitute unverified work. Follow the repository's merge policy and verify that the PR actually merged.
- **Local synchronization:** update the requested clone and local branch from the intended remote branch, checking other worktrees before moving branch pointers. Prefer a fast-forward. Preserve local commits and uncommitted work; investigate divergence and resolve it within the authorized scope. Switch the active checkout only when requested. Updating remote-tracking refs alone does not update a local checkout.

## Verify and hand off

Reuse valid verification from the implementation or commit stage. Repeat checks when the content, integration result, or relevant environment changes, or when repository policy requires them.

Report the requested endpoint with its evidence: pushed branch and commit, PR URL and state, and each synchronized clone's path, local branch, HEAD, and relation to its remote. Identify remaining changes, divergence, or pending checks. Claim deployment success only after verifying the deployment when it is part of the request.
