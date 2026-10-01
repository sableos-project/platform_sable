# Sable Start keyboard-first UX

> **Integration note:** this document specifies the Sable-facing HOME product surface and interaction semantics. It does not require a standalone HOME APK. Current canonical integration hosts Sable Start presentation/state inside Launcher3/Launcher3QuickStep; see `../C3B_INTEGRATION_BOUNDARY.md`.

Status: **design contract — keyboard-first launcher/home**

This document defines the keyboard-first Sable Start / Launcher UX for Titan 2,
Titan 2 Elite, Zinwa Q27 and future keyboard-first devices.

Sable Start remains the Sable HOME surface. It must preserve the Android mental
model for home, apps, widgets/actions and app info, while making physical-keyboard
launch, command entry, focus, pointer mode and square/compact layouts first-class.

```text
SABLE_START_HOME=YES
ANDROID_HOME_MENTAL_MODEL=KEEP
KEYBOARD_FIRST=YES
TOUCH_SECONDARY=YES
VISIBLE_FOCUS_REQUIRED=YES
COMMON_APPS_NOT_FORKED_BY_DEVICE=YES
BASE_QUICK_BAR=REQUIRED
```

## Design goals

Sable Start should feel familiar to Android users and efficient on physical
keyboard devices.

Goals:

```text
one-keystroke app access
fast type-to-launch
persistent Phone access
persistent Hub access
persistent Command/Search access
persistent All Apps access
visible focus at all times
predictable Back/Home/Recents behavior
Android-compatible app drawer and app-info paths
Sable Command access from Home
glanceable Hub/attention entry point
safe pointer/touch fallback
square and compact layout support
```

Non-goals:

```text
no separate Titan-only launcher fork
no forced Pixel Launcher layout on square devices
no app-package names as primary labels
no hidden keyboard-only features without discoverability
no raw factory/diagnostic actions from Home
no communication actions hidden behind search-only flows
```

## Base quick bar

Sable Start requires a persistent base quick bar at the bottom of the Home
surface. This is the daily-driver access row for communication, command and app
inventory.

Default Titan 2 slots:

```text
Phone | Hub | Command | All Apps
```

Optional fifth slot where density allows:

```text
Phone | Hub | Command | Camera | All Apps
```

The quick bar borrows the familiar Android launcher dock concept, but its Sable
priority is different: fast call, fast Hub, fast command and fast app inventory.

```text
PHONE_QUICK_ACCESS=REQUIRED
HUB_QUICK_ACCESS=REQUIRED
COMMAND_QUICK_ACCESS=REQUIRED
ALL_APPS_QUICK_ACCESS=REQUIRED
CAMERA_QUICK_ACCESS=OPTIONAL_PROFILE_OR_USER_PIN
USER_CUSTOMIZATION=YES_WITH_SAFE_DEFAULTS
```

Keyboard shortcuts:

```text
Fn + 1  Phone
Fn + 2  Hub
Fn + 3  Command / Search
Fn + 4  All Apps
Fn + 5  optional Camera or user-selected quick slot
```

The quick bar must be keyboard-focusable, pointer-clickable and touchable. It may
collapse to compact icon labels on smaller displays, but it must remain obvious.

## Screen design — Titan 2 square profile

Titan 2 uses a square main display with physical keyboard below it. The default
Sable Start layout should be a dense vertical composition, not a phone-slab grid.

Recommended visual structure:

```text
top status/title area
command/search bar
Today/Hub glance strip
pinned app/action tiles or list
All Apps preview or list
persistent base quick bar
optional shortcut hint row
```

Default focus starts on the command/search field, first pinned item or base quick
bar depending on the user's chosen Home mode. The focused element must be
visually obvious on LCD square displays.

```text
TITAN2_DEFAULT_LAYOUT=square_keyboard_start
TITAN2_PRIMARY_COLUMN=vertical
TITAN2_APP_GRID=SECONDARY_OPTION
TITAN2_FOCUS_RING=REQUIRED
TITAN2_BASE_QUICK_BAR=REQUIRED
```

## Screen design — compact AMOLED keyboard profile

Titan 2 Elite and Q27-class devices are compact portrait keyboard devices. They
should use the same Sable Start semantics but may use a taller list-first layout.

```text
COMPACT_KEYBOARD_DEFAULT_LAYOUT=list_plus_favorites
COMPACT_BASE_QUICK_BAR=REQUIRED
AMOLED_ATTENTION=PROFILE_DEPENDENT
AOD_ENTRY_POINTS=ONLY_IF_PROFILE_VALIDATED
```

## Home modes

Sable Start should support two presentation modes from the same semantic model:

```text
Keyboard-first list mode
  default for Titan 2, Titan 2 Elite and Q27
  optimized for type-to-launch and arrows/D-pad

Touch tile mode
  optional profile or user preference
  preserves Sable tile/card visual language
```

A device profile may choose the default mode, but the common app/launcher model
must remain shared.

## Type-to-launch

From the default Home screen, printable characters should immediately begin a
launch query unless a text field is already active.

```text
IF no text field is focused:
  printable key opens command/search field
  key becomes first query character

IF command/search field focused:
  printable key edits query

IF pointer mouse mode active:
  printable key obeys mouse-mode mapping until mouse mode exits
```

Examples:

```text
M
  query = m
  focus first matching item, e.g. Mail or Media

M E
  query = me
  focus Messages or Media depending ranking/user history

S E
  query = se
  focus Settings

H
  query = h
  focus Hub
```

Type-to-launch differs from Settings type-ahead focus navigation:

```text
Settings:
  letters move focus among visible rows

Sable Start:
  letters enter command/search and filter apps/actions/contacts/settings
```

## Command/search model

The Sable Start command/search field is a single entry point for local device
intent. It should remain local-first and provider-safe.

Supported classes:

```text
apps
app shortcuts
settings destinations
contacts / people actions
Hub actions
recent tasks
files where indexed locally
local notes/tasks where supported
web/browser search only by explicit action
```

Suggested prefixes:

```text
?app       app and shortcut search
@person    message/contact action
#person    call action
/settings  Settings destination search
/hub       Hub filter/action
!action    command/action alias
```

Prefixes are shortcuts, not required syntax. Normal plain-language matching must
also work.

## Pinned items

Pinned items may be apps or actions.

```text
Pinned app
Pinned contact/person action
Pinned setting
Pinned diagnostic shortcut only if safe/read-only
Pinned Hub filter
```

Unsafe diagnostics, factory-test launchers, calibration tools and write-capable
engineering actions must not be pinned by default.

## All Apps

All Apps must be keyboard-friendly and privacy-forward.

```text
primary label = app name
secondary label = permission/security summary where space allows
package name = details/debug only
```

Example row:

```text
Camera
Camera · Microphone · Location
```

Rows support:

```text
Enter     launch
Space     preview / expand actions where supported
Long press / Menu / Fn+Enter     app actions
Alt+Enter     App info
Back      close drawer/search or return Home
```

## App actions

App actions must preserve Android expectations while adding keyboard access.

```text
Open
App info
Permissions
Uninstall / disable where allowed
Add to pinned
Add to quick bar where allowed
Remove from pinned
Widget / shortcut where supported
Privacy summary
```

The app-actions menu must be reachable by touch, keyboard and pointer.

## Phone quick access

The Phone quick-bar slot exists because a keyboard-first phone must still be a
fast calling device.

```text
Enter on Phone
  open dialer or recent calls according to user preference

digits while Phone focused
  open dialer and enter digits

Menu / long press / Fn+Enter
  Dial pad, Recent calls, Contacts, Emergency information where allowed
```

Launcher may route the user to dialer/contacts; it must not own telephony state.

## Hub quick access

Sable Start should expose Hub as a primary surface without making Home a
notification database owner.

```text
Enter on Hub
  open Sable Hub priority view

Space on Hub
  expand safe glance/peek if privacy policy allows

Menu / long press / Fn+Enter
  choose Hub filter: Priority, Messages, Mail, People, Missed calls
```

Provider ownership remains outside the launcher. Start may link to source app
or Hub action; it must not silently own provider credentials/history.

## Keyboard interactions

Baseline:

```text
Up / Down
  move vertical focus

Left / Right
  move inside tile rows, quick bar or panes where present

Enter
  launch / open / activate

Space
  preview / expand / toggle safe focused control

Back / Esc
  close search, close actions, or return to previous layer

Home
  return to Sable Start top layer

Square / Recents
  open task switcher / Overview

Fn + Up / Fn + Down
  page up / page down

Fn + Left / Fn + Right
  previous / next section

Fn + 1..5
  activate base quick bar slots

Alt + Enter
  app info / details for focused app

Sym
  open symbol entry for command/search when text field focused

Long Backspace
  clear query
```

No keyboard action may trap the user in Start, search, app actions, quick bar or
the app drawer.

## Mouse / pointer behavior

Pointer mode is explicit and must not conflict with type-to-launch.

```text
pointer movement:
  shows hover state
  does not steal keyboard focus until click/activation

pointer click:
  focuses and activates or selects the clicked item

keyboard after pointer:
  returns to keyboard focus mode from last selected item

keyboard-driven mouse mode:
  type-to-launch disabled until mouse mode exits
```

The user must always be able to exit pointer mode from hardware keys.

```text
EXIT_MOUSE_MODE=Back | Esc | explicit pointer-mode toggle
```

## Accessibility and discoverability

Sable Start must include shortcut help.

```text
Fn+? or Help
  opens shortcut cheat sheet

Long press focused item
  show available actions

First-run tip
  explains type-to-launch, base quick bar shortcuts, command prefixes and app actions
```

Every core action must have touch and keyboard paths.

## Visual language

Sable Start uses common Sable design language:

```text
dark-first but theme-following
large readable typography on small displays
Metro/Zune-influenced blocks and typography
strong focus ring
clear section headers
simple icons with semantic color
compact information density
persistent bottom communication/command bar
```

Visual design must not rely on OLED-only behavior because Titan 2 is LCD-class
and AOD is disabled by default on that profile.

## Acceptance

```text
SABLE_START_KEYBOARD_FIRST_UX=PASS
ANDROID_HOME_MENTAL_MODEL=PASS
TITAN2_SQUARE_LAYOUT=PASS
COMPACT_KEYBOARD_LAYOUT=PASS
BASE_QUICK_BAR=PASS
PHONE_QUICK_ACCESS=PASS
HUB_QUICK_ACCESS=PASS
COMMAND_QUICK_ACCESS=PASS
ALL_APPS_QUICK_ACCESS=PASS
TYPE_TO_LAUNCH=PASS
COMMAND_SEARCH_ENTRY=PASS
ALL_APPS_PRIVACY_SUMMARY=PASS
APP_ACTIONS_KEYBOARD_ACCESSIBLE=PASS
HUB_GLANCE_PROVIDER_SAFE=PASS
MOUSE_POINTER_INTERACTION=PASS
NO_FOCUS_TRAPS=PASS
NO_DEVICE_CODE_CHANGED=PASS
NO_BUILD_CODE_CHANGED=PASS
FLASH_ENABLEMENT=NO
```
