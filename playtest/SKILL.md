---
name: playtest
description: Set up and play a repeatable game scenario, then hand it back ready for the user to try.
disable-model-invocation: true
---

# Playtest

1. Identify the moment the user wants to judge, the build and platform, and what they need to see or feel. Set the starting point just before the relevant input, encounter, or transition. For a reload or reconnect bug, include both the initial run and the triggering transition.
2. Reuse an existing checkpoint, seed, save copy, or debug entry point in the intended checkout. Keep setup local to the test unless the user requests a shared environment. Preserve the behavior under test: if the user wants to judge damage without dying, hits must still register and reduce visible health. Disclose any test overrides.
3. Play the scenario through the normal controls. For a comparison, hold the starting conditions and route constant, vary the requested parameter, and measure the outcome the player cares about. For co-op behavior, identify which clients were independently controlled and which participants were simulated. Record a short clip when timing or movement matters.
4. Restore the starting point and verify the user can enter or repeat it. Lead with the playable link or native entry point, the action to try, and how to reset. Leave the test build available. Report what happened, the environment, and any platform or multiplayer behavior you could not exercise.
