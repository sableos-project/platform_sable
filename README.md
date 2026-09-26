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
- [Settings visual confirmation](docs/design/SETTINGS_VISUAL_CONFIRMATION.md)
- [Sable Start keyboard-first UX](docs/design/SABLE_START_KEYBOARD_FIRST_UX.md)
- [Sable Start screen design](docs/design/SABLE_START_SCREEN_DESIGN.md)
- [Sable Start three-page model](docs/design/SABLE_START_THREE_PAGE_MODEL.md)
- [Sable Start visual confirmation](docs/design/SABLE_START_VISUAL_CONFIRMATION.md)
- [Sable Start functional references](docs/design/SABLE_START_REFERENCES.md)
- [Sable Hub keyboard-first UX](docs/design/SABLE_HUB_KEYBOARD_FIRST_UX.md)
- [Sable Hub visual confirmation](docs/design/SABLE_HUB_VISUAL_CONFIRMATION.md)
- [All Apps privacy UX](docs/design/ALL_APPS_PRIVACY_UX.md)
- [All Apps visual confirmation](docs/design/ALL_APPS_VISUAL_CONFIRMATION.md)

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
START_BASE_QUICK_BAR_IN_SETTINGS=NO
SETTINGS_VISUAL_ARTIFACT_SOURCE_CONTROLLED=YES
TITAN2_HARDWARE_TEMPLATE=YES
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
THREE_PAGE_START_MODEL=YES
LEFT_PAGE_GLANCE_WIDGETS=YES
RIGHT_PAGE_HUB=YES
BASE_QUICK_BAR_CONFIGURABLE=YES
VISUAL_ARTIFACT_SOURCE_CONTROLLED=YES
TITAN2_HARDWARE_TEMPLATE=YES
FUNCTIONAL_REFERENCE_ONLY=YES
CODE_IMPORT=NO
```

Functional inspirations are documented as behavior references only: Android
Launcher/Pixel Launcher/Launcher3, Niagara-style vertical access, BlackBerry OS7
keyboard shortcuts, BlackBerry OS10 Hub/Peek/Flow, Windows Phone/Metro/Zune visual
structure, OpenMiniLaunch/Mink command-box ideas, Commander command overlay, and
keyboard/pointer learnings from Pastiera/Plektra and q25toolbox.

## Sable Hub UX posture

Sable Hub is the Sable communications and attention surface. It aggregates and
routes messages, calls, mail, notifications and people views while preserving
provider/source ownership.

```text
SABLE_HUB_SURFACE=YES
COMMUNICATIONS_FIRST=YES
PROVIDER_OWNERSHIP_RETAINED=YES
PRIORITY_FILTER_DEFAULT=YES
FILTER_BAR_VISIBLE=YES
PRIVACY_LOCKED_MODE=YES
INLINE_REPLY_PROVIDER_SAFE=YES
START_BASE_QUICK_BAR_IN_HUB=NO
HUB_VISUAL_ARTIFACT_SOURCE_CONTROLLED=YES
TITAN2_HARDWARE_TEMPLATE=YES
```

Hub must not invent unsupported provider actions. Inline reply is allowed only
when the source exposes a safe reply path.

## All Apps UX posture

All Apps is the trusted app discovery and customization surface. It preserves the
Android All Apps mental model while showing Sable privacy/security summaries
under each app name.

```text
ALL_APPS_SURFACE=YES
ANDROID_ALL_APPS_MENTAL_MODEL=KEEP
APP_NAMES_PRIMARY=YES
PACKAGE_NAMES_DEFAULT_VISIBLE=NO
PRIVACY_SUMMARY_UNDER_APP=YES
APP_ACTIONS_KEYBOARD_ACCESSIBLE=YES
PIN_TO_START=YES
ADD_TO_BASE_BAR=YES
ALL_APPS_VISUAL_ARTIFACT_SOURCE_CONTROLLED=YES
TITAN2_HARDWARE_TEMPLATE=YES
```

All Apps must not present raw package names or raw Android permission constants
as the default user-facing model. Developer details remain available through App
info / developer views.

## Visual artifact rule

Titan 2 UI validation artifacts must use Titan 2 proportions unless the artifact
is explicitly for a different device profile.

```text
TITAN2_TEMPLATE_FOR_VISUAL_VALIDATION=YES
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=YES
NO_SLAB_PHONE_VISUAL_TARGET=YES
NO_STRETCHED_SCREEN_TARGET=YES
```

This rule applies to Sable Start, Settings, Hub, All Apps and subsequent Titan 2
visual design artifacts.

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
- source-controlled Settings visual confirmation artifact;
- Start screen app privacy summaries and keyboard app actions;
- Sable Start three-page Center/Glance/Hub page model;
- source-controlled Sable Start visual confirmation artifact;
- source-controlled Sable Hub visual confirmation artifact;
- Sable Hub provider-safe row actions and privacy states;
- source-controlled All Apps visual confirmation artifact;
- All Apps privacy/security summaries under app names;
- All Apps app actions, pin-to-Start and add-to-base-bar flows;
- Titan 2 visual-template rule for square display plus physical keyboard;
- configurable base quick bar with required fallback paths;
- curated Sable glance widgets with Weather as first Home candidate;
- critical text-entry fallback for setup, pairing and lockscreen flows;
- gated diagnostics bridge for hardware/factory test surfaces;
- common Sable Keyboard/IME boundary;
- common Sable Camera/system-image boundary.

Common app semantics do not fork by device model. Hardware, display, attention,
keyboard and critical-input differences are expressed through profiles.
