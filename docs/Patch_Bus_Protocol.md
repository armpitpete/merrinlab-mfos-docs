# Patch Bus Protocol

## Purpose

The MerrinLab patch bus lets separate browser instruments talk to each other without copying one instrument's code into another repo.

It is currently used to connect the Digital 16-Step Sequencer to Alien Screamer.

## Protocol name

```text
merrinlab.patch.v0.1
```

## Browser channel

```text
BroadcastChannel: merrinlab-patch-bus
```

A local `CustomEvent` fallback may also be used.

## Message shape

```js
{
  protocol: "merrinlab.patch.v0.1",
  source: "module-id",
  type: "message-type",
  time: performance.now(),
  payload: {}
}
```

## Required receiver behaviour

Receivers should ignore:

- disabled patch bus state
- empty messages
- messages from their own `source`
- messages with the wrong `protocol`
- message types they do not understand

## Standard message types

### `pitch-cv`

Used to send pitch/control-voltage style information.

Recommended payload:

```js
{
  value: 0,        // voltage/octave style offset, where 0 is the receiver's base pitch
  pitch: 60,       // optional MIDI-style pitch value
  step: 1          // optional sequencer step number
}
```

Receiver rule:

- Treat `value` as a relative control amount.
- Do not assume the sender owns the whole pitch system.
- Local pitch controls should still work unless a specific issue says otherwise.

### `gate`

Used to open or close a voice.

Recommended payload:

```js
{
  open: true,
  step: 1
}
```

Receiver rule:

- `open: true` opens the voice.
- `open: false` closes the voice.
- Instruments with a Drone switch may ignore gate closure while Drone is on.

### `clock`

Used to announce sequencer timing pulses or active step changes.

Recommended payload:

```js
{
  step: 1,
  reset: false
}
```

Receiver rule:

- Use `clock` only when useful.
- Do not make sound behaviour depend on `clock` unless a specific issue says so.

### `panic`

Used to stop or mute a receiver safely.

Recommended payload:

```js
{
  reason: "manual panic"
}
```

Receiver rule:

- Mute sound safely.
- Do not destroy the UI state.

## Current confirmed connection

The Digital 16-Step Sequencer sends:

- `pitch-cv`
- `gate`
- `clock`

Alien Screamer receives:

- `pitch-cv`
- `gate`
- `panic`

Confirmed behaviour:

- Sequencer changes Alien Screamer pitch/control.
- Gates work when Alien Screamer Drone is off.
- Clock messages are available for later use.

## Next protocol improvement

Add simple connection status indicators.

Good enough:

- Sequencer shows when it last sent pitch/gate/clock.
- Alien Screamer shows when it last received pitch/gate.
- Protocol stays unchanged.
