---
name: park-that
description: >-
  Pause one specific line of work without losing its context while an implementation continues. Use when the user says "park that", "park this", "leave that for later", or otherwise asks to defer part of the current task while preserving it for a follow-up.
---

# Park part of the work

Treat the conversation as one evolving task unless the user clearly replaces or abandons it. Follow-up instructions normally modify that task rather than starting over.

## Maintain task state

Keep track of:

- the current objective;
- active lines of work;
- parked lines of work;
- decisions already made;
- constraints and accepted behavior that still apply.

Apply each follow-up as a change to that state. A later instruction overrides an earlier one only where they conflict. Carry every unrelated decision and constraint forward.

## When the user parks something

1. Resolve "that" to the specific line of work the user is referring to. If two materially different interpretations remain, ask a short clarifying question before changing scope.
2. Stop work on that line after the minimum needed to leave the current state coherent. Do not investigate, implement, polish, or validate it further. Do not discard partial work or make a commit unless the user asked for that.
3. Keep working on the other active parts of the task. If the parked line was the only remaining work, stop the implementation.
4. Add or update an explicit todo or working note, especially during a long-running task. Put it in the current task list, plan, or working notes. Use this form:

   ```text
   - [ ] PARKED: <specific task>. Continue later from <current state>. Preserve <relevant decisions or constraints>.
   ```

   The reminder must name the deferred task. Do not write only "parked item" or rely on a pronoun.
5. Keep the reminder visible in later progress summaries and the final handoff while it remains parked. For work that may cross context or session boundaries, use an existing durable project note or tracker when one is already part of the task. Do not create repository files, issues, or external tasks solely for this reminder without authorization.

Parking is not cancellation. Do not silently delete the item, treat it as completed, or resume it because adjacent work makes it convenient.

## Follow-ups to parked work

- If a follow-up changes the parked task's requirements, update its reminder and keep it parked.
- Resume it only when the user asks to resume, continue, or implement that parked work.
- When resuming, restore its recorded decisions and constraints before acting.
- If the user cancels it, remove it from the active reminder and record that it was cancelled rather than completed when task history matters.

When reporting status, distinguish `active`, `parked`, `completed`, and `cancelled`. Never present parked work as a blocker or as finished.
