# Sable Start screen design

Status: **screen design contract — launcher/home**

This document records concrete Sable Start screen layouts for keyboard-first
profiles. It complements `SABLE_START_KEYBOARD_FIRST_UX.md`.

## Shared components

```text
Status/title area
Command/search field
Today or Hub glance
Pinned apps/actions
All Apps list
Focus ring
Shortcut hint row where space allows
```

## Titan 2 square screen

Titan 2 should default to a square-optimized vertical layout.

```text
┌────────────────────────────────────────┐
│ 10:46        Sable Start          99%  │
├────────────────────────────────────────┤
│  🔎 Search or command                  │
│     type app, action, person, setting  │
├────────────────────────────────────────┤
│  Today                                 │
│  Hub: 2 priority · Weather · Battery   │
├────────────────────────────────────────┤
│  Pinned                                │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  │
│  │ Hub  │ │Phone │ │ Mail │ │Camera│  │
│  └──────┘ └──────┘ └──────┘ └──────┘  │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐  │
│  │Media │ │Readr │ │Calc  │ │Setngs│  │
│  └──────┘ └──────┘ └──────┘ └──────┘  │
├────────────────────────────────────────┤
│  All apps                              │
│  ▸ Calculator                          │
│    Calendar                            │
│    Camera                              │
│    Hub                                 │
│    Mail                                │
│    Media                               │
│    Settings                            │
└────────────────────────────────────────┘
```

Design notes:

```text
The command field must be reachable with one action.
Pinned tiles stay compact and predictable.
All Apps remains visible without requiring a separate gesture.
The focused row/tile uses a strong Sable focus ring.
Text labels are preferred over icon-only affordances.
```

## Titan 2 square alternate: list-first minimal

For users who prefer faster keyboard use and less visual density:

```text
┌────────────────────────────────────────┐
│ Sable Start                       99%  │
├────────────────────────────────────────┤
│ > Search or command                    │
├────────────────────────────────────────┤
│ Hub        2 priority · 5 messages     │
│ Phone      Call or search contacts     │
│ Messages   Compose / unread            │
│ Mail       Inbox                        │
│ Browser    Search or open URL          │
│ Camera     Open camera                 │
│ Media      Now playing / library       │
│ Settings   Device controls             │
├────────────────────────────────────────┤
│ A B C D E F G H I J K L M …            │
└────────────────────────────────────────┘
```

This mode is closest to a keyboard productivity launcher. It should remain an
option if the tile mode feels too busy on the square display.

## Compact AMOLED keyboard-device screen

Titan 2 Elite / Q27 class can use a taller compact layout.

```text
┌──────────────────────────────┐
│ Sable Start              99% │
├──────────────────────────────┤
│ 🔎 Search or command         │
├──────────────────────────────┤
│ Hub                          │
│ 2 priority · 5 messages      │
├──────────────────────────────┤
│ Favorites                    │
│ Hub      Phone    Mail       │
│ Browser  Camera   Settings   │
├──────────────────────────────┤
│ All Apps                     │
│ A                            │
│ Apps                         │
│ B                            │
│ Browser                      │
│ C                            │
│ Calculator                   │
│ Calendar                     │
│ Camera                       │
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
```

## App actions surface

```text
┌────────────────────────────────────────┐
│ Settings                               │
├────────────────────────────────────────┤
│ Open                                   │
│ App info                               │
│ Permissions                            │
│ Add to pinned                          │
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
TITAN2_SQUARE_VISUAL_REVIEW=REQUIRED_BEFORE_IMPLEMENTATION
COMPACT_KEYBOARD_VISUAL_REVIEW=REQUIRED_BEFORE_IMPLEMENTATION
MAC_DESIGN_BUILD_REQUIRED=NO
```

The visual review may be image/mockup based and does not require a separate macOS
design APK like earlier Pixel launcher work.
