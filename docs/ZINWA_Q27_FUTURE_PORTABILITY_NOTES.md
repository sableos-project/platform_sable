# Zinwa Q27 future portability notes

Status: **RESEARCH_CONTEXT — future product candidate, not an active build target**

Date: 2026-09-24

Q27 remains intentionally outside the active Titan 2 / Titan 2 Elite bring-up
sequence.

## Current role

```text
support level     RESEARCH
assurance         unqualified
interaction       keyboard-first future
artifact register blocked
flash plan        blocked
flash             blocked
```

K1/K2 includes a fail-closed Q27 device adapter so the canonical engineering
interface already recognizes the name without implying support.

## Promotion condition

Promote Q27 from RESEARCH only after shipped/current hardware and firmware
provide enough evidence for:

- exact product/board/SoC identity;
- stock restore source and hashes;
- bootloader/fastboot/fastbootd behavior;
- partition/AVB topology;
- Treble/vendor compatibility;
- physical keyboard/trackpad mapping;
- display geometry/input association;
- camera/audio/sensor/telephony baselines;
- practical N0 restore/deployment strategy.

Prototype/community evidence is useful for hypotheses, not runtime claims.

## Common-product rule

Q27 should consume the same keyboard-first Sable semantic contracts as Titan
family devices where compatible. Do not create a Q27-specific launcher, Camera,
Keyboard/IME or application fork.

Device-specific input/display/vendor behavior belongs in a bounded Q27 adapter.

## Android substrate

A future Q27 Android-version/vendor choice does not by itself justify a separate
Sable application/product branch. Common source should use appropriate API
compatibility and runtime testing.

## Current execution priority

```text
Panther frozen reference
  -> K1/K2 complete
  -> keyboard-first common design
  -> Titan 2 N0 work
  -> Titan 2 Elite independent work
  -> reassess Q27 on shipped hardware
```
