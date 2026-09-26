# Zinwa Q27 future portability notes

Status: **RESEARCH_CONTEXT — future product candidate, not an active build target**

Date: 2026-09-26

Q27 remains intentionally outside the active Titan 2 first-build sequence.

## Current role

```text
support level     RESEARCH
assurance         unqualified
interaction       keyboard-first future
capability record docs/device-capabilities/ZINWA_Q27.md
artifact register blocked
flash plan        blocked
flash             blocked
```

K1/K2 includes a fail-closed Q27 device adapter so the canonical engineering
interface already recognizes the name without implying support.

## Current capability posture

Q27 is a compact keyboard-device candidate with public/planning information only.
It must not inherit Titan 2 or Titan 2 Elite evidence.

```text
DISPLAY_CLASS=COMPACT_TALL_KEYBOARD_CANDIDATE
PANEL=AOD_CANDIDATE_REQUIRES_VALIDATION
KEYBOARD_PROFILE=q27_required
POINTER_PROFILE=unknown_until_hardware
CRITICAL_TEXT_ENTRY=required_not_validated
PUBLIC_BUILD_IMAGE=FAIL_CLOSED
PUBLIC_FLASH=NO
```

## Promotion condition

Promote Q27 from RESEARCH only after shipped/current hardware and firmware
provide enough evidence for:

- exact product/board/SoC identity;
- stock restore source and hashes;
- bootloader/fastboot/fastbootd behavior;
- partition/AVB topology;
- Treble/vendor compatibility;
- display geometry, panel type, refresh, density and AOD/doze behavior;
- physical keyboard, touch surface and pointer/mouse mapping;
- software keyboard fallback and critical text-entry behavior;
- camera/audio/sensor/telephony baselines;
- practical N0 restore/deployment strategy.

Prototype/community/public evidence is useful for hypotheses, not runtime or
release claims.

## Common-product rule

Q27 should consume the same keyboard-first Sable semantic contracts as Titan
family devices where compatible. Do not create a Q27-specific launcher, Camera,
Keyboard/IME or application fork.

Device-specific input/display/vendor behavior belongs in a bounded Q27 adapter
and Q27 capability profile.

## Android substrate

A future Q27 Android-version/vendor choice does not by itself justify a separate
Sable application/product branch. Common source should use appropriate API
compatibility and runtime testing.

## Current execution priority

```text
Panther frozen reference
  -> K1/K2 complete
  -> keyboard-first common design
  -> Titan 2 N0_A16 profile/substrate work
  -> Titan 2 Elite independent baseline
  -> reassess Q27 on shipped/current hardware
```
