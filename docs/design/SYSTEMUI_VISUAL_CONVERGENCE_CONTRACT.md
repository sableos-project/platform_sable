# DESIGN-KF-B — SystemUI Visual Convergence Contract

Status: **accepted implementation-ready design contract**  
Date: **2026-10-03**

This contract closes the visual-system gap identified by Panther physical review.
SableOS keeps Android SystemUI behavior and security ownership while applying one
coherent Sable visual, focus, privacy and responsive language across platform
surfaces.

## Scope

This contract applies to:

```text
status bar
Quick Settings
notification shade
brightness/control sliders
media controls
system dialogs and transient sheets where Sable owns styling
Keyguard / Lockscreen presentation
Settings presentation
SetupWizard presentation
Sable-owned platform-facing surfaces
```

It does not authorize replacement of Android behavior, permission checks,
Keyguard security logic, notification delivery or Settings ownership.

## Product rule

```text
ANDROID_BEHAVIOR_MODEL=KEEP
SABLE_VISUAL_SYSTEM=ONE_COMMON_TOKEN_SYSTEM
GRAPHENEOS_BRANDING_IN_SABLE_PRODUCT_UI=NO_WHERE_SABLE_OWNS_PRODUCT_STRING
DEVICE_MODEL_BRANCHING_IN_STYLE=NO
PROFILE_DRIVEN_DENSITY_AND_LAYOUT=YES
```

Visual convergence means common semantics and tokens, not pixel-identical layouts
on every display class.

## Core token model

Every Sable-owned or Sable-themed system surface must consume equivalent semantic
roles:

```text
color.surface.base
color.surface.raised
color.surface.overlay
color.text.primary
color.text.secondary
color.text.disabled
color.accent
color.focus
color.success
color.warning
color.danger
color.privacy

shape.small
shape.medium
shape.large

space.1
space.2
space.3
space.4
space.6
space.8
```

Recommended baseline geometry:

```text
SPACE_GRID=4dp
ROW_MIN_HEIGHT=48dp
COMPACT_ROW_TARGET=48-56dp
CARD_RADIUS=12dp
SMALL_CONTROL_RADIUS=8dp
FOCUS_STROKE=2dp
FOCUS_OUTER_GAP=2dp
```

These are semantic baseline values; accessibility/font-scale/layout constraints
may increase sizes.

## Typography

Use a small role set rather than app-specific typography:

```text
Display / hero     28sp target
Screen title       22sp target
Section title      18sp target
Primary row        16sp target
Secondary row      14sp target
Metadata / hint    12sp target
```

Rules:

- never shrink important text merely to fit a square display;
- prefer reflow, fewer simultaneous controls or secondary menus;
- respect Android font scale;
- no text clipping at 1.3x font scale for core controls;
- long labels ellipsize at word boundaries where possible.

## State language

Keyboard focus must never be confused with selection or pressed state.

```text
FOCUS
  visible 2dp focus outline using focus role
  may coexist with selected/toggled state

SELECTED / TOGGLED
  filled or emphasized state using accent role

PRESSED
  transient state only

HOVER
  pointer affordance only; never steals keyboard focus

DISABLED
  readable label + disabled semantic state; not low-contrast disappearance
```

Privacy/redaction states use a distinct neutral/privacy treatment and never the
same styling as an error state.

## Light, dark and accent propagation

System theme is the source of truth.

```text
FOLLOW_SYSTEM=REQUIRED
LIGHT=REQUIRED
DARK=REQUIRED
SABLE_ACCENT_MAPPING=REQUIRED
APP_LOCAL_THEME_DIVERGENCE=NO_BY_DEFAULT
```

Quick Settings theme control changes the system state. Sable apps and system
surfaces consume the same semantic roles.

Accent is used for focus, active controls and restrained emphasis. It must not
turn large bars/cards into undifferentiated solid accent blocks.

## Quick Settings and shade

Retain the Android compact/expanded shade model.

Square-display target:

```text
COMPACT_SHADE_DEFAULT=YES
HIGH_VALUE_CONTROLS_FIRST=YES
TWO_COLUMN_TILE_GRID_PREFERRED_ON_SQUARE=YES
NOTIFICATION_CONTENT_REMAINS_VISIBLE=YES
PERSISTENT_LARGE_HEADER=NO
```

Profile-gated controls such as keyboard backlight, rear SubScreen and AOD appear
only when the capability is validated.

## Brightness and sliders

Sliders must expose:

```text
label
current value/state where meaningful
keyboard focus
decrement/increment semantics
touch/pointer drag
accessible role/value
```

Left/Right changes the focused slider by a bounded step. Home/End may move to
minimum/maximum only where Android behavior and accessibility semantics permit.

A slider must never become an unlabeled accent-colored bar.

## Media controls

System media controls must use MediaSession/provider-owned state.

Compact media card requirements:

```text
title visible
artist/source visible when available
play/pause visible
previous/next only when session supports them
progress shown when meaningful
artwork optional
provider/source ownership preserved
```

On compact/square displays, secondary controls move behind an explicit action
surface rather than clipping.

```text
SOLID_ACCENT_BAR_WITHOUT_METADATA=FORBIDDEN
UNSUPPORTED_MEDIA_ACTIONS=HIDDEN
LOCKSCREEN_MEDIA_PRIVACY=SOURCE_AND_USER_POLICY
```

## Lockscreen / Keyguard

Keep Android secure-entry and emergency behavior.

Visual convergence applies to:

```text
time/date hierarchy
notification cards
focus treatment
PIN/password field styling
emergency/action affordances
media card styling
privacy/redaction treatment
```

It must not alter authentication policy or permit keyboard shortcuts to bypass
Keyguard.

## Settings and SetupWizard

Settings and SetupWizard use the same typography, focus, control and privacy
tokens.

They retain Android information architecture and setup behavior. Sable styling
does not justify moving standard Android settings into a new taxonomy.

Critical text-entry surfaces must remain readable and usable if global accent,
dark mode or font scale changes.

## Dialogs and sheets

Transient system UI follows one hierarchy:

```text
title
concise explanation
primary action
secondary/cancel action
destructive action visually distinct
```

Keyboard:

```text
Tab / Shift+Tab  move actions
Enter            activate focused primary/control
Esc / Back       cancel when safe
```

Destructive actions never receive accidental one-key activation.

## Motion

Motion is restrained and functional.

```text
DEFAULT_TRANSITION=120-180ms
LARGE_DECORATIVE_PARALLAX=NO
FOCUS_MOVEMENT_ANIMATION=SUBTLE
REDUCE_MOTION_RESPECTED=YES
```

No animation may delay a security-critical or emergency action.

## Responsive rules

### Square keyboard devices

```text
CONTENT_FIRST=YES
PERSISTENT_SIDE_NAV=NO
TWO_COLUMN_CONTROLS_WHERE_CLEAR=YES
TRANSIENT_SECONDARY_ACTIONS=YES
FOCUS_ALWAYS_VISIBLE=YES
```

### Compact portrait keyboard devices

```text
SAME_SEMANTICS=YES
DENSITY_PROFILE_ADJUSTED=YES
AOD_ATTENTION_CONTROLS=CAPABILITY_GATED
```

### Touch-first Panther

Panther remains the visual regression reference for touch behavior, but keyboard
focus and token semantics must still be valid when an external keyboard is used.

## Branding

Where SableOS owns the product string, the UI must say SableOS, not GrapheneOS or
an upstream project name.

Upstream legal attribution belongs in About/legal/NOTICE surfaces, not accidental
product branding in ordinary user notifications.

## Visual regression set

Implementation must capture stable screenshots for at least:

```text
compact shade light
compact shade dark
expanded Quick Settings
notification actions
brightness slider focused
media controls compact
locked notification redaction
PIN/password secure entry
Settings top level
Setup critical text entry
system confirmation dialog
```

Capture at default font scale and one enlarged font scale.

## Acceptance

```text
SYSTEMUI_SABLE_VISUAL_CONVERGENCE=PASS
SYSTEM_THEME_SINGLE_SOURCE=PASS
VISIBLE_KEYBOARD_FOCUS=PASS
HOVER_DOES_NOT_STEAL_FOCUS=PASS
SYSTEM_MEDIA_METADATA_VISIBLE=PASS
SOLID_ACCENT_MEDIA_BAR=PASS_ABSENT
QUICK_SETTINGS_COMPACT_SQUARE=PASS
PROFILE_GATED_HARDWARE_CONTROLS=PASS
LOCKSCREEN_SECURITY_BEHAVIOR_UNCHANGED=PASS
SETTINGS_ANDROID_IA_PRESERVED=PASS
SETUP_CRITICAL_TEXT_ENTRY=PASS
SABLE_PRODUCT_BRANDING=PASS
FONT_SCALE_1_3_CORE_CONTROLS=PASS
VISUAL_REGRESSION_SET=PASS
```
