---
name: wrap-up
description: >-
  Clean up task worktrees, branches, and local servers, and synchronize the local
  checkout when the user asks to wrap up or sync local.
---

# Wrap up

Clean up the current task's resources and synchronize the original local checkout. A wrap-up request authorizes the cleanup below, including switching off the finished task branch when needed. Reuse destinations and decisions already established in the conversation. A sync-only request stays limited to synchronization.

Commit, push, review, PR creation, merging, and deployment require their own requests. Wrap-up does not add those operations.

## Synchronize local

Verify the repository, remote, task branch, PR state, original checkout, and other worktrees. Fetch relevant remote refs. Use the requested synchronization destination or established PR base; ask only if the destination is materially ambiguous.

Update the actual local branch and checkout with a fast-forward when possible. Preserve local commits and uncommitted work. If they prevent synchronization, leave them intact, report the divergence, and continue independent cleanup. Leave unrelated active checkouts in place.

## Clean up

- Remove the task's clean worktrees and local branches once all their work is verified as preserved on a remote branch or included in a merged PR. Check for local files that need preserving before removing a worktree.
- Delete the task's remote branch only after confirming its work was merged. Retain branches backing open PRs. For squash merges, verify the merged PR and its contents rather than relying on Git ancestry alone.
- Preserve uncommitted work, unique local commits, shared resources, and anything still needed for requested testing or linked evidence. Retain affected resources and explain why when deletion would lose work.
- Stop the current task's local servers unless the user asked to keep a preview running. Remove disposable task-owned temporary files.

## Verify and report

Confirm the resulting checkout, branch, worktree, and server state. Report the local checkout's path, branch, HEAD, and ahead/behind relation to its remote, plus what was removed or retained and why. Describe any remaining gap explicitly instead of calling a divergent checkout fully synced.
