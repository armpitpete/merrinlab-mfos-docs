# Design Decisions

## 1. Documentation repo decision

Shared MFOS/MerrinLab documentation belongs in its own repo:

```text
merrinlab-mfos-docs
```

Reason:

- The docs are for the whole instrument family.
- `merrinlab-vcv` is one technical project, not the family documentation hub.
- A separate docs repo makes linking and maintenance cleaner.

## 2. Standalone instrument rule

Each instrument should remain standalone unless a specific issue says otherwise.

Current standalone instruments:

- Alien Screamer
- 16-Step Sequencer
- Echo Rockit
- Ultimate Synth

Do not merge these into one large app yet.

## 3. Patch bus rule

Use the MerrinLab patch bus for cross-instrument communication.

Do not copy one instrument's full code into another instrument repo.

Patch bus protocol:

```text
merrinlab.patch.v0.1
```

## 4. Faceplate rule

Use image-based faceplates when the original panel detail matters.

Transparent browser controls may sit over the image.

Avoid redrawing a complex original panel entirely in CSS if it causes visual instability.

## 5. Playability over exact simulation

These browser instruments are not full circuit simulations.

Priority order:

1. stable sound
2. playable controls
3. recognisable MFOS-inspired behaviour
4. clear visual faceplate
5. patch-bus compatibility
6. deeper circuit accuracy only when it does not break playability

## 6. Alien Screamer Sync Effect decision

The real Alien Screamer Sync Effect resets the VCO ramp by using LFO square-wave edges to disturb the VCO comparator bias.

The browser implementation tried a resettable ramp model. It was not reliable enough.

Observed problems:

- no audible effect
- muddy reset clamp
- too-short reset clamp disappearing
- doubled reset feeling
- risk of broken sound when recreating oscillator nodes

Decision:

Keep the faceplate label:

```text
Sync Effect
```

Implement browser behaviour as:

```text
LFO-edge-driven Sync Bite
```

Internal name:

```text
syncBite
```

Do not describe the current browser version as a true ramp reset.

## 7. Alien Screamer VCO range decision

Current browser mapping:

```text
0  ~= 100 Hz
10 ~= 22,000 Hz
```

Reason:

The control should start at a low audible base and reach the edge/outside of hearing.

## 8. Sequencer connection decision

The Digital 16-Step Sequencer is the first working sender.

The Classic page stays visual/non-functional until a specific issue says otherwise.

Confirmed working connection:

- Digital Sequencer sends `pitch-cv`
- Digital Sequencer sends `gate`
- Digital Sequencer sends `clock`
- Alien Screamer receives pitch and gate
- Alien Screamer responds when Drone is off

## 9. Stop rule

After a working checkpoint:

1. test live page
2. confirm behaviour by ear/eye
3. commit/push
4. sync local repo
5. update notes if a decision changed
6. only then start the next issue

Do not keep adding features after a stable checkpoint.
