# MerrinLab MFOS Docs

Shared documentation for MerrinLab software instruments inspired by Music From Outer Space hardware.

This repo is documentation-only. It is the control manual for the MerrinLab MFOS software instrument family.

## Purpose

Use this repo to keep shared rules in one place:

- instrument index
- patch-bus protocol
- design decisions
- issue-writing rules
- shared stop rules
- cross-repo connection notes

## Current instrument repos

| Repo | Role |
|---|---|
| `merrinlab-alien-screamer` | Standalone Alien Screamer browser synth |
| `merrinlab-16-step-sequencer` | Software 16-step sequencer and patch-bus sender |
| `mfos-echo-rockit` | Browser Echo Rockit sound producer/processor |
| `merrinlab-ultimate-synth` | Browser/software synth inspired by MFOS Ultimate + Expander |
| `merrinlab-vcv` | Wider VCV / plugin work |

## Core rule

Each instrument stays in its own repo.

Shared behaviour is documented here and connected through small protocols such as the MerrinLab patch bus.

Do not copy full instrument code from one repo into another unless a specific issue explicitly requires it.

## Main docs

- [General Documentation](docs/MFOS_MerrinLab_General_Documentation.md)
- [Patch Bus Protocol](docs/Patch_Bus_Protocol.md)
- [Instrument Index](docs/Instrument_Index.md)
- [Design Decisions](docs/Design_Decisions.md)
- [Issue Template](docs/Issue_Template.md)

## What belongs here

- shared documentation
- protocol notes
- decisions that affect more than one repo
- issue templates
- plain-language architecture notes
- instrument index

## What does not belong here

- synth source code
- audio files
- build outputs
- Affinity / PSD working files
- passwords, tokens, emails, or server paths
- large private artwork files
