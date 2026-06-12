# Instrument Index

## Purpose

This index lists the current MerrinLab MFOS-related software instruments and what each repo is for.

## Current repos

### `merrinlab-alien-screamer`

Standalone browser synth inspired by the MFOS Alien Screamer.

Current status:

- playable browser instrument
- image-based faceplate
- transparent browser controls over the panel image
- VCO Frequency works
- LFO Modulation works
- LFO Frequency works
- Output Level works
- Modulation Shape works
- Power works
- Sync Effect works as Sync Bite
- receives patch-bus `pitch-cv` and `gate`
- confirmed working with the Digital 16-Step Sequencer

Important decisions:

- VCO Frequency maps from about 100 Hz at 0 to about 22,000 Hz at 10.
- Sync Effect is implemented as LFO-edge-driven Sync Bite, not true ramp reset.
- Do not restart the ramp-reset experiment inside the current engine unless it is a separate contained experiment.

### `merrinlab-16-step-sequencer`

Software 16-step sequencer inspired by analogue sequencing and quantized variable clock behaviour.

Current status:

- Classic page is a visual/hardware-style prototype
- Digital page has basic patch-bus output
- Digital page sends `pitch-cv`, `gate`, and `clock`
- Run / Stop / Reset / Manual Step work on the Digital page
- confirmed controlling Alien Screamer through the patch bus

Important decisions:

- Keep Classic non-functional unless a specific issue says otherwise.
- Use the Digital page as the first working sender.
- Do not copy Alien Screamer code into the sequencer.

### `mfos-echo-rockit`

Browser-based software version of the MFOS Echo Rockit sound producer/processor.

Current status:

- playable prototype in progress
- core sound and panel behaviour being refined by ear and visual testing

Important decisions:

- Keep Echo Rockit as its own repo.
- Do not merge it into Alien Screamer or the sequencer.
- Add patch-bus compatibility later only when a contained issue asks for it.

### `merrinlab-ultimate-synth`

Standalone/software synth inspired by the MFOS Ultimate and Ultimate Expander.

Current status:

- larger instrument project
- faceplate and control layout work in progress

Important decisions:

- Keep it separate from Alien Screamer, Echo Rockit, and the sequencer.
- Do not use another instrument's layout as a direct template unless a specific issue says so.

### `merrinlab-vcv`

Wider MerrinLab / VCV / plugin work.

Current status:

- separate technical project
- no longer the correct long-term home for general MFOS documentation

Important decisions:

- Shared MFOS documentation now belongs in `merrinlab-mfos-docs`.
- `merrinlab-vcv` should link to the docs repo later rather than owning shared documentation.

## Family rule

Each repo stays responsible for its own instrument.

The docs repo owns shared rules, shared protocol notes, issue templates, and cross-project decisions.
