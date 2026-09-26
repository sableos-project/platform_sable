# SableOS common platform contracts

Status: **current common architecture — 2026-09-26**

```text
Pixel 7 / panther   REFERENCE_FROZEN / R9 Hub V1 physical acceptance PASS
R9 Hub V1           MERGED / PR #110
Keyboard-first V1   MERGED / PR #108
K1/K2               multi-device artifact/deployment foundation MERGED
Titan 2             active keyboard-first PORTABILITY / N0_A16 strategy
Titan 2 Elite       next independent keyboard-first PORTABILITY target
Q27                 RESEARCH / future candidate
Production signing  deferred
```

This repository owns common Sable semantic, design, portability, support and
release contracts. It does not own target-specific BSP/device code.

Current image authority from the private integration repository:

```text
R9_PANTHER_ACCEPTED_SOURCE=edf62e5bb08372a1395841d6cc5d78d3148a7695
R9_PANTHER_TARGET_FILES_SHA256=a0b359613c4f30e9a834fba212e0b044a97d63ed0537c59471c31b99b627d285
```

## Current product architecture

```text
common Sable applications + semantics
        |
        +-- touch-first profile
        |     -> frozen Panther reference
        |
        +-- keyboard-first profile family
              -> Titan 2 profile
              -> Titan 2 Elite profile
              -> Q27 profile
        |
        v
bounded device adapters
```

Sable Hub V1 is the accepted communications surface:

```text
Priority | Messages | Email | People
```

Hub is an aggregator and interaction surface. Source applications/providers keep
ownership of their accounts, credentials, private databases and protocol stacks.

## Accepted common application family

The frozen Panther reference proves the current product family:

- Sable HOME / launcher presentation;
- Sable Calculator;
- Sable Sudoku;
- Sable Minesweeper;
- Sable 2048;
- Sable Media;
- Sable Reader;
- Sable Text Reader;
- Sable Hub / Messages;
- Sable Mail;
- Sable Weather;
- Sable Calendar.

Reader and Text Reader are separate products.

Open appearance/polish issues remain intentionally open until Titan 2 SableOS
install closure proves or supersedes them.

## Current active architecture documents

- [Architecture](docs/ARCHITECTURE.md)
- [Device support levels](docs/DEVICE_SUPPORT_LEVELS.md)
- [Portability rules](docs/PORTABILITY_RULES.md)
- [Release model](docs/RELEASE_MODEL.md)
- [Camera enhancement model](docs/CAMERA_ENHANCEMENT_MODEL.md)
- [Keyboard-device tools](docs/KEYBOARD_DEVICE_TOOLS.md)
- [Keyboard-first device profile model](docs/KEYBOARD_FIRST_DEVICE_PROFILE_MODEL.md)
- [Display and attention profile model](docs/DISPLAY_AND_ATTENTION_PROFILE_MODEL.md)
- [Keyboard and pointer profile model](docs/KEYBOARD_AND_POINTER_PROFILE_MODEL.md)
- [Critical text-entry gates](docs/CRITICAL_TEXT_ENTRY_GATES.md)
- [Keyboard-first reference intake](docs/KEYBOARD_FIRST_REFERENCE_INTAKE.md)
- [Device capability matrix](docs/device-capabilities/DEVICE_CAPABILITY_MATRIX.md)
- [Keyboard-first Settings UX](docs/design/KEYBOARD_FIRST_SETTINGS_UX.md)
- [Settings search UX](docs/design/SETTINGS_SEARCH_UX.md)
- [Hardware diagnostics and dialer-code UX](docs/design/HARDWARE_DIAGNOSTICS_AND_DIALER_CODES.md)
- [Sable Start keyboard-first UX](docs/design/SABLE_START_KEYBOARD_FIRST_UX.md)
- [Sable Start screen design](docs/design/SABLE_START_SCREEN_DESIGN.md)
- [Sable Start functional references](docs/design/SABLE_START_REFERENCES.md)

Historical R8/R9 planning documents are retained with explicit superseded
classification.

## Keyboard-device capability posture

The current profile model is intentionally not a generic keyboard-phone profile.
Titan 2, Titan 2 Elite and Q27 must not inherit each other's display, AOD,
keyboard, pointer, SubScreen, critical-input or release evidence.

```text
Titan 2
  profile: square keyboard device + rear SubScreen candidate
  AOD: no by default
  status: N0_A16 planning, no Sable artifact/boot yet

Titan 2 Elite
  profile: compact AMOLED keyboard-device candidate
  AOD: candidate, requires hardware/power/doze validation
  status: independent baseline pending

Q27
  profile: compact AMOLED keyboard-device candidate
  AOD: candidate, requires shipped/current hardware validation
  status: research only
```

Public specifications are planning inputs only. They do not replace local
hardware evidence, firmware binding, or release acceptance.

## Settings UX posture

Sable Settings must preserve the familiar Android Settings structure while
adding profile-aware keyboard, display, attention, SubScreen, critical text-entry
and diagnostics surfaces.

```text
ANDROID_SETTINGS_STRUCTURE=PRIMARY
SETTINGS_SEARCH_REQUIRED=YES
KEYBOARD_NAVIGATION_REQUIRED=YES
HARDWARE_DIAGNOSTICS_BRIDGE=GATED
RAW_FACTORY_TEST_DIRECT_LAUNCH=NO_BY_DEFAULT
```

Titan 2 factory-test evidence, including the `*#*#3377#*#*` hardware test path,
is treated as a diagnostics input, not as a normal user Settings hierarchy.

## Sable Start UX posture

Sable Start remains the Sable HOME surface. It preserves Android Home / All Apps /
app-info expectations while making type-to-launch, command entry, Hub glance,
visible focus, app privacy summaries and pointer-mode boundaries first-class on
keyboard devices.

```text
SABLE_START_HOME=YES
ANDROID_HOME_MENTAL_MODEL=KEEP
TYPE_TO_LAUNCH=YES
COMMAND_SEARCH_ENTRY=YES
TITAN2_SQUARE_LAYOUT=YES
COMPACT_KEYBOARD_LAYOUT=YES
FUNCTIONAL_REFERENCE_ONLY=YES
CODE_IMPORT=NO
```

Functional inspirations are documented as behavior references only: Android
Launcher/Pixel Launcher/Launcher3, Niagara-style vertical access, BlackBerry OS7
keyboard shortcuts, BlackBerry OS10 Hub/Peek/Flow, Windows Phone/Metro/Zune visual
structure, OpenMiniLaunch/Mink command-box ideas, Commander command overlay, and
keyboard/pointer learnings from Pastiera/Plektra and q25toolbox.

## K1/K2 boundary

Artifact identity supports multiple artifact kinds and is independent of physical
serial. Deployment safety/evidence is common; partition/transport semantics are
device-adapter-owned.

Panther is the qualified target-files/A-B adapter. Titan 2, Titan 2 Elite and
Q27 remain fail-closed for release artifact registration/flash until independently
qualified.

## Cross-repository alignment

Current adjacent repositories should use this repository as the common semantic
and capability-contract authority:

```text
platform_manifest
  exact composition and artifact identity

build
  public build/sign/verify/package/flash contracts

device_sable_titan2
  Titan 2 public device-adapter boundary

aimindseye/unihertz-titan2
  Titan-family research handoff and private-evidence boundary

aimindseye/sableos
  private integration and pre-public build-target work
```

Cross-repo references should not copy profile data as independent truth. They
should link or cite the current platform_sable profile docs and keep release or
flash gates fail-closed until evidence exists.

## Active design work

Keyboard-first product design now covers:

- deterministic visible focus;
- arrows/D-pad navigation;
- Enter/Space activation;
- Back/Escape;
- type-to-search;
- type-to-launch from Sable Start;
- command/shortcut navigation;
- stable focus restoration;
- square/near-square responsive layout;
- compact AMOLED keyboard-device layouts;
- secondary glance display policy;
- AOD/pulse/attention-surface capability profiles;
- keyboard-only accessibility;
- touch as a secondary path;
- explicit pointer/mouse mode boundaries;
- per-device keyboard and pointer-surface profiles;
- Settings structure compatible with Android user expectations;
- Settings search aliases for profile, diagnostics and factory-test concepts;
- Start screen app privacy summaries and keyboard app actions;
- critical text-entry fallback for setup, pairing and lockscreen flows;
- gated diagnostics bridge for hardware/factory test surfaces;
- common Sable Keyboard/IME boundary;
- common Sable Camera/system-image boundary.

Common app semantics do not fork by device model. Hardware, display, attention,
keyboard and critical-input differences are expressed through profiles.
