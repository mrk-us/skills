---
name: debug-motion
description: Diagnose animations that replay, reset, jump, flash, stutter, or lose continuity after rerenders, polling, navigation, or tab changes. Use for an existing motion defect, including vague reports such as "janky" or "reloads." Trace component identity, state, layout, and animation ownership before changing the effect.
---

# Debug motion

Find the event that breaks continuity and fix its owner. Preserve the user's current timing, easing, distances, geometry, copy, and interactions unless changing one is the requested fix. Read the active source and diff first; an earlier version is not authority over manual edits.

## Establish the failure

Identify the affected element, expected behavior, and trigger. Separate an unwanted entrance replay from a dropped frame, layout shift, discontinuous loop, or a full page reload. Use existing recordings, screenshots, logs, and the user's description. A screenshot establishes appearance, not temporal behavior.

Inspect the component, its parent, identity keys, effects, data updates, and relevant CSS together. Browser or computer-control verification requires explicit user approval. Continue source investigation and focused checks without it, and label an unobserved runtime cause as a hypothesis.

## Trace the cause

| Symptom | Inspect | Smallest useful correction |
|---|---|---|
| Entrance plays on each data refresh | Changing keys, nested component definitions, loading branches, route boundaries, presence wrappers | Keep the intended instance mounted. Give genuine transitions stable identity. A rerender alone does not prove a remount. |
| Animation resets without a remount | Effects restarting controls, changing animation names or targets, state resetting progress, polling replacing objects | Tie the animation to the event that should start it. Distinguish data refresh from entering a new state. |
| Skeleton or content jumps | Different loading/final dimensions, fonts, images, conditional siblings, conflicting layout animations | Reserve the final geometry and let one mechanism own the position change. |
| Spinner or loop snaps at its boundary | First/last keyframes, transform origins, SVG dash phase, rotation count, delay, repeat mode | Make the cycle continuous. Keep duration and rotation count separate; doubling both preserves average angular speed. |
| Motion fights itself | CSS transitions plus Motion on the same property, competing transforms, parent/child layout effects | Choose one owner per animated property. Preserve any independent transforms or interactions. |
| Only development or background tabs fail | Strict Mode effect setup/cleanup, hot reload, focus revalidation, visibility handlers, elapsed-time calculations | Reproduce the actual trigger. Do not disable Strict Mode, polling, or updates to conceal it. |
| Frames drop while identity stays stable | Layout reads/writes, expensive paint, per-frame React state, timers, large blur/mask regions | Locate the expensive work before optimizing. Do not infer GPU compositing from a CSS property alone. |

Trace one likely cause at a time. Temporary instrumentation should distinguish render, mount, unmount, and animation start events. Remove it after diagnosis. Avoid blanket memoization, arbitrary delays, global "has animated" flags, or deleting useful transitions to suppress the symptom.

## Verify the repair

Check the trigger that failed and the nearest behavior that must still work, such as polling without replay and a deliberate entrance with animation. Cover reduced-motion behavior if the change affects it. Use a focused test when identity or state logic can be protected meaningfully.

With browser approval, observe the original sequence, including a complete loop or tab round-trip when relevant. Otherwise report what source checks established and that visual behavior remains unverified. Finish with the cause or remaining hypothesis, the change, and the evidence. A successful build is not proof of smooth motion.
