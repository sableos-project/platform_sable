# Keyboard-first reference intake

Status: **design input — learnings only, not code import approval**

SableOS should learn from existing keyboard-first Android projects while keeping Sable-owned architecture, licensing boundaries and release claims.

This document records references that inform Sable keyboard-first design. It does not approve code import, branding reuse, release artifact reuse or inherited security/support claims.

## Reference matrix

| Reference | Primary lesson | Sable landing area | Code import posture |
| --- | --- | --- | --- |
| OpenMiniLaunch / MinkLauncher | keyboard-first launcher, command box, local conversations, provider handoff | Sable Start, Sable Hub, Sable Command | possible later after Apache-2.0 review; not required now |
| Commander | global command overlay, notification hub, action aliases, quick controls, keyboard navigation | Sable Command, Sable Hub, Settings search | possible later after MIT review; not required now |
| Pastiera 0.86 / Plektra | physical-keyboard IME, modifiers, compact input, SYM/emoji/snippets, dictionaries, settings/deep links | Sable Keyboard/Profile model + D6 backlog | behavior/product reference only; GPL source import not authorized |
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

## Pastiera 0.86 / Plektra learnings

Reviewed checkpoint:

```text
repository=https://github.com/palsoftware/pastiera
release=v0.86
commit=e7d8f27ecc7253e61690b5d34f110b25dc68bb16
release_date=2026-09-26
license=GPL-3.0
successor=https://github.com/pkb-rocks/plektra
```

Pastiera 0.86 is the final planned Pastiera feature release. Security maintenance
continues there while active feature development moves to Plektra.

Adopt as Sable-owned concepts:

```text
modifier state machine
one-shot/lock/held modifiers
Nav Mode
compact candidate/modifier/language strip
configurable SYM and emoji surfaces
local snippets / shortcodes
JSON-backed keyboard layout/profile model
user-visible layout import/export idea
multiple local dictionaries
optional local learned next-word behavior after privacy review
searchable settings and stable deep links
Titan 2 Elite rounded-display/inset test cases
physical-keyboard regression tests
```

Ownership remains explicit:

```text
QuickLauncher/app launch -> Launcher3-hosted Sable Start
global/power shortcuts -> platform/Sable Start semantics
IME -> text composition and bounded input UI
```

Do not adopt Shizuku as an input dependency. Do not enable persistent keyboard
clipboard history by default. Do not add an automatic unverified remote
dictionary/layout channel.

GPL Pastiera/Plektra source must not be copied into Sable Keyboard or
platform/vendor core without a separate explicit licensing decision and full
compliance. This intake authorizes behavior/product learning only.

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
