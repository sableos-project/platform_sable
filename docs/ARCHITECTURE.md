# SableOS common architecture

Status: **current normative architecture — 2026-10-02**

## Product structure

SableOS uses one common product core with multiple interaction and hardware
profiles plus bounded device adapters.

```text
common product semantics
    launcher / hub / apps / appearance / privacy contracts
        |
        +-- touch-first presentation
        |     -> Panther reference profile
        |
        +-- keyboard-first profile family
              -> Titan 2 profile
              -> Titan 2 Elite profile
              -> Q27 profile
        |
        v
device adapter
    display/input/camera/boot/vendor/telephony specifics
        |
        v
Android framework + vendor BSP + hardware
```

Common app semantics do not fork by device model. Hardware, display, attention,
keyboard, pointer and critical-input differences are expressed through profiles
and device adapters.

## Active architecture contracts

The current keyboard-device architecture is profile-first:

```text
SableHardwareProfile
SableDisplayProfile
SablePanelPowerProfile
SableAttentionSurfaceProfile
SableKeyboardProfile
SablePointerSurfaceProfile
SableCriticalTextEntryProfile
SableAppLayoutProfile
```

Normative profile docs:

- [Keyboard-first device profile model](KEYBOARD_FIRST_DEVICE_PROFILE_MODEL.md)
- [Display and attention profile model](DISPLAY_AND_ATTENTION_PROFILE_MODEL.md)
- [Keyboard and pointer profile model](KEYBOARD_AND_POINTER_PROFILE_MODEL.md)
- [Critical text-entry gates](CRITICAL_TEXT_ENTRY_GATES.md)
- [Keyboard-first reference intake](KEYBOARD_FIRST_REFERENCE_INTAKE.md)
- [Device capability matrix](device-capabilities/DEVICE_CAPABILITY_MATRIX.md)

## Launcher / HOME

Sable Start owns the Sable-facing HOME **product semantics and presentation**:
Start / All Apps / Search / Peek / app-context / keyboard-first navigation.

Runtime hosting is an integration decision, not a public design-module
requirement. The current canonical implementation after the R9L8 cutover hosts
Sable Start presentation/state source inside Launcher3/Launcher3QuickStep, which
is the Android HOME runtime and Recents/Overview/task/gesture substrate.

```text
SABLE_START_PRODUCT_SURFACE=YES
STANDALONE_SABLELAUNCHER_RUNTIME_REQUIRED=NO
CURRENT_CANONICAL_HOME_RUNTIME=Launcher3QuickStep
HOME_AUTHORITY_SCOPE=SABLE_FIRST_PARTY_CANONICAL
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO
```

See `C3B_INTEGRATION_BOUNDARY.md`. Launcher3/Quickstep is the Sable-owned canonical
HOME runtime; Android user selection of a third-party HOME remains supported.
Third-party launchers do not automatically inherit Quickstep/SystemUI or Private
Space integration.

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


## Battery usage, health and charging

Battery remains an Android Settings capability, not a separate Sable launcher app.

```text
Settings -> Battery
  overview
  usage
  health
  charging & protection
```

The common product contract is availability-aware and profile-gated:

- usage relies on Android platform accounting rather than a parallel Sable profiler;
- health values such as maximum capacity and cycle count are shown only when a
  trustworthy framework/Health/vendor source exists;
- derived values must identify that they are estimates and expose confidence;
- unsupported values render as unavailable rather than fabricated;
- mutable charge-limit/adaptive-charging controls require a validated
  device-specific backend;
- common Settings UI does not directly write raw vendor sysfs/procfs nodes;
- battery analytics/history remain local and bounded.

The frozen historical S1.7 `PowerSnapshot` contract remains narrow and is not
silently expanded for this feature.

Keyboard-first Battery follows the normalized input and Settings focus contracts:
deterministic focus, Up/Down traversal, Enter/Space activation, Settings
type-ahead, and an explicit keyboard inspection mode for charts.

Normative design:

- [Battery usage, health and charging UX](design/BATTERY_HEALTH_USAGE_UX.md)
- [Battery visual confirmation](design/BATTERY_HEALTH_USAGE_VISUAL_CONFIRMATION.md)

## Application boundaries

Applications own capabilities; the Sable shell organizes people, attention and
actions.

Common app source must not fork merely because a target has a physical keyboard,
different SoC, square display, AMOLED display, secondary display, trackpad,
mouse mode or vendor BSP.

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

Sable Camera is a common system-image workstream. Keyboard-first devices use the
accepted **Camera Control Deck** interaction model:

> The screen is the viewfinder. The physical keyboard is the camera control
> surface.

Architecture:

```text
camera-core
camera-capabilities
semantic camera actions
device-profiles
ui / Camera Control Deck
platform-integration
```

Portrait remains supported. Landscape emphasizes the viewfinder and physical
keyboard, with transient parameter strips, keyboard focus-point movement,
AF/AE lock candidates and a discoverable control legend. Touch remains a
complete fallback and the app does not globally force landscape.

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
    touch surface / capacitive keyboard / mouse mode
```

Device scan-code quirks do not belong in common IME or app code. Titan 2,
Titan 2 Elite and Q27 require independent keyboard and pointer profiles.

Sable Keyboard is the first-party/factory-default direction, subject to
canonical integration, but Android user choice is preserved:

```text
THIRD_PARTY_IME_INSTALL_ALLOWED=YES
THIRD_PARTY_IME_ENABLE_ALLOWED=YES
THIRD_PARTY_IME_SELECTION_ALLOWED=YES
FORCE_SABLE_IME_AFTER_USER_SELECTION=NO
```

Third-party IMEs are not automatically covered by Sable's direct-boot,
lockscreen or physical-keyboard acceptance evidence.

## Critical text entry

Setup, pairing, lockscreen and recovery-critical dialogs must always have a
working text-entry path.

```text
Bluetooth pairing
Wi-Fi password entry
Setup Wizard
Lockscreen / PIN / password
Emergency text fields
Account sign-in
Recovery / restore prompts
```

A physical keyboard profile is not sufficient unless software-keyboard fallback,
Alt/SYM/numeric/symbol entry and critical-dialog entry are independently proven.

## Display and attention

Display shape and panel technology are first-class architecture inputs.

```text
SQUARE_KEYBOARD
COMPACT_TALL_KEYBOARD
SECONDARY_GLANCE_DISPLAY
TOUCH_REFERENCE_SLAB
```

AOD is a panel/power/device capability, not a product-wide feature flag.

```text
Titan 2       AOD_DEFAULT=NO / rear SubScreen candidate
Titan 2 Elite AOD=CANDIDATE_REQUIRES_VALIDATION
Q27           AOD=CANDIDATE_REQUIRES_VALIDATION
```

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
- display-profile-aware square/compact/touch layouts;
- touch retained as secondary input;
- pointer/touch-surface behavior when device hardware supports it.

## Device roles

Panther is REFERENCE_FROZEN. Titan 2 is active N1D/C3B engineering integration.
Titan 2 Elite remains an independent keyboard-first candidate requiring separate evidence. Q27 remains RESEARCH.

No new PRIMARY device is currently declared.

## Build/artifact boundary

K1/K2 is part of the architecture:

- artifact records support multiple artifact kinds;
- serial is not build/artifact identity;
- common deployment code owns safety/evidence;
- device adapters own transport/partition/restore semantics;
- unqualified devices fail closed.

A device-capability matrix is not build, flash or release authorization.

## Security boundary

Common apps should prefer ordinary app permissions and supported Android APIs.
Privileged/system authority is narrow, explicit, device/product justified and
negative-tested.
