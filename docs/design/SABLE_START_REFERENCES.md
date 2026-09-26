# Sable Start functional references

Status: **design reference — no code import**

This document records launcher and OS ideas that informed the Sable Start
keyboard-first UX. These are functional inspirations, not authority to copy UI,
branding, assets or source code.

```text
FUNCTIONAL_REFERENCE_ONLY=YES
CODE_IMPORT=NO
BRAND_CLONE=NO
SABLE_OWNED_DESIGN=YES
```

## Android Launcher / Pixel Launcher / Launcher3

Use as the baseline mental model for Android compatibility:

```text
Home button returns Home
Recents/Overview remains recognizable
All Apps / app drawer exists
long-press app actions exist
App info and permissions remain reachable
widgets/shortcuts remain Android-compatible concepts
```

Sable Start may depend on Android launcher/task primitives, but the user-facing
Home experience is Sable-owned.

## Niagara Launcher

Functional inspiration:

```text
fast vertical app access
low-friction one-handed navigation
minimal home surface
letter/list-driven app discovery
quick access without a dense icon grid
```

Sable adaptation:

```text
vertical All Apps list for keyboard-first devices
large readable app rows
fast type-to-launch
section jumps and focused app actions
```

Sable does not adopt Niagara branding, exact UI or product model.

## BlackBerry OS7

Functional inspiration:

```text
physical-keyboard confidence
message-first productivity
shortcut-driven app launch
list-first navigation
hardware keys as primary controls
```

Sable adaptation:

```text
visible focus everywhere
keyboard shortcuts for Home, Hub, Search, app actions
predictable Back/Home/Recents behavior
launcher usable without touch
```

## BlackBerry OS10

Functional inspiration:

```text
Hub-centric communication flow
Peek / glance without losing context
Flow between app, Hub and reply
active-card/task awareness
```

Sable adaptation:

```text
Hub glance on Start
Sable Hub as the communications surface
future Peek/Command integration
recent tasks exposed through Android Recents/Overview semantics
```

Sable does not clone BB10 gestures or replace Android navigation rules.

## Windows Phone / Metro / Zune

Functional inspiration:

```text
strong typography
simple geometric layout
cards/tiles for glanceable information
high contrast
content-first visual hierarchy
```

Sable adaptation:

```text
section headers
pinned app/action tiles
Today/Hub glance cards
Sable design tokens and accent color
Zune-like Media direction retained separately
```

## OpenMiniLaunch / Mink

Functional inspiration:

```text
keyboard-first launcher thinking
command/magic box concept
local-first app/action/contact entry
notification/conversation awareness through provider handoff
```

Sable adaptation:

```text
Sable Command field on Start
local-first app/action/settings/contact lookup
Hub/provider handoff model instead of provider database ownership
```

Any code reuse requires separate license/provenance review.

## Commander

Functional inspiration:

```text
global command bar
notification hub
quick controls
action aliases
keyboard-first overlay interaction
```

Sable adaptation:

```text
Sable Command model
launcher-accessible action aliases
future global command overlay
notification/Hub handoff rather than raw ownership
```

Any code reuse requires separate license/provenance review.

## Pastiera / Plektra

Functional inspiration:

```text
physical-keyboard typing ergonomics
modifier states
Sym/Alt/Fn behavior
layout portability
```

Sable adaptation:

```text
Start command/search must support Alt/Sym/Fn text entry
software keyboard fallback must remain reachable
keyboard profile stays device-specific
```

Pastiera-style IME details belong in the Sable Keyboard profile, not directly in
launcher UI code.

## q25toolbox

Functional inspiration:

```text
per-app keyboard behavior
pointer/mouse mode realities on keyboard phones
hardware workaround awareness
```

Sable adaptation:

```text
explicit pointer mode boundary
keyboard-driven mouse mode does not conflict with type-to-launch
per-app input policy belongs to profiles/settings, not hardcoded launcher paths
```

## Reference boundary

```text
REFERENCE_USED_FOR_BEHAVIOR=YES
REFERENCE_USED_FOR_CODE=NO
REFERENCE_USED_FOR_BRANDING=NO
REFERENCE_USED_FOR_RELEASE_CLAIM=NO
```

Sable Start is a Sable-owned launcher design. It can learn from existing launchers
and keyboard OS history while remaining Android-compatible, privacy-forward and
profile-driven.
