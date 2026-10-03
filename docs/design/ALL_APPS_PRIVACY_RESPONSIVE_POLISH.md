# DESIGN-KF-D — All Apps Privacy Row and Responsive App Polish Addendum

Status: **accepted implementation-ready design contract**  
Date: **2026-10-03**

This addendum closes the remaining All Apps privacy-row detail and turns Panther
physical-review defects into common responsive rules for keyboard-first devices.

## Part 1 — All Apps privacy/security row

All Apps remains label-first.

Default row:

```text
[icon] App Name
       permissions · Camera · Microphone · +2
```

If no high-signal access is effectively allowed:

```text
permissions · none sensitive
```

Package names remain hidden by default.

## Truthfulness rule

The summary shows **currently effective user-relevant access**, not merely
manifest declarations.

For runtime permissions/AppOps-managed capabilities, implementation should show a
group only when the current Android user/profile has effective allowed access.

If effective state cannot be determined safely, omit the group rather than infer.

```text
REQUESTED_PERMISSION_ONLY=INSUFFICIENT
CURRENT_USER_PROFILE_REQUIRED=YES
APP_OP_EFFECTIVE_STATE_USED_WHEN_RELEVANT=YES
UNKNOWN_STATE=OMIT_NOT_GUESS
SEPARATE_PERMISSION_DATABASE=NO
```

## High-signal groups

Initial compact groups:

```text
Location
Camera
Microphone
Contacts
Phone
Messages
Calendar
Photos
Audio
Nearby
Notifications
Accessibility
Device admin
Install apps
Overlay
Usage access
```

The default compact row prioritizes the first eleven everyday access groups.
Special-access groups may use a warning/security badge and appear in expanded
privacy detail rather than consuming the entire row.

## Compact row density

Square-display row rules:

```text
MAX_INLINE_PERMISSION_LABELS=3
EXCESS_LABELS=+N
SECONDARY_LINE_MAX=1
RAW_PERMISSION_NAMES=NO
PACKAGE_NAME=NO
WARNING_ICON_ONLY_WITH_ACCESSIBLE_LABEL=YES
```

Example:

```text
Maps
permissions · Location · Contacts · Notifications · +1
```

When width is smaller:

```text
Maps
permissions · Location · Contacts · +2
```

No mid-word clipping.

## Partial access

Where Android exposes partial media/location access, the summary may distinguish
it only when the state is reliable and understandable.

Allowed examples:

```text
Photos limited
Location approximate
```

Do not create ambiguous badges such as "restricted" without explanation.

## Notifications

"Notifications" appears only when notifications are effectively enabled for the
app/user and the platform permission/policy allows them.

Do not equate presence of POST_NOTIFICATIONS in the manifest with actual allowed
notifications.

## Multi-user and work profiles

Privacy state is keyed to the app instance in the current Android user/profile.

```text
PACKAGE_ONLY_CACHE_KEY=FORBIDDEN
WORK_PROFILE_BADGE=REQUIRED
CROSS_PROFILE_PERMISSION_MERGE=NO
```

The same package may display different summaries in personal and work profiles.

## Data freshness and performance

Launcher must not issue expensive permission/AppOps queries during every row draw.

Use an ephemeral background snapshot/cache scoped to process/session and user,
with invalidation on relevant package/permission/profile changes.

```text
PERSISTENT_LAUNCHER_PERMISSION_DB=NO
ROW_BIND_BLOCKING_QUERY=NO
BACKGROUND_SNAPSHOT=YES
INVALIDATE_ON_PACKAGE_CHANGE=YES
INVALIDATE_ON_PERMISSION_CHANGE_WHERE_SIGNAL_AVAILABLE=YES
```

App info remains the authoritative full detail surface.

## All Apps keyboard behavior

```text
Up/Down
  move app focus

Enter
  open app

Space
  expand compact privacy detail, if implemented

Menu / Fn+Enter
  actions

/ or Search
  search

Printable letters
  type-to-jump when search not active
```

Privacy detail/actions must not require long-press/touch.

App actions include:

```text
Open
App info
Permissions / Privacy & security
Notification settings
Pin to Start
Add to base bar
Uninstall when allowed
Disable when Android policy allows
```

Destructive actions require confirmation.

## Part 2 — Responsive polish rules

These rules apply to common Sable apps and specifically close Panther findings
that must not propagate into Titan keyboard-first layouts.

### Navigation destinations

Compact/square top-level navigation must not clip labels.

```text
MID_WORD_TAB_CLIPPING=FORBIDDEN
OVERFLOWING_FIXED_TAB_ROW=FORBIDDEN
MAX_COMPACT_SIMULTANEOUS_TOP_DESTINATIONS=3
SECONDARY_DESTINATIONS=OVERFLOW_OR_EXPLICIT_PAGE
```

If four or more destinations exist, prefer:

- a compact pivot/dropdown;
- an explicit More destination;
- a two-level header;
- a keyboard-accessible overflow menu.

Do not make tiny text fit four/five labels.

### Media mini-player

A mini-player must always communicate state.

Minimum compact content:

```text
title or source
play/pause
state/progress indicator where meaningful
optional secondary metadata
```

```text
SOLID_ACCENT_BAR_WITHOUT_MEANING=FORBIDDEN
PLAYBACK_STATE_AMBIGUOUS=FORBIDDEN
```

Secondary actions move to a context menu on constrained layouts.

### Dense list rows

On compact layouts:

```text
PRIMARY_TEXT=1-2_LINES
SECONDARY_TEXT=1_LINE_DEFAULT
PRIMARY_TRAILING_ACTIONS_MAX=1
SECONDARY_ACTIONS_TO_MENU=YES
TOUCH_TARGET_MIN=48dp
VISIBLE_KEYBOARD_FOCUS=YES
```

Do not pack title, subtitle, star/add/play and multiple extra actions into one
narrow row.

### Alphabet navigation

Contacts/People and similar indexed lists expose exactly one functional
alphabet-index concept.

```text
DUPLICATE_LEFT_RIGHT_ALPHABET_RAILS=FORBIDDEN
KEYBOARD_A_TO_Z_JUMP=PRIMARY_ON_KEYBOARD_DEVICES
TOUCH_ALPHABET_RAIL=OPTIONAL_SECONDARY
ALPHABET_RAIL_MUST_FUNCTION_IF_VISIBLE=YES
```

If grouping headers already provide alphabet context, do not add a second
nonfunctional decorative index.

### Latest/current content

For communications/media/productivity surfaces:

```text
CURRENT_OR_LATEST_STATE_VISIBLE_ON_OPEN=YES
SCROLL_TO_DISCOVER_CURRENT_STATE=NO
RETURN_TO_CURRENT_ACTION=REQUIRED_WHERE_HISTORY_EXISTS
```

### Long labels

```text
MID_WORD_CLIP=NO
ELLIPSIS_ALLOWED=YES
TOOLTIP_OR_ACCESSIBLE_FULL_LABEL=YES_WHEN_TRUNCATED
KEYBOARD_FOCUS_REVEALS_FULL_CONTEXT=YES_WHERE_PRACTICAL
```

### Focus restoration

Returning from detail/context/app-info restores the prior meaningful focus where
the item still exists.

```text
FOCUS_RESTORATION=REQUIRED
BACK_RETURNS_ONE_LOGICAL_LAYER=REQUIRED
POINTER_HOVER_STEALS_FOCUS=NO
```

This directly covers Search/Peek/App-info style nested flows.

## Visual confirmation requirements

Implementation review must capture:

```text
All Apps with no-sensitive row
All Apps with 1 permission
All Apps with 3 permissions
All Apps with >3 permissions
work-profile duplicate package with different state
large font-scale All Apps
Media compact navigation without clipping
Media mini-player
Contacts single functional alphabet index
dense list row with overflow actions
focus restoration after detail/app-info
```

## Acceptance

```text
ALL_APPS_EFFECTIVE_PRIVACY_SUMMARY=PASS
ALL_APPS_CURRENT_USER_SCOPED=PASS
ALL_APPS_RAW_PERMISSION_NAMES=PASS_ABSENT
ALL_APPS_PERMISSION_DB=PASS_ABSENT
ALL_APPS_MAX_INLINE_PERMISSION_LABELS=PASS_3
ALL_APPS_WORK_PROFILE_SEPARATION=PASS
ALL_APPS_ROW_BIND_BLOCKING_QUERY=PASS_ABSENT
RESPONSIVE_TAB_CLIPPING=PASS_ABSENT
MEDIA_MINI_PLAYER_MEANINGFUL=PASS
DENSE_ROW_ACTION_OVERFLOW=PASS
CONTACTS_DUPLICATE_ALPHABET_RAIL=PASS_ABSENT
VISIBLE_ALPHABET_RAIL_FUNCTIONAL=PASS
FOCUS_RESTORATION=PASS
FONT_SCALE_COMPACT_LAYOUT=PASS
```
