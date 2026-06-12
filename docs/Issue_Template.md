# Shared Issue Template

Use this format for MerrinLab MFOS project issues.

```md
# Issue — [specific change]

## Goal
[One clear goal. One issue should do one thing.]

## Scope
[What is allowed in this issue.]

## Required behaviour
- [specific behaviour]
- [specific behaviour]
- [specific behaviour]

## Do not change
- [protected area]
- [protected area]
- [protected area]

## Good enough
[Clear stopping condition. Describe when the issue should stop.]
```

## Good issue examples

```md
# Issue — Add simple connection status indicators

## Goal
Make it obvious when Alien Screamer and the 16-Step Sequencer are talking through the patch bus.

## Scope
Add small status indicators only.

## Required behaviour
- Alien Screamer shows when it last received sequencer pitch/gate.
- Sequencer shows when it last sent pitch/gate/clock.
- Keep the existing patch bus protocol.

## Do not change
- Do not change sound behaviour.
- Do not redesign either panel.
- Do not change patch-bus message names.

## Good enough
With both pages open, it is visually obvious that messages are being sent and received.
```

```md
# Issue — Add Echo Rockit patch-bus input

## Goal
Allow Echo Rockit to receive simple patch-bus control messages.

## Scope
Receive messages only. Do not redesign Echo Rockit.

## Required behaviour
- Receive `gate` if useful.
- Receive `clock` if useful.
- Ignore unsupported message types safely.

## Do not change
- Do not change the existing sound unless required for the input.
- Do not copy code from another synth.
- Do not change the panel layout.

## Good enough
Echo Rockit can react to one useful external patch-bus message without breaking its current standalone behaviour.
```

## Bad issue examples

Avoid:

```text
Improve the synth
Make the panel better
Connect everything
Make it authentic
Fix the sound
```

These are too broad.

## Issue rule

If an issue starts changing more than one system, stop and split it.

Examples:

- UI status indicators are one issue.
- Sound behaviour is one issue.
- Patch-bus protocol changes are one issue.
- Faceplate layout is one issue.
- Documentation updates are one issue.
