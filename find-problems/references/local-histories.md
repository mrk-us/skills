# Reading local sessions and conversation exports

## Select transcripts

Apply the source and symlink rules in the skill before enumeration and again when reopening evidence. Inspect directory entries without following links. Within a supplied directory or ZIP, select only conversation records. Read archive members in place and skip links, attachments and account metadata. Treat an unrecognized entry as a coverage gap when it could represent chat history.

| Provider | Session layout | Evidence to extract |
| --- | --- | --- |
| Claude Code | Project-level session JSONL and associated subagent session JSONL | Read message IDs, parent IDs, role and text blocks. A tool result inside a user-role envelope is machine output. Attribute delegated turns to their originating task. |
| Codex | Rollout JSONL in dated session folders or archived sessions | Use session metadata and role-bearing response messages. Where the message stream is absent, use user and agent events. Reconcile both representations by turn and sequence before counting. |
| Claude | Conversation exports, often JSON with `chat_messages` | Follow message order and parent links when present. Map `human` and `assistant` senders to roles and read visible text blocks. Reconcile duplicate plain-text and structured versions. |
| ChatGPT | Conversation JSON (including numbered parts), or rendered chat HTML | For a `mapping` tree, follow parent links from `current_node` to reconstruct the active branch. Review other saved branches separately and count shared ancestors once. Read the message author's role and visible content. |
| Antigravity CLI (`agy`) | One saved session per `.db` or legacy `.pb` in `~/.gemini/antigravity-cli/conversations/` | Decode visible turns in step order with a compatible read-only reader. For example, [agy-reader](https://github.com/mjacobs/agy-reader) supports `agy-reader export-offline --format=json SESSION.db` to stdout. SQLite journal files are part of their database. Reconcile any exported transcript with the same session ID. If decoding is unavailable or incomplete, report the gap and accept a transcript export. |
| Gemini web | Supplied Gemini conversation or Google Takeout activity exports | Read labeled user and model turns. Some activity entries record only a prompt. Cite that prompt on its own and mark the reply or outcome unknown. |
| Other agents | A supplied session store, JSON/JSONL, HTML, Markdown, or plain-text transcript | Identify thread boundaries, speaker labels, ordering and visible dialogue from the actual records. Preserve missing roles or dates as unknown. Skip and report structures that cannot be interpreted reliably. |

Decode a few turns to verify the record shape, then read every eligible conversation through the cutoff captured at the start. For JSONL, stream records by line. If a line is malformed, record the gap and continue with later lines. Read the conversation text and relevant recorded outcomes. Skip embedded file payloads and media.

## Track coverage in the conversation

Keep one compact entry per store with its cutoff, known date range, candidate conversation count, fully reviewed count, partial or unread count, confirmed mirrors and reasons for gaps. Use "unknown" when a denominator is unavailable. A missing store does not establish zero conversations.

At batch boundaries retain the source, session ID, last reviewed record or turn and redacted findings, including a completion marker for sessions with zero findings. If the pass ends early, give the next transcript and record so the user can resume from the report.

Identify the store or export by name, then cite the relative file or archive member and a line, message ID, or turn index. Keep the absolute home directory out of report text and link targets. For pasted text, cite the supplied conversation and turn. Use IDs alone when paths contain sensitive text. Verify the locator against the eligible transcript itself.
