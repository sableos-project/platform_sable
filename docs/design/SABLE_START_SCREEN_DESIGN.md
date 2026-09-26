# Sable Start screen design

Status: **screen design contract — launcher/home**

This document records concrete Sable Start screen layouts for keyboard-first
profiles. It complements `SABLE_START_KEYBOARD_FIRST_UX.md` and is visually bound
by `SABLE_START_VISUAL_CONFIRMATION.md`.

## Source-controlled visual artifact

The visual target for the Titan 2 square three-page Start model is recorded in:

```text
docs/design/SABLE_START_VISUAL_CONFIRMATION.md
docs/design/artifacts/sable-start-titan2-3page.svg
```

Future implementation validation should compare delivered UI against that visual
artifact and this screen design contract.

## Shared components

```text
Status/title area
Command/search field
Today or Hub glance
Pinned apps/actions
All Apps list
Persistent base quick bar
Focus ring
Shortcut hint row where space allows
```

## Base quick bar

Sable Start must provide a persistent bottom quick bar on keyboard-first devices.
This solves the daily-driver problem where calling, Hub access and command entry
must not depend on scrolling, search or an app drawer.

Required default slots for Titan 2:

```text
[Phone] [Hub] [Command] [All Apps]
```

Optional five-slot layout where density and readability allow:

```text
[Phone] [Hub] [Command] [Camera] [All Apps]
```

Rules:

```text
BASE_QUICK_BAR=REQUIRED
PHONE_QUICK_ACCESS=REQUIRED
HUB_QUICK_ACCESS=REQUIRED
COMMAND_QUICK_ACCESS=REQUIRED
ALL_APPS_QUICK_ACCESS=REQUIRED
CAMERA_QUICK_ACCESS=OPTIONAL_PROFILE_OR_USER_PIN
USER_CUSTOMIZATION=YES_WITH_SAFE_DEFAULTS
```

The base quick bar is inspired by the Android launcher dock mental model, but its
content priority is Sable-specific: communication first, command/search always
available and app inventory always reachable.

Keyboard behavior:

```text
Fn + 1  Phone
Fn + 2  Hub
Fn + 3  Command / Search
Fn + 4  All Apps
Fn + 5  optional Camera or user-selected quick slot

Left / Right when quick bar focused
  move between quick bar slots

Enter
  activate selected quick action

Long press / Menu / Fn+Enter
  quick-slot options where customization is allowed
```

The quick bar must remain visible on the Home screen and should remain available
from the app drawer/search layers where space allows. It may collapse to labeled
icons in tight modes, but it must not become undiscoverable.

## Titan 2 square screen — default with base quick bar

Titan 2 should default to a square-optimized vertical layout with persistent
communication and command access at the base.

```text
┌────────────────────────────────────────┐
│ 10:46        Weather              99%  │
├────────────────────────────────────────┤
│  72°  Partly cloudy · H 75 / L 61      │
├────────────────────────────────────────┤
│  🔎 Search or command                  │
│     type app, action, person, setting  │
├────────────────────────────────────────┤
│  Home apps                             │
│  ▸ Messages                            │
│    Mail                                │
│    Calendar                            │
│    Contacts                            │
│    Phone                               │
│    Camera                              │
├────────────────────────────────────────┤
│  ☎ Phone   ◇ Hub   🔎 Command   ▦ Apps │
└────────────────────────────────────────┘
```

Design notes:

```text
Phone and Hub are always one focus step or shortcut away.
Command/search remains visually central and has a base shortcut.
All Apps is always visible at the base.
Pinned/home apps are user-orderable.
The focused row/tile/icon uses a strong Sable focus ring.
Text labels are preferred over icon-only affordances on Titan 2.
The top content is useful glance information, not SableOS logo/branding.
```

## Titan 2 square alternate: list-first minimal with base quick bar

For users who prefer faster keyboard use and less visual density:

```text
┌────────────────────────────────────────┐
│ Weather                           99%  │
├────────────────────────────────────────┤
│ > Search or command                    │
├────────────────────────────────────────┤
│ Messages   Compose / unread            │
│ Mail       Inbox                        │
│ Browser    Search or open URL          │
│ Camera     Open camera                 │
│ Media      Now playing / library       │
│ Settings   Device controls             │
├────────────────────────────────────────┤
│ A B C D E F G H I J K L M …            │
├────────────────────────────────────────┤
│ ☎ Phone  ◇ Hub  🔎 Command  ▦ Apps     │
└────────────────────────────────────────┘
```

This mode is closest to a keyboard productivity launcher. It should remain an
option if the tile mode feels too busy on the square display.

## Compact AMOLED keyboard-device screen

Titan 2 Elite / Q27 class can use a taller compact layout. The base quick bar is
still required, but it may use compact labels or icons depending on density.

```text
┌──────────────────────────────┐
│ Weather                  99% │
├──────────────────────────────┤
│ 🔎 Search or command         │
├──────────────────────────────┤
│ Home apps                    │
│ Messages                     │
│ Mail                         │
│ Browser                      │
│ Camera                       │
│ Settings                     │
├──────────────────────────────┤
│ All Apps                     │
│ A                            │
│ Apps                         │
│ B                            │
│ Browser                      │
│ C                            │
│ Calculator                   │
├──────────────────────────────┤
│ ☎   ◇   🔎   ▦              │
└──────────────────────────────┘
```

## Command/search expanded state

```text
┌────────────────────────────────────────┐
│ Search or command                      │
│ me                                     │
├────────────────────────────────────────┤
│ Apps                                   │
│ ▸ Media                                │
│   Messages                             │
├────────────────────────────────────────┤
│ People                                 │
│   Meera — message                      │
├────────────────────────────────────────┤
│ Settings                               │
│   Media controls                       │
├────────────────────────────────────────┤
│ Actions                                │
│   Message someone                      │
├────────────────────────────────────────┤
│ ☎ Phone  ◇ Hub  🔎 Command  ▦ Apps     │
└────────────────────────────────────────┘
```

Rules:

```text
Enter opens focused result.
Down/Up move result focus.
Right opens action/details where supported.
Back exits search and restores Home focus.
Long Backspace clears query.
Sym opens symbol panel when text input needs symbols.
Alt/Fn character entry must work.
Base quick bar remains reachable where space allows.
```

## Hub quick access behavior

The Hub base icon is not just a shortcut to an app icon. It is a first-class
communication entry point when present in the base quick bar, but it is still
optional because the right Start page is Hub.

```text
Enter on Hub quick icon
  open Sable Hub priority view

Space on Hub quick icon
  show safe glance/peek if privacy policy allows

Long press / Menu on Hub quick icon
  choose Hub filter: Priority, Messages, Mail, People, Missed calls
```

Privacy rule:

```text
LOCKED_OR_PRIVATE_MODE
  Hub quick icon may show count only
  no message text preview unless user policy allows
```

## Phone quick access behavior

```text
Enter on Phone quick icon
  open dialer / recent calls according to user preference

Type digits while Phone quick icon focused
  open dialer with those digits

Long press / Menu on Phone quick icon
  actions: Dial pad, Recent calls, Contacts, Emergency information where allowed
```

Sable Start should support a fast call path without making phone/telephony state
owned by the launcher.

## App actions surface

```text
┌────────────────────────────────────────┐
│ Settings                               │
├────────────────────────────────────────┤
│ Open                                   │
│ App info                               │
│ Permissions                            │
│ Add to pinned                          │
│ Add to quick bar where allowed         │
│ Privacy summary                        │
│ Widgets / shortcuts                    │
└────────────────────────────────────────┘
```

Keyboard:

```text
Enter      activate focused action
Back       close menu
Alt+Enter  open App info directly from app row
```

## Privacy summary in All Apps

Where space allows:

```text
Camera
Camera · Microphone · Location

Messages
Contacts · Notifications · SMS

Browser
Network · Notifications
```

On very tight displays, show one-line summary and move details to App info.

## Visual confirmation requirement

Before implementation, the design should be reviewed on at least a Titan 2 square
mockup and a compact keyboard-device mockup.

```text
SABLE_START_VISUAL_ARTIFACT_PRESENT=PASS
TITAN2_SQUARE_VISUAL_REVIEW=REQUIRED_BEFORE_IMPLEMENTATION
COMPACT_KEYBOARD_VISUAL_REVIEW=REQUIRED_BEFORE_IMPLEMENTATION
BASE_QUICK_BAR_VISUAL_REVIEW=REQUIRED_BEFORE_IMPLEMENTATION
MAC_DESIGN_BUILD_REQUIRED=NO
```

The visual review may be image/mockup based and does not require a separate macOS
design APK like earlier Pixel launcher work.
