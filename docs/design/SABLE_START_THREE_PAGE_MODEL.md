# Sable Start three-page Titan 2 model

Status: **design contract — 3-page Start model and customizable quick bar**

This document extends the Sable Start keyboard-first UX with the revised Titan 2
home model:

```text
LEFT_PAGE=CUSTOMIZABLE_GLANCE_WIDGETS
CENTER_PAGE=START_LAUNCHER
RIGHT_PAGE=HUB_COMMUNICATIONS
BASE_QUICK_BAR=CONFIGURABLE
WEATHER_WIDGET_ON_HOME=YES
FREEFORM_WIDGET_CANVAS=NO_BY_DEFAULT
```

The goal is to keep Sable Start useful as a phone, a communication device and a
keyboard productivity launcher without wasting scarce base-bar space.

## Page model

Sable Start uses a simple three-page mental model on Titan 2:

```text
Swipe right / Fn+Right from Center -> Hub page
Swipe left  / Fn+Left  from Center -> Glance widgets page
Center page                       -> launch apps, actions and commands
```

The pages are semantic, not separate launchers:

```text
Left    = glanceable information and widgets
Center  = launching, pinned apps, base quick bar, type-to-launch
Right   = Hub communications and priority messages
```

This allows Hub to be very close even when the user removes Hub from the base
quick bar.

## Center page — Start / Launch

The center page is the default home page.

```text
┌────────────────────────────────────────┐
│ 10:46                              99% │
├────────────────────────────────────────┤
│ Weather widget                         │
│ 72°  Partly cloudy      Fri 68° Sat 70°│
├────────────────────────────────────────┤
│ Search or command                      │
├────────────────────────────────────────┤
│ Home apps                              │
│ Messages                               │
│ Mail                                   │
│ Calendar                               │
│ Contacts                               │
│ Phone                                  │
│ Camera                                 │
├────────────────────────────────────────┤
│ [Slot 1] [Slot 2] [Command] [Apps]     │
└────────────────────────────────────────┘
```

The first screen should not use the SableOS logo/text as a large content block.
That space is better used for a real glance module such as Weather.

```text
HOME_BRANDING_BLOCK=NO_BY_DEFAULT
HOME_WEATHER_WIDGET=DEFAULT_CANDIDATE
HOME_TOP_MODULE=USER_CONFIGURABLE
```

## Left page — Glance widgets

The left page is a structured widget/module surface. It is not an Android-style
freeform widget canvas by default.

```text
┌────────────────────────────────────────┐
│ Glance                                 │
├────────────────────────────────────────┤
│ Weather                                │
│ 72° partly cloudy · hourly / 3-day     │
├────────────────────────────────────────┤
│ Calendar                               │
│ Next: Team Sync 2:00 PM                │
├────────────────────────────────────────┤
│ Battery                                │
│ 90% · charging / estimated remaining   │
├────────────────────────────────────────┤
│ Media                                  │
│ Nothing playing / now playing          │
├────────────────────────────────────────┤
│ Tasks / reminders                      │
└────────────────────────────────────────┘
```

Supported first-party glance widgets / home modules:

```text
Weather
Calendar / next event
Hub summary
Mail summary
Media / now playing
Battery / charging
Tasks / reminders
Device status
```

Rules:

```text
CURATED_SABLE_WIDGETS=YES
GENERIC_ANDROID_WIDGETS=FUTURE_CANDIDATE
FREEFORM_PLACEMENT=NO_BY_DEFAULT
REORDER_WIDGETS=YES
HIDE_WIDGETS=YES
COMPACT_OR_EXPANDED_WIDGET_SIZE=YES
```

## Right page — Hub

The right page is the close communications page. It exists even when Hub is not
pinned in the base quick bar.

```text
┌────────────────────────────────────────┐
│ Hub                                    │
├────────────────────────────────────────┤
│ Priority                               │
│ Messages · Mail · Missed calls         │
├────────────────────────────────────────┤
│ Messages                               │
│ 2 unread                               │
├────────────────────────────────────────┤
│ Mail                                   │
│ 1 unread                               │
├────────────────────────────────────────┤
│ Calendar                               │
│ Team Sync 2:00 PM                      │
├────────────────────────────────────────┤
│ Missed calls                           │
└────────────────────────────────────────┘
```

Hub page rules:

```text
HUB_RIGHT_PAGE=YES
HUB_BASE_BAR_SLOT=USER_CHOICE
HUB_FONT=COMPACT_READABLE
LOCKED_PRIVACY_MODE=COUNTS_ONLY_BY_DEFAULT
PROVIDER_OWNERSHIP_PRESERVED=YES
```

The Hub page is not a replacement for the Sable Hub app. It is a launcher-adjacent
communications surface that opens Sable Hub for full interaction.

## Base quick bar customization

The base quick bar is required, but its contents are not hardcoded.

Default recommendation:

```text
Slot 1: Phone
Slot 2: Hub
Slot 3: Command
Slot 4: All Apps
```

Allowed user configuration:

```text
Slot 1..4: user-selectable app/action subject to safety rules
Command: recommended persistent slot, but may be moved if another explicit command shortcut exists
All Apps: recommended persistent slot, but may be replaced only if a visible All Apps path remains
Hub: recommended default, replaceable because right-swipe/Fn+Right provides Hub access
Phone: recommended default on phone devices, replaceable by user
```

Optional 5-slot profile where readable:

```text
Slot 1: Phone
Slot 2: Hub or Browser
Slot 3: Command
Slot 4: Camera or user app
Slot 5: All Apps
```

Examples:

```text
Communication default:
  Phone | Hub | Command | All Apps

Browser-first user:
  Phone | Browser | Command | All Apps

Camera-first user:
  Phone | Camera | Command | All Apps

Minimal launcher user:
  Browser | Mail | Command | All Apps
```

Safety and discoverability rules:

```text
BASE_BAR_REORDER=YES
BASE_BAR_REPLACE_SLOTS=YES
BASE_BAR_RESET_TO_DEFAULT=REQUIRED
BASE_BAR_COMMAND_PATH_REQUIRED=YES
ALL_APPS_PATH_REQUIRED=YES
HUB_PATH_REQUIRED=YES_BY_RIGHT_PAGE_OR_SLOT
PHONE_PATH_RECOMMENDED_ON_PHONE=YES
UNSAFE_DIAGNOSTICS_IN_BASE_BAR=NO_BY_DEFAULT
```

## Edit mode

Users must be able to reorder home apps and base quick-bar slots.

Entry points:

```text
Long press empty Home area
Menu key on Home
Settings > Sable Start > Home layout
Long press quick-bar slot
```

Keyboard behavior:

```text
Up / Down
  move selected item in vertical list

Left / Right
  move selected item between quick-bar slots or sections

Enter
  pick up / drop item

Space
  select item or open item options

Back
  cancel current move or leave edit mode

Menu / Fn+Enter
  item options: Replace, Remove, Move, Reset slot
```

Pointer/touch behavior:

```text
drag handle
  reorder apps/widgets/quick slots

long press
  item options

click Done
  save layout
```

## Swipe and keyboard page navigation

Swipe behavior:

```text
Swipe left on Center
  open Glance widgets page

Swipe right on Center
  open Hub page

Swipe down
  notifications / quick settings

Swipe up
  All Apps if enabled by user preference
```

Keyboard behavior:

```text
Fn + Left
  open Glance widgets page

Fn + Right
  open Hub page

Home
  return to Center page

Back
  return one page/layer toward Center, then Home

Left / Right without Fn
  move focus within the current page
```

This avoids overloading normal focus navigation while still making page switches
fast.

## Acceptance

```text
THREE_PAGE_START_MODEL=PASS
CENTER_PAGE_START=PASS
LEFT_PAGE_GLANCE_WIDGETS=PASS
RIGHT_PAGE_HUB=PASS
WEATHER_WIDGET_ON_HOME=PASS
CURATED_WIDGETS=PASS
GENERIC_FREEFORM_WIDGETS=NO_BY_DEFAULT
BASE_QUICK_BAR_CONFIGURABLE=PASS
HUB_BASE_SLOT_REPLACEABLE=PASS
HUB_ACCESS_STILL_AVAILABLE=RIGHT_PAGE
BASE_BAR_REORDER=PASS
HOME_APPS_REORDER=PASS
KEYBOARD_PAGE_NAVIGATION=PASS
TOUCH_SWIPE_PAGE_NAVIGATION=PASS
NO_BUILD_CODE_CHANGED=PASS
NO_DEVICE_CODE_CHANGED=PASS
FLASH_ENABLEMENT=NO
```
