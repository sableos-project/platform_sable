# SableOS common architecture

Status: **current normative architecture — 2026-09-24**

## Product structure

SableOS uses one common product core with multiple interaction profiles and
bounded device adapters.

```text
common product semantics
    launcher / hub / apps / appearance / privacy contracts
        |
        +-- touch-first presentation
        +-- keyboard-first presentation
        |
        v
device adapter
    display/input/camera/boot/vendor/telephony specifics
        |
        v
Android framework + vendor BSP + hardware
```

## Launcher

`org.sableos.launcher` / SableLauncher owns HOME and the Sable-facing
Start/All Apps/Search/Peek/app-context experience.

Launcher3QuickStep remains a private platform dependency for
Recents/Overview/task/gesture substrate. It is not HOME eligible.

Historical SableStart source remains presentation/history reference.

## Appearance

Settings is the global appearance authority.

Common modes:

```text
Follow system
Light
Dark
```

Apps consume semantic roles rather than inventing independent theme stores.
Panther physical acceptance proved Light/Dark propagation across Sable apps.

## Application boundaries

Applications own capabilities; the Sable shell organizes people, attention and
actions.

Common app source must not fork merely because a target has a physical keyboard,
different SoC, square display or vendor BSP.

## Reader split

Current products are separate:

```text
Sable Reader
    publication / EPUB / Readium / bookshelf / reading state / TTS

Sable Text Reader
    TXT / ACTION_VIEW / SEND / PROCESS_TEXT
    paste/edit
    local-only TTS + WAV
    OCR Latin + Devanagari
```

Sable Text Reader is bounded for local/offline behavior and does not inherit the
old network translation/model-download design.

## Camera

Sable Camera is a common system-image workstream.

Architecture:

```text
camera-core
camera-capabilities
device-profiles
ui
platform-integration
```

Use normal Camera2/vendor HAL capability first. SYSTEM_CAMERA privilege is
device-specific and only justified by physical evidence plus negative
third-party discovery/access tests.

## Keyboard/input

Separate:

```text
common Sable Keyboard / IME
    text composition, layouts, symbols, languages, emoji

device physical-keyboard adapter
    scan/keylayout/keycharacter mapping
    Fn/Sym/vendor keys
    backlight
    pointer/trackpad
```

Device scan-code quirks do not belong in common IME or app code.

## Keyboard-first interaction

Required across launcher and first-party apps:

- deterministic visible focus;
- arrow/D-pad movement;
- Enter/Space activation;
- Back/Escape;
- printable-key type-to-search where appropriate;
- shortcut/command discoverability;
- stable focus restoration;
- no focus traps;
- square/near-square responsive layouts;
- touch retained as secondary input.

## Device roles

Panther is REFERENCE_FROZEN. Titan 2 is active PORTABILITY/N0 research. Titan 2
Elite is an independent candidate. Q27 remains RESEARCH.

No new PRIMARY device is currently declared.

## Build/artifact boundary

K1/K2 is part of the architecture:

- artifact records support multiple artifact kinds;
- serial is not build/artifact identity;
- common deployment code owns safety/evidence;
- device adapters own transport/partition/restore semantics;
- unqualified devices fail closed.

## Security boundary

Common apps should prefer ordinary app permissions and supported Android APIs.
Privileged/system authority is narrow, explicit, device/product justified and
negative-tested.
