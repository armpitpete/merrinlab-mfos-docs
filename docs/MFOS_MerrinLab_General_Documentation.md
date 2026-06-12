# MFOS / MerrinLab General Documentation

## Purpose

This document is the shared guide for MerrinLab software instruments inspired by Music From Outer Space style hardware.

It exists so the separate repos do not drift into different rules, different naming, or incompatible patch behaviour.

## Current repos

| Repo | Role | Current status |
|---|---|---|
| `merrinlab-ultimate-synth` | Browser/software version inspired by MFOS Ultimate + Expander | Faceplate and controls in progress |
| `merrinlab-16-step-sequencer` | Software 16-step sequencer | Digital page sends patch-bus control messages |
| `merrinlab-alien-screamer` | Standalone Alien Screamer browser synth | Playable, image faceplate, patch-bus receiver working |
| `mfos-echo-rockit` | Browser Echo Rockit sound producer/processor | Playable prototype in progress |
| `merrinlab-vcv` | Wider MerrinLab / VCV / plugin work | Separate technical project |
| `merrinlab-mfos-docs` | Shared documentation repo | Family documentation home |

## Project rule

Each instrument should stay as its own repo and its own standalone browser instrument unless a specific issue says otherwise.

Do not copy one instrument's full code into another instrument.

Shared behaviour should be connected through small protocols, shared documents, or duplicated tiny helpers only when necessary.

## Design principle

These are not full circuit simulations.

They are playable browser/software instruments that respect the original hardware idea, panel logic, and sound character.

Use this priority order:

1. stable sound
2. playable controls
3. recognisable MFOS-inspired behaviour
4. clear visual faceplate
5. patch-bus compatibility
6. deeper circuit accuracy only when it does not break playability

## Faceplate rule

Use the original panel or a cleaned image-based faceplate when that gives the most accurate result.

Avoid trying to redraw complex historic panels entirely in CSS if the visual result becomes unstable.

Transparent browser controls may sit over a faceplate image.

Required behaviour:

- visible panel stays accurate enough
- invisible browser controls remain usable
- no clipped controls
- no internal scrollbars
- no accidental layout redesign during sound or patch-bus work

## Patch bus rule

The shared patch bus protocol is:

```text
merrinlab.patch.v0.1
```

The browser transport currently uses:

```text
BroadcastChannel: merrinlab-patch-bus
```

with local `CustomEvent` fallback when needed.

## Standard message types

### `pitch-cv`

Used to send pitch/control-voltage style information.

Recommended payload:

```js
{
  value: 0,
  pitch: 60,
  step: 1
}
```

### `gate`

Used to open or close a voice.

Recommended payload:

```js
{
  open: true,
  step: 1
}
```

### `clock`

Used to announce sequencer steps or timing pulses.

Recommended payload:

```js
{
  step: 1,
  reset: false
}
```

### `panic`

Used to stop/mute a receiver safely.

Recommended payload:

```js
{
  reason: "manual panic"
}
```

## Current confirmed connection

The Digital 16-Step Sequencer can control Alien Screamer through the patch bus.

Confirmed behaviour:

- sequencer sends `pitch-cv`
- sequencer sends `gate`
- sequencer sends `clock`
- Alien Screamer receives `pitch-cv`
- Alien Screamer receives `gate`
- Alien Screamer pitch/control changes when the sequencer runs
- Alien Screamer gate works when Drone is off

## Alien Screamer current decisions

### VCO Frequency

Current browser mapping:

```text
0  ~= 100 Hz
10 ~= 22,000 Hz
```

This was chosen because the control should go from a low audible base into the edge/outside of hearing.

### Sync Effect

The panel keeps the label:

```text
Sync Effect
```

The browser behaviour is:

```text
LFO-edge-driven Sync Bite
```

Reason:

The original circuit resets the VCO ramp by using LFO square-wave edges to disturb the VCO comparator bias. A direct resettable-ramp browser version was attempted, but it was not reliable enough in this implementation.

Observed problems:

- no audible effect
- muddy reset clamp
- too-short reset clamp disappearing
- doubled reset feeling
- risk of broken audio when recreating oscillator nodes

Decision:

Use a stable musical approximation:

- detect leading and trailing LFO square-wave edges
- apply a short pitch/drive/filter bite
- keep timing tied to LFO Frequency
- avoid muddy gating
- avoid doubled sound
- do not describe the browser version as a true ramp reset unless that is later rebuilt successfully

Internal name:

```text
syncBite
```

## Sequencer current decisions

The Classic page remains a visual/hardware-style prototype.

The Digital page is the first patch-bus sender.

Current Digital page behaviour:

- Run starts a simple sequencer loop
- Stop halts it and closes the gate
- Reset returns to step 1
- Manual Step sends one step while stopped
- Clock Rate changes the loop speed
- each active step sends pitch, gate, and clock messages

## Echo Rockit current direction

Echo Rockit is a separate playable browser sound producer/processor.

Do not merge it into Alien Screamer or the sequencer.

Future shared behaviour should use the same patch bus where useful.

## Ultimate Synth current direction

Ultimate Synth is the larger MFOS Ultimate + Expander style project.

It should remain a separate standalone instrument.

Do not use the Alien Screamer or Echo Rockit panel logic as a direct layout model unless the issue explicitly asks for it.

## Documentation rule for every repo

Each repo should eventually contain:

```text
README.md
NOTES.md
PATCHING.md or CONNECTIONS.md when relevant
```

Minimum purpose:

- `README.md` explains what the project is and how to open/use it.
- `NOTES.md` records design decisions and compromises.
- `PATCHING.md` or `CONNECTIONS.md` explains patch-bus messages and compatible instruments.

## Issue-writing rule

Each issue should be small and contained.

Use this pattern:

```md
# Issue — [specific change]

## Goal
[One clear goal]

## Scope
[What is allowed]

## Required behaviour
- [specific behaviour]
- [specific behaviour]

## Do not change
- [protected area]
- [protected area]

## Good enough
[Clear stopping condition]
```

Avoid vague issues such as:

```text
Improve the synth
Make it better
Connect everything
Redesign the panel
```

## Stop rules

Do not continue into a new feature after a working checkpoint.

After each working feature:

1. test live page
2. confirm behaviour by ear/eye
3. commit/push
4. sync local repo
5. write/update notes if a design decision changed
6. only then start the next issue

## Next sensible shared issue

Add simple connection status indicators.

Goal:

- Alien Screamer shows when it last received sequencer pitch/gate.
- Sequencer shows when it last sent pitch/gate/clock.
- Keep the existing patch bus protocol.
- Do not change sound behaviour.
- Do not redesign either panel.

Good enough:

With both pages open, it is visually obvious that messages are being sent and received.
