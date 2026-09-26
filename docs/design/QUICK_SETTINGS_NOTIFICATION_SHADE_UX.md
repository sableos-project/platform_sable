# Quick Settings / Notification Shade UX

Status: **keyboard-first design contract — Titan 2 visual target required**

This document defines the Sable Quick Settings and Notification Shade behavior for
keyboard-first devices. It keeps the Android notification shade mental model while
making keyboard access, theme propagation, privacy redaction, Hub handoff and
attention-surface controls first-class.

```text
QUICK_SETTINGS_NOTIFICATION_SHADE_UX=YES
ANDROID_NOTIFICATION_SHADE_MENTAL_MODEL=KEEP
KEYBOARD_FIRST_SHADE_ACCESS=YES
TITAN2_HARDWARE_TEMPLATE=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Scope

Quick Settings / Notification Shade owns:

```text
- top status glance;
- compact Quick Settings tiles;
- expanded Quick Settings tiles;
- notifications list;
- notification actions;
- privacy redaction state;
- Hub handoff;
- theme/dark-mode control path;
- keyboard backlight and attention controls;
- SubScreen / attention-surface controls where supported by profile.
```

It does not own app accounts, provider databases, notification provider internals
or unsupported provider actions.

## Structure

Sable keeps the Android pull-down model:

```text
Closed
  status bar only

First open / compact shade
  time/date/status
  compact controls
  active notifications
  Hub handoff

Expanded shade
  larger Quick Settings tile grid
  brightness / keyboard-backlight controls where supported
  attention / SubScreen controls where supported
  notifications below controls
```

For Titan 2 square display, the compact shade is preferred by default. It must
show a small set of high-value controls and avoid wasting vertical space.

## Required Quick Settings tiles

The default Titan 2 tile set should prioritize controls that matter on a
keyboard-first phone:

```text
Wi-Fi
Bluetooth
Mobile data / SIM
Dark theme
Battery saver
Do Not Disturb
Keyboard backlight
Flashlight
Location
Hotspot
SubScreen / Glance display, if supported by profile
Attention / notification light, if supported by profile
```

Profile-driven behavior:

```text
TITAN2_AOD_TILE_DEFAULT=NO
TITAN2_SUBSCREEN_TILE=CANDIDATE_REQUIRES_VALIDATION
TITAN2_KEYBOARD_BACKLIGHT_TILE=CANDIDATE_REQUIRES_VALIDATION
TITAN2_ELITE_AOD_TILE=CANDIDATE_REQUIRES_VALIDATION
Q27_AOD_TILE=CANDIDATE_REQUIRES_VALIDATION
```

A tile must not appear as a working control until the underlying profile and
hardware path are validated. Unsupported tiles may appear only as diagnostics or
explanatory entries, not as working toggles.

## Theme propagation

Dark theme must be a system source of truth, not an app-by-app guess.

```text
DARK_THEME_QS_TILE=REQUIRED
SABLE_APPS_FOLLOW_SYSTEM_THEME=REQUIRED
SABLE_START_FOLLOW_SYSTEM_THEME=REQUIRED
SABLE_SETTINGS_FOLLOW_SYSTEM_THEME=REQUIRED
SABLE_HUB_FOLLOW_SYSTEM_THEME=REQUIRED
ALL_APPS_FOLLOW_SYSTEM_THEME=REQUIRED
COMMAND_FOLLOW_SYSTEM_THEME=REQUIRED
```

The QS tile toggles the system theme. Sable apps must observe that state and
update without requiring the user to set a separate per-app dark-mode preference.

## Keyboard access

Every shade operation must work without touch.

```text
Fn + Down
  open compact shade

Fn + Down again
  expand Quick Settings

Fn + Up
  collapse one level

Back
  close shade or return from expanded to compact shade

Left / Right
  move between tiles or notification action buttons

Up / Down
  move focus between tile rows and notification rows

Enter
  activate focused tile or open focused notification

Space
  toggle focused tile or safe-peek notification

Menu / Fn + Enter
  open focused tile details or notification actions

/
  search notifications or controls when search is available

Home
  close shade and return to Home
```

No focus trap is allowed. If a notification action, tile detail screen, or
expanded control receives focus, Back must return to the previous shade level.

## Notification rows

Notification rows should preserve Android expectations while adding Sable Hub
handoff.

```text
Notification row
  app/source name
  short title
  body preview if privacy allows
  timestamp
  unread / priority markers where applicable
  focused action strip
```

Allowed actions:

```text
Open
Dismiss
Snooze
Reply, only when provider exposes safe inline reply
Open in Hub, when Hub can represent the source safely
App notification settings
```

Disallowed:

```text
Invent reply support when provider does not expose it
Read private provider databases directly
Show sensitive content in locked/private mode
Expose raw package names as the primary user-facing row label
```

## Hub handoff

The shade should make Hub reachable without turning the shade into Hub itself.

```text
HUB_HANDOFF_FROM_SHADE=YES
HUB_SUMMARY_ENTRY=YES
START_BASE_QUICK_BAR_IN_SHADE=NO
SHADE_IS_NOT_HUB=YES
```

Recommended behavior:

```text
Hub summary row
  Enter opens Sable Hub priority view
  Space shows privacy-safe peek
  Menu shows Hub filters if unlocked and allowed
```

## Privacy states

The shade must respect lockscreen and private-mode posture.

```text
LOCKED_MODE_COUNTS_ONLY=DEFAULT
PRIVATE_MODE_REDACT_BODY=DEFAULT
SENSITIVE_NOTIFICATION_REDACTION=REQUIRED
HUB_HANDOFF_PRIVACY_SAFE=REQUIRED
SCREENSHOT_SAFE_REDACTION=REQUIRED_FOR_LOCKED_SHADE
```

Locked/private examples:

```text
Messages: 2 new
Mail: 1 new
Call: missed call
```

Unlocked examples may show body previews only if the notification source allows
that display and Sable privacy settings permit it.

## Pointer and touch behavior

Touch and pointer interactions are secondary paths.

```text
Swipe down from top
  open shade

Second swipe down / drag
  expand Quick Settings

Swipe up / Back
  close shade

Pointer hover
  show hover state without stealing keyboard focus

Pointer click
  focus and activate clicked tile or notification

Keyboard after pointer
  resumes focus from last clicked item
```

If keyboard-driven mouse mode is active, normal text/navigation keys must not
accidentally trigger shade shortcuts until the user exits mouse mode or invokes an
explicit shade shortcut.

## Visual target

Source-controlled visual target:

```text
docs/design/artifacts/sable-quick-settings-titan2.svg
```

The artifact must use the Titan 2 hardware template:

```text
TITAN2_HARDWARE_TEMPLATE=YES
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=YES
NO_SLAB_PHONE_VISUAL_TARGET=YES
NO_STRETCHED_SCREEN_TARGET=YES
```

## Implementation validation checklist

Future implementation or visual review should produce evidence for:

```text
QUICK_SETTINGS_NOTIFICATION_SHADE_UX=PASS
TITAN2_QS_VISUAL_TARGET=PASS
ANDROID_NOTIFICATION_SHADE_MENTAL_MODEL=PASS
KEYBOARD_FIRST_SHADE_ACCESS=PASS
COMPACT_SHADE=PASS
EXPANDED_QS=PASS
DARK_THEME_PROPAGATION=PASS
SABLE_APPS_FOLLOW_SYSTEM_THEME=PASS
KEYBOARD_BACKLIGHT_TILE_PROFILE_GATED=PASS
SUBSCREEN_TILE_PROFILE_GATED=PASS
AOD_TILE_PROFILE_GATED=PASS
NOTIFICATION_ROW_ACTIONS=PASS
INLINE_REPLY_PROVIDER_SAFE=PASS
HUB_HANDOFF_FROM_SHADE=PASS
LOCKED_MODE_COUNTS_ONLY=PASS
PRIVATE_MODE_REDACTION=PASS
MOUSE_POINTER_INTERACTION=PASS
NO_FOCUS_TRAPS=PASS
START_BASE_QUICK_BAR_IN_SHADE=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
