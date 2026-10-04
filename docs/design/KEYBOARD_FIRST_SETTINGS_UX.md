# Keyboard-first Settings UX

Status: **design contract — keyboard-first devices**

This document defines the SableOS Settings UX direction for Titan 2, Titan 2
Elite, Zinwa Q27 and future keyboard-first devices.

The goal is not to invent a new Settings taxonomy. SableOS should preserve the
mental model users already know from Android Settings while adding Sable-owned
keyboard, display, attention, privacy and diagnostics surfaces where the stock
experience is insufficient.

```text
ANDROID_SETTINGS_STRUCTURE=PRIMARY
SABLE_KEYBOARD_FIRST_ADDITIONS=PROFILE_AWARE_EXTENSIONS
SEARCH_REQUIRED=YES
VISIBLE_FOCUS_REQUIRED=YES
TOUCH_SECONDARY=YES
TYPE_AHEAD_FOCUS_NAVIGATION=YES
```

## Core rule

Do not make users relearn where ordinary Android settings live.

Sable Settings should keep familiar Android top-level destinations and add
keyboard-first device controls as bounded sections inside the most predictable
places.

```text
Network & internet
Connected devices
Apps
Notifications
Battery
Display
Sound & vibration
Storage
Privacy
Security & emergency
Location
Safety / wellbeing where present
System
About phone
```

Sable-specific device controls should appear as extensions, not replacements.

## Keyboard-first information architecture

Recommended top-level shape:

```text
Search settings

Network & internet
Connected devices
Apps
Notifications
Battery
Display
Sound & vibration
Keyboard & input
Attention & SubScreen
Privacy
Security & emergency
Location
System
Diagnostics
About phone
```

Where Android already has a standard section, keep the Android name. Where a
Titan-class device needs a new capability owner, add a Sable section with a clear
name.

## Keyboard & input

This is the primary control center for physical-keyboard devices.

```text
Keyboard & input
  Physical keyboard
    layout
    modifier behavior
    Alt / Sym / Fn
    key repeat
    language switching
    shortcut help
    key test

  Pointer / touch surface
    touchpad/touch surface mode
    mouse mode
    pointer speed
    scroll direction
    tap/click behavior
    app-specific pointer behavior

  Sable Keyboard
    software keyboard availability
    symbols and emoji
    one-handed / compact behavior where applicable
    critical-entry fallback test

  Keyboard shortcuts
    global shortcuts
    app shortcuts
    Hub shortcuts
    Command shortcuts
    shortcut conflicts
```

Titan 2, Titan 2 Elite and Q27 must not share a hardcoded profile. Settings must
show the active profile and provide a clear test path.

```text
ACTIVE_KEYBOARD_PROFILE=titan2 | titan2_elite | q27 | unknown
PROFILE_PASS_INHERITANCE=NO
```

## Display

Display keeps the familiar Android location but becomes profile-aware.

```text
Display
  brightness
  dark theme
  font size
  display size
  rotation
  refresh rate where supported
  app layout compatibility
  square/compact layout mode
  rounded-corner / cutout handling where present
  screen saver / ambient display where supported
```

AOD must not be exposed as a normal toggle on devices whose profile says it is
unsupported.

```text
Titan 2:
  AOD hidden or marked unsupported
  SubScreen / attention controls live under Attention & SubScreen

Titan 2 Elite / Q27:
  AOD candidate only after doze/panel/power validation
```

## Attention & SubScreen

This section owns notification attention surfaces that are not ordinary display
or notification toggles.

```text
Attention & SubScreen
  Always-on display / ambient display where supported
  notification pulse
  keyboard backlight alerts
  haptics
  LED / flash attention where supported
  rear SubScreen / glance display where present
  privacy mode
```

Titan 2 rear SubScreen should not be presented as a general app display. It is a
privacy-controlled attention/glance surface unless later evidence proves a safe
broader role.

```text
SUBSCREEN_GENERAL_APP_SURFACE=NO_BY_DEFAULT
SUBSCREEN_GLANCE_SURFACE=YES_CANDIDATE
```


## Battery

Battery keeps Android's familiar top-level location but gains Sable-owned usage,
health and charging-protection presentation where the platform can support it.

```text
Battery
  current level / charging state
  Battery usage
  Battery health
  Charging & protection
  Battery saver
```

Keyboard-first posture:

- list-first on square/compact displays;
- deterministic visible focus;
- type-ahead aliases such as Usage / Health / Charging;
- bounded charts with Enter-to-inspect and Left/Right bucket navigation;
- Back/Escape exits chart inspection and restores row focus;
- no gesture-only battery data.

Health/control truthfulness is part of the UX contract. Capacity health, cycle
count and charging controls are capability-gated per device profile; unavailable
data remains explicitly unavailable.

See:

- `BATTERY_HEALTH_USAGE_UX.md`
- `BATTERY_HEALTH_USAGE_VISUAL_CONFIRMATION.md`

## Apps

Apps should retain Android's expected location and behavior, but Sable must add
privacy-forward summaries.

```text
Apps
  Recently opened apps
  All apps
  Default apps
  App battery usage
  Special app access
  Permission manager
```

All Apps must support keyboard navigation and should show privacy-impacting
capabilities beneath the app name where space allows.

```text
App Name
Camera · Microphone · Location · Contacts · Notifications
```

The package name belongs in details/debug views, not the primary list.

## Diagnostics

Diagnostics is a Sable-owned section for read-only or gated hardware checks. It
must not become a generic privileged mutation console.

```text
Diagnostics
  Device profile
  Display test
  Keyboard test
  Pointer/touch surface test
  Critical text-entry test
  Audio test
  Microphone test
  Camera capability report
  Sensors
  Radio information
  Factory-test bridge where available and safe
  Export diagnostic report
```

Factory/engineering surfaces discovered through dialer codes should be treated as
source evidence for Sable diagnostics, not as normal user settings.

## Type-ahead focus navigation

Settings should support fast, keyboard-native navigation without forcing every
printable key into full search. When no text field is active and the Settings
search box is not focused, printable letters form a short-lived type-ahead focus
buffer.

```text
TYPE_AHEAD_FOCUS_NAVIGATION=REQUIRED
TYPE_AHEAD_DEFAULT_SCOPE=visible_settings_rows_then_all_rows
TYPE_AHEAD_TIMEOUT_MS=800
TYPE_AHEAD_CASE_INSENSITIVE=YES
TYPE_AHEAD_ACCENT_INSENSITIVE=YES
TYPE_AHEAD_WRAP=YES
TYPE_AHEAD_VISIBLE_FEEDBACK=YES
```

Example on the top-level Settings list:

```text
initial focus: Search settings or first top-level row
press S:
  focus Sound & vibration

press S then Y before timeout:
  buffer = sy
  focus System

press S after timeout:
  buffer = s
  focus Sound & vibration again

press S repeatedly:
  cycle through rows whose normalized title starts with s
  e.g. Sound & vibration -> Security & emergency -> System, if present
```

Type-ahead must use normalized Settings titles and aliases. This allows renamed
or localized settings to remain discoverable without inventing a separate
keyboard-only taxonomy.

```text
Sound & vibration:
  sound
  volume
  vibration
  do not disturb

System:
  system
  language
  date
  time
  backup
  reset

Keyboard & input:
  keyboard
  keys
  qwerty
  fn
  alt
  sym
  input
```

Type-ahead is focus navigation, not search results. It should move the visible
focus ring to the best matching Settings row and keep the user in the current
screen. Enter then opens the focused row.

## Search versus type-ahead

Sable Settings has two keyboard discovery paths:

```text
type-ahead focus navigation:
  fast row jump within the current Settings screen

Settings search:
  full indexed search across all Settings screens and aliases
```

Recommended trigger behavior:

```text
Printable key, no text field focused:
  type-ahead focus navigation

/ or Search key or Ctrl+K / Command+K where available:
  open/focus Settings search

Enter on Search settings row:
  open/focus Settings search

Typing while Settings search is focused:
  edit the search query

Back while Settings search is active:
  close search and restore previous focus
```

This keeps Android's familiar visible search box while making physical-keyboard
navigation fast enough for one-handed use.

## Mouse and pointer interaction

Pointer support must not break keyboard navigation. Settings should use a
last-input-wins focus model with separate pointer hover and keyboard focus.

```text
LAST_INPUT_WINS=YES
POINTER_HOVER_DOES_NOT_STEAL_KEYBOARD_FOCUS=YES
POINTER_CLICK_MOVES_FOCUS_AND_ACTIVATES=YES
KEYBOARD_TYPE_AHEAD_MOVES_FOCUS_NOT_POINTER=YES
```

When a mouse, touchpad or touch surface is used:

```text
pointer move:
  show pointer / hover affordance
  do not erase keyboard focus unless configured by accessibility setting

pointer click / tap:
  focus and activate the clicked row or control

keyboard letter after pointer use:
  return to keyboard focus mode
  run type-ahead focus navigation unless a text field is active

keyboard arrows after pointer use:
  move from the last focused/clicked row
```

If a device exposes a keyboard-driven mouse-control mode, that mode must be
explicit and reversible. While keyboard-driven mouse mode is active, letter keys
used for pointer movement must not also trigger Settings type-ahead.

```text
MOUSE_CONTROL_MODE=EXPLICIT
MOUSE_CONTROL_MODE_INDICATOR=REQUIRED
TYPE_AHEAD_DISABLED_DURING_KEY_MOUSE_MODE=YES
EXIT_MOUSE_MODE_REQUIRED=YES
```

Recommended behavior:

```text
Fn + pointer profile key:
  toggle keyboard-driven mouse mode, if the device profile supports it

Esc / Back:
  exit mouse mode before leaving the screen, if no pointer drag/test is active

Search key or /:
  exit mouse mode and focus Settings search

Enter / Space:
  click/activate the pointer target in mouse mode
```

Text-entry fields always take priority over type-ahead and pointer shortcuts.

```text
TEXT_FIELD_FOCUSED=TEXT_ENTRY_MODE
ALT_SYM_FN_TEXT_ENTRY_REQUIRED=YES
TYPE_AHEAD_DISABLED_IN_TEXT_ENTRY=YES
MOUSE_CONTROL_DISABLED_IN_TEXT_ENTRY_UNLESS_EXPLICIT=YES
```

## Navigation requirements

Settings must be fully operable with the physical keyboard.

```text
VISIBLE_FOCUS=REQUIRED
ARROW_NAVIGATION=REQUIRED
ENTER_ACTIVATION=REQUIRED
BACK_ESCAPE=REQUIRED
TYPE_AHEAD_FOCUS_NAVIGATION=REQUIRED
SETTINGS_SEARCH=REQUIRED
FOCUS_RESTORATION=REQUIRED
NO_FOCUS_TRAPS=REQUIRED
TOUCH_SECONDARY=YES
```

The focused row/card must be visually obvious on square and compact displays.

## Titan 2 default posture

```text
DISPLAY_CLASS=square_lcd_with_rear_subscreen
AOD=NO_BY_DEFAULT
SUBSCREEN=ATTENTION_GLANCE_CANDIDATE
KEYBOARD=PRIMARY_INPUT
TOUCH=SECONDARY_INPUT
SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
CRITICAL_TEXT_ENTRY_TEST=REQUIRED
```

## Titan 2 Elite / Q27 default posture

```text
DISPLAY_CLASS=compact_amoled_keyboard_device_candidate
AOD=CANDIDATE_REQUIRES_VALIDATION
KEYBOARD=PRIMARY_INPUT
TOUCH_OR_POINTER=PROFILE_DEPENDENT
SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
CRITICAL_TEXT_ENTRY_TEST=REQUIRED
```

## Acceptance

```text
SETTINGS_STRUCTURE_ANDROID_COMPATIBLE=PASS
SETTINGS_SEARCH_REQUIRED=PASS
TYPE_AHEAD_FOCUS_NAVIGATION=PASS
KEYBOARD_NAVIGATION_REQUIRED=PASS
MOUSE_POINTER_INTERACTION_MODEL=PASS
DISPLAY_PROFILE_AWARE=PASS
ATTENTION_PROFILE_AWARE=PASS
CRITICAL_TEXT_ENTRY_TEST_REQUIRED=PASS
FACTORY_TEST_BRIDGE_GATED=PASS
BUILD_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
