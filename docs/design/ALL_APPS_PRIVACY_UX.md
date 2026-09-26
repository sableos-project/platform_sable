# All Apps / App Privacy Summary UX

Status: **keyboard-first design contract**

All Apps is the trusted app discovery surface for Sable Start, command/search,
app actions and Settings. It must feel familiar to Android users while adding
Sable's privacy/security posture directly under each app name.

```text
ALL_APPS_SURFACE=YES
ANDROID_ALL_APPS_MENTAL_MODEL=KEEP
APP_NAMES_PRIMARY=YES
PACKAGE_NAMES_DEFAULT_VISIBLE=NO
PRIVACY_SUMMARY_UNDER_APP=YES
KEYBOARD_FIRST=YES
TITAN2_HARDWARE_TEMPLATE=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Product purpose

All Apps answers four questions quickly:

```text
What app am I looking for?
Can I trust what this app can access?
What action do I want to take?
Should this app be pinned to Start or the base quick bar?
```

It is not a replacement for Android App info. It is a fast launcher surface that
links to App info, permissions and Sable-specific pin/customization actions.

## Default structure

```text
Top
  title: All Apps
  search field
  active filter/sort indicator when applied

List
  app icon
  app display name
  privacy/security-impacting summary
  optional warning marker for sensitive access
  chevron / action affordance

Action area
  Enter opens app
  Space peeks summary or expands quick details
  Menu / long press opens app actions
```

Default ordering:

```text
Sort: alphabetical by app display name
Grouping: none by default
App names: display label only
Package name: hidden unless App details / developer mode
```

## Privacy summary under app names

The app list must show the highest-value user-facing privacy/security summary
under each app name.

Examples:

```text
Calculator
  No sensitive permissions

Calendar
  Calendar · Notifications

Camera
  Camera · Microphone · Location

Contacts
  Contacts · Notifications

Maps
  Location · Network

Messages
  SMS · Contacts · Notifications

Browser
  Camera · Microphone · Location when allowed
```

Summary rules:

```text
SHOW_PERMISSION_GROUPS=YES
SHOW_RAW_PERMISSION_NAMES=NO_BY_DEFAULT
SHOW_PACKAGE_NAME=NO_BY_DEFAULT
SHOW_SENSITIVE_BADGES=YES
SHOW_NO_SENSITIVE_PERMISSIONS=YES
SHOW_RECENT_ACCESS_BADGE=OPTIONAL_AFTER_RUNTIME_SUPPORT
```

Sensitive groups include:

```text
Camera
Microphone
Location
Contacts
Calendar
SMS
Phone
Call logs
Nearby devices / Bluetooth
Files / photos / media
Notifications
Accessibility
Device admin
Install unknown apps
Display over other apps
Usage access
```

## Filters

All Apps must support privacy-aware filtering without making the default screen
feel like an audit tool.

Required filters:

```text
All
Sensitive access
No sensitive permissions
Recently used
Pinned to Start
Base bar candidates
System apps, when enabled
```

Optional filters after implementation evidence:

```text
Recently accessed camera
Recently accessed microphone
Recently accessed location
Background access
Notifications allowed
Restricted / disabled apps
```

## Search and type-to-jump

All Apps supports both type-to-jump and explicit search.

```text
Printable letter when search is not focused
  type-ahead app focus navigation

Multiple letters within timeout
  refine prefix match

/ or Search key
  open full app search

Typing in search field
  filter app list by app name, app category and permission summaries

Back while search active
  clear search or close search and restore previous focus
```

Examples:

```text
C
  Calculator

C then A
  Calendar or Camera depending app ordering and match rank

M
  Mail or Maps

S
  Settings
```

Search must match:

```text
app display name
common aliases
permission group names
privacy words such as camera, mic, location, contacts
system words such as default app, browser, dialer, messages
```

## Keyboard interaction

```text
Up / Down
  move app row focus

Left / Right
  move filter focus when filter bar is active; otherwise no destructive action

Enter
  open focused app

Space
  expand / peek privacy summary where available

Menu / long press / Fn+Enter
  open app actions menu

Back
  return to Sable Start or previous surface

/ or Search key
  focus search

Fn + 1..5
  optional Start base quick-bar shortcuts, only when inherited from launcher context
```

All rows must have visible focus. No app action may depend on touch-only long
press.

## App actions menu

The app actions menu is required and keyboard-accessible.

Required actions:

```text
Open
App info
Permissions
Pin to Start
Add to base bar
Remove from Start, when pinned
Remove from base bar, when present
Uninstall, when allowed
Disable, for eligible system apps only when exposed by Android policy
```

Optional actions:

```text
Open split / floating mode, if device policy supports it
Notification settings
Privacy dashboard for this app
Battery usage
Storage usage
```

Dangerous actions must be confirmable and not one-key accidental:

```text
UNINSTALL_ONE_KEY=NO
DISABLE_ONE_KEY=NO
CLEAR_DATA_ONE_KEY=NO
```

## Add to Start / base bar

All Apps is the main discovery point for customizing Sable Start.

```text
PIN_TO_START=YES
ADD_TO_BASE_BAR=YES
BASE_BAR_REORDER_LINK=YES
BASE_BAR_MAX_VISIBLE_SLOTS=PROFILE_DEFINED
HUB_BASE_SLOT_REPLACEABLE=YES
COMMAND_ACCESS_REQUIRED=YES
ALL_APPS_ACCESS_REQUIRED=YES
```

If the base bar is full, the user must be offered a reorder/replace flow rather
than failing silently.

## Mouse / pointer interaction

```text
Pointer hover
  visual hover only; do not steal keyboard focus

Click row
  focus and open app, matching Enter behavior

Right click / long press
  open app actions menu

Scroll wheel
  scroll list

Click search
  enter search text mode

Keyboard after pointer use
  resumes visible keyboard focus from last focused/clicked row
```

## Privacy and trust boundaries

All Apps may summarize permissions, but it must not invent access state.

```text
PERMISSION_SUMMARY_SOURCE=ANDROID_PACKAGE_PERMISSION_STATE
RECENT_ACCESS_SOURCE=ANDROID_PRIVACY_INDICATOR_OR_DASHBOARD_WHEN_AVAILABLE
UNKNOWN_ACCESS_STATE=DO_NOT_DISPLAY_AS_FACT
PACKAGE_NAME_IN_DEVELOPER_MODE=OK
SOURCE_OF_TRUTH=ANDROID_SYSTEM_STATE
```

If runtime evidence is unavailable, the summary should show static granted
permission groups only and avoid claims like "recently used".

## Visual target

The visual target is source-controlled:

```text
docs/design/artifacts/sable-all-apps-titan2.svg
```

It must follow the Titan 2 visual-template rule:

```text
TITAN2_HARDWARE_TEMPLATE=YES
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=YES
NO_SLAB_PHONE_VISUAL_TARGET=YES
NO_STRETCHED_SCREEN_TARGET=YES
```

## Validation checklist

```text
ALL_APPS_PRIVACY_UX=PASS
APP_NAMES_PRIMARY=PASS
PACKAGE_NAMES_HIDDEN_BY_DEFAULT=PASS
PRIVACY_SUMMARY_UNDER_APP=PASS
SENSITIVE_PERMISSION_BADGES=PASS
NO_SENSITIVE_PERMISSIONS_SUMMARY=PASS
TYPE_TO_JUMP=PASS
FULL_APP_SEARCH=PASS
PRIVACY_FILTERS=PASS
APP_ACTIONS_KEYBOARD_ACCESSIBLE=PASS
PIN_TO_START=PASS
ADD_TO_BASE_BAR=PASS
MOUSE_POINTER_INTERACTION=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
