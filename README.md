# SableOS common platform contracts

Status: **current common architecture — 2026-09-24**

```text
Pixel 7 / panther   REFERENCE_FROZEN / R9 physical acceptance PASS
K1/K2               multi-device artifact/deployment foundation MERGED
Titan 2             active keyboard-first PORTABILITY / N0 research
Titan 2 Elite       next independent keyboard-first PORTABILITY target
Q27                 RESEARCH / future candidate
Production signing  deferred
```

This repository owns common Sable semantic, design, portability, support and
release contracts. It does not own target-specific BSP/device code.

## Current product architecture

```text
common Sable applications + semantics
        |
        +-- touch-first profile
        |     -> frozen Panther reference
        |
        +-- keyboard-first profile
              -> Titan 2
              -> Titan 2 Elite
              -> future Q27
        |
        v
bounded device adapters
```

Current HOME is standalone `org.sableos.launcher` / SableLauncher.
Launcher3QuickStep is Recents/Overview/task/gesture substrate only.

## Accepted common application family

The frozen Panther reference proves the current product family:

- SableLauncher;
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

Settings is global appearance authority; common apps consume
Follow-system/Light/Dark and shared semantic design roles.

## Current active architecture documents

- [Architecture](docs/ARCHITECTURE.md)
- [Device support levels](docs/DEVICE_SUPPORT_LEVELS.md)
- [Portability rules](docs/PORTABILITY_RULES.md)
- [Release model](docs/RELEASE_MODEL.md)
- [Camera enhancement model](docs/CAMERA_ENHANCEMENT_MODEL.md)
- [Keyboard-device tools](docs/KEYBOARD_DEVICE_TOOLS.md)

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
- keyboard-only accessibility;
- touch as a secondary path;
- common Sable Keyboard/IME boundary;
- common Sable Camera/system-image boundary.

Common app semantics do not fork by device model.
