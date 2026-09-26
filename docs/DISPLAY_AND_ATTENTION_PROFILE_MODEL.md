# Display and attention profile model

Status: **architecture contract — required before keyboard-device UI freeze**

SableOS must model screen geometry, panel behavior and attention surfaces independently. Keyboard-first devices are not interchangeable: Titan 2 has a square LCD-class main display plus rear SubScreen, while Titan 2 Elite and Zinwa Q27 are compact AMOLED keyboard devices where AOD may be possible but must be validated.

## Display classes

```text
TOUCH_SLAB
  Pixel/Panther reference class.

SQUARE_KEYBOARD
  Square or near-square main display attached to a physical keyboard.
  Titan 2 main screen starts here.

COMPACT_AMOLED_KEYBOARD
  Compact portrait-ish AMOLED keyboard device.
  Titan 2 Elite and Zinwa Q27 start here, pending evidence.

SECONDARY_GLANCE_DISPLAY
  Small secondary display used for restricted status, notification, media,
  call or camera/selfie flows.
  Titan 2 rear SubScreen starts here.
```

A display class is a design input, not a release claim.

## Main display contract

Every device profile must record:

```text
physical_size
resolution
logical_resolution
logical_density
smallest_width_dp
refresh_modes
panel_type
natural_orientation
rotation_policy
cutout_policy
rounded_corner_policy
touch_association
display_id_stability
```

Runtime display IDs are not stable product identity. Apps and system surfaces must use profile semantics, not hard-coded display ID numbers.

## Secondary display contract

If a secondary display exists, record:

```text
present
role
resolution
logical_density
touch_association
wake_policy
brightness_owner
security_policy
notification_policy
camera_policy
app_launch_policy
input_active_when_disabled
privacy_mode
```

Titan 2's rear SubScreen is not a general second-phone surface by default. It should begin as a restricted attention/glance surface until stock behavior and first-Sable behavior prove otherwise.

## Panel power classes

```text
LCD_NO_AOD_BY_DEFAULT
  AOD disabled by default. Pulse or screen-on notification behavior requires
  power and privacy validation.

AMOLED_AOD_CANDIDATE_REQUIRES_VALIDATION
  AOD may be supported but requires SystemUI/doze evidence, panel behavior,
  burn-in mitigation and idle power measurement.

UNKNOWN_PANEL_POWER
  Fail closed for AOD/pulse claims.
```

AOD is not a theme option and not a simple SystemUI toggle. It is a capability derived from panel type, vendor doze policy, SystemUI configuration, burn-in handling and measured power.

## AOD and pulse policy

```text
AOD_SUPPORTED=NO_BY_DEFAULT_UNLESS_VALIDATED
PULSE_SUPPORTED=NO_BY_DEFAULT_UNLESS_VALIDATED
AOD_ON_LCD=NO_BY_DEFAULT
AOD_ON_AMOLED=CANDIDATE_ONLY
```

Titan 2 starts as:

```text
TITAN2_AOD=NO_BY_DEFAULT
TITAN2_REAR_SUBSCREEN_GLANCE=CANDIDATE
TITAN2_PULSE=TEST_ONLY_UNTIL_POWER_EVIDENCE
```

Titan 2 Elite and Q27 start as:

```text
AMOLED_AOD=CANDIDATE_REQUIRES_VALIDATION
BURN_IN_MITIGATION=REQUIRED
IDLE_POWER_TEST=REQUIRED
```

## Attention surfaces

SableOS must route notification/attention behavior through available surfaces:

```text
AOD
PULSE
SECONDARY_DISPLAY
NOTIFICATION_LED
KEYBOARD_BACKLIGHT
HAPTIC
SOUND
STATUS_BAR
LOCKSCREEN
```

Each surface has a privacy policy:

```text
locked_private
locked_count_only
locked_sender_only
unlocked_preview
unlocked_action
```

Default policy for secondary displays and AOD is privacy-minimal until the user opts in.

## Product layout mapping

```text
Sable Start:
  TOUCH_SLAB -> Pixel reference layout
  SQUARE_KEYBOARD -> square list/card layout
  COMPACT_AMOLED_KEYBOARD -> compact keyboard-first portrait layout

Sable Hub:
  TOUCH_SLAB -> touch-first hub
  SQUARE_KEYBOARD -> dense square triage layout
  COMPACT_AMOLED_KEYBOARD -> list-first keyboard triage layout
  SECONDARY_GLANCE_DISPLAY -> privacy-restricted glance only

Sable Command:
  TOUCH_SLAB -> bottom/center overlay
  SQUARE_KEYBOARD -> compact centered overlay with strong focus state
  COMPACT_AMOLED_KEYBOARD -> top/center command bar with list results

Sable Settings:
  two-pane only when display profile proves usable width/height
  otherwise list-first with keyboard section jumps
```

## Acceptance before UI freeze

```text
DISPLAY_PROFILE_DEFINED=YES
PANEL_POWER_PROFILE_DEFINED=YES
ATTENTION_SURFACE_PROFILE_DEFINED=YES
AOD_POLICY_DEFINED=YES
SECONDARY_DISPLAY_POLICY_DEFINED=YES_OR_NOT_APPLICABLE
PRIVACY_POLICY_DEFINED=YES
```
