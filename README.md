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

Historical R8/R9 planning documents are retained with explicit superseded
classification.

## K1/K2 boundary

Artifact identity supports multiple artifact kinds and is independent of physical
serial. Deployment safety/evidence is common; partition/transport semantics are
device-adapter-owned.

Panther is the qualified target-files/A-B adapter. Titan 2, Titan 2 Elite and
Q27 remain fail-closed for release artifact registration/flash until independently
qualified.

## Active design work

Keyboard-first product design now covers:

- deterministic visible focus;
- arrows/D-pad navigation;
- Enter/Space activation;
- Back/Escape;
- type-to-search;
- command/shortcut navigation;
- stable focus restoration;
- square/near-square responsive layout;
- compact AMOLED keyboard-device layouts;
- secondary glance display policy;
- AOD/pulse/attention-surface capability profiles;
- keyboard-only accessibility;
- touch as a secondary path;
- per-device keyboard and pointer-surface profiles;
- critical text-entry fallback for setup, pairing and lockscreen flows;
- per-device capability matrix for Titan 2, Titan 2 Elite and Q27;
- common Sable Keyboard/IME boundary;
- common Sable Camera/system-image boundary.

Common app semantics do not fork by device model. Hardware, display, attention,
keyboard and critical-input differences are expressed through profiles.
