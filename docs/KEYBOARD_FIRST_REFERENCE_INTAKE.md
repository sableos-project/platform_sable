# Keyboard-first reference intake

Status: **design input — learnings only, not code import approval**

SableOS should learn from existing keyboard-first Android projects while keeping Sable-owned architecture, licensing boundaries and release claims.

This document records references that inform Sable keyboard-first design. It does not approve code import, branding reuse, release artifact reuse or inherited security/support claims.

## Reference matrix

| Reference | Primary lesson | Sable landing area | Code import posture |
| --- | --- | --- | --- |
| OpenMiniLaunch / MinkLauncher | keyboard-first launcher, command box, local conversations, provider handoff | Sable Start, Sable Hub, Sable Command | possible later after Apache-2.0 review; not required now |
| Commander | global command overlay, notification hub, action aliases, quick controls, keyboard navigation | Sable Command, Sable Hub, Settings search | possible later after MIT review; not required now |
| Pastiera / Plektra | physical-keyboard IME, layout JSON, modifier state, Nav Mode, SYM pages | Sable Keyboard/Profile model | no platform-core import; GPL/app-boundary review required |
| q25toolbox | per-app keyboard policy, auto-focus, display scaling, hardware workarounds | SableInputProfile, AppLayoutProfile, device adapters | no import until license/provenance clear; root/LSPosed model not Sable base |
| Titan 2 stock research | Bluetooth pairing text-entry failure and SubScreen/keyboard evidence | CriticalTextEntryProfile, Titan adapter | evidence-derived requirements only |

## OpenMiniLaunch / MinkLauncher learnings

Adopt as Sable-owned concepts:

```text
command box from home
local app/search/file/contact handoff
conversation aggregation through Android/provider surfaces
inline reply only when provider exposes RemoteInput
no delivery/read-status claim by the launcher
privacy-first transient handling
permission-specific onboarding
```

Do not copy wholesale launcher UI.

## Commander learnings

Adopt as Sable-owned concepts:

```text
global keyboard command overlay
notification hub with filter/dismiss/open/reply
recent-app switching through keyboard
settings/search aliases
quick controls
provider/app handoff boundaries
feature-specific permissions
```

Do not make Sable core dependent on Accessibility or third-party app hooks for platform behavior.

## Pastiera / Plektra learnings

Adopt as Sable-owned concepts:

```text
modifier state machine
one-shot/lock/held modifiers
Nav Mode
SYM pages
JSON-backed keyboard layout/profile model
user-visible layout import/export idea
physical-keyboard regression tests
```

GPL code must not be copied into platform/vendor core without a deliberate GPL-compatible app boundary and full compliance.

## q25toolbox learnings

Adopt as user-story/device-behavior inputs:

```text
per-app keyboard behavior
auto-focus first editable field
chat Enter-to-send
in-call keyboard shortcuts
per-app display scaling
proximity/keyboard recovery
recents layout interest
```

Reject as Sable base architecture:

```text
root app required
LSPosed required
bind-mounted keylayout as normal path
hidden Accessibility interception as primary OS strategy
```

SableOS controls the OS image and should implement equivalent behavior through profiles, device adapters, settings and system policy where appropriate.

## Intake process

Before any reference affects product behavior:

```text
REFERENCE_REVIEWED=YES
LICENSE_REVIEWED=YES
USER_STORY_EXTRACTED=YES
SABLE_CONTRACT_UPDATED=YES
CODE_IMPORT_REQUIRED=NO_BY_DEFAULT
ATTRIBUTION_REQUIRED=RECORDED_WHEN_USED
```

## Release boundary

A Sable release must not claim it includes, supports, forks or endorses any reference project unless that is explicitly true and documented.

```text
NO_REFERENCE_PROJECT_BRANDING_INHERITED=YES
NO_REFERENCE_PROJECT_SECURITY_CLAIM_INHERITED=YES
NO_REFERENCE_PROJECT_SUPPORT_CLAIM_INHERITED=YES
```
