# Keyboard-first device profile model

Status: **architecture contract — profile-first before Titan-family build work**

SableOS must not treat keyboard-first devices as one hardware class. Pixel 7 / panther worked as a stable slab reference, but Titan 2, Titan 2 Elite, Zinwa Q27 and future MediaTek keyboard devices differ in display geometry, panel power behavior, keyboard matrix, modifier semantics, touch/mouse surfaces, secondary displays and stock vendor policy.

This document defines the common profile model that every keyboard-first target must satisfy before SableOS makes a build, boot, flash or release-support claim.

## Core rule

```text
COMMON_SABLE_UI=YES
COMMON_SABLE_APPS=YES
COMMON_HARDWARE_PROFILE=NO
DEVICE_SPECIFIC_PROFILE=REQUIRED
```

Common Sable apps should not fork by device model. Instead, common apps consume profile facts and capability flags supplied by device adapters and release composition.

## Profile family

Every keyboard-first device must define these profiles:

```text
SableHardwareProfile
SableDisplayProfile
SablePanelPowerProfile
SableAttentionSurfaceProfile
SableKeyboardProfile
SablePointerSurfaceProfile
SableCriticalTextEntryProfile
SableAppLayoutProfile
```

A missing profile means the target remains fail-closed for user-facing image claims.

## Device identity boundary

A profile is bound to a device family and evidence set, not a retail name alone.

```text
device_family
retail_variant
region
stock_build
android_release
security_patch
vendor_api
vndk
kernel_version
panel/display facts
input-device facts
keyboard layout facts
power/attention facts
known stock policy limitations
```

Titan 2, Titan 2 Elite and Zinwa Q27 must have separate profile records.

```text
keyboard_profile/titan2       != keyboard_profile/titan2_elite
keyboard_profile/titan2       != keyboard_profile/q27
keyboard_profile/titan2_elite != keyboard_profile/q27
```

No PASS may be inherited across those devices without independent evidence.

## Minimum profile schema

```text
SableHardwareProfile:
  target_id
  device_family
  retail_variant
  stock_basis
  soc_family
  userspace_abi
  vendor_api
  vndk
  kernel_version
  treble_state
  dynamic_partition_state
  virtual_ab_state
  supported_artifact_kinds

SableDisplayProfile:
  displays[]
  default_display_class
  logical_density
  smallest_width_dp
  natural_orientation
  rotation_policy
  cutout_policy
  rounded_corner_policy
  secondary_display_policy

SablePanelPowerProfile:
  panel_type
  refresh_modes
  doze_supported
  doze_suspend_supported
  aod_supported
  pulse_supported
  burn_in_policy
  idle_power_evidence

SableAttentionSurfaceProfile:
  aod
  pulse
  secondary_display
  notification_led
  keyboard_backlight
  haptic
  sound
  privacy_mode

SableKeyboardProfile:
  physical_layout
  scan_code_map
  keylayout_file
  key_character_map
  modifier_model
  symbol_model
  numeric_entry_model
  command_key_model
  lockscreen_policy

SablePointerSurfaceProfile:
  surface_present
  surface_type
  relative_pointer
  absolute_touch
  scroll_axes
  tap_click
  long_press
  gesture_zones
  mouse_mode_policy

SableCriticalTextEntryProfile:
  setup_wizard
  bluetooth_pairing
  wifi_password
  lockscreen
  account_sign_in
  emergency_text
  recovery_prompts
  software_keyboard_fallback

SableAppLayoutProfile:
  display_class
  default_home_layout
  hub_layout
  command_overlay_layout
  settings_layout
  media_layout
  third_party_app_compatibility_class
```

## Known initial device classes

```text
Titan 2:
  display_class=SQUARE_KEYBOARD_WITH_REAR_SUBSCREEN
  panel_power_class=LCD_NO_AOD_BY_DEFAULT
  attention_class=SUBSCREEN_GLANCE_CANDIDATE
  keyboard_profile=titan2
  pointer_profile=titan2_touchpad_mouse_candidate

Titan 2 Elite:
  display_class=COMPACT_AMOLED_KEYBOARD
  panel_power_class=AMOLED_AOD_CANDIDATE_REQUIRES_VALIDATION
  attention_class=AOD_PULSE_CANDIDATE
  keyboard_profile=titan2_elite
  pointer_profile=titan2_elite_independent

Zinwa Q27:
  display_class=COMPACT_AMOLED_KEYBOARD
  panel_power_class=AMOLED_AOD_CANDIDATE_REQUIRES_VALIDATION
  attention_class=AOD_PULSE_CANDIDATE
  keyboard_profile=q27
  pointer_profile=q27_independent
```

These classes are planning labels, not support claims. They must be replaced or confirmed by evidence.

## Product rule

A Sable app may adapt layout, focus order, command shortcuts and attention behavior to the profile. It must not hard-code device-name branches unless the device adapter has exposed an explicit capability that cannot be modeled generically.

```text
GOOD:
  if display_class == SQUARE_KEYBOARD
  if attention.secondary_display == true
  if keyboard.modifier_model.sym == PAGE_OR_TIMEOUT

BAD:
  if device == titan2 then special-case everything
```

## Release gate

Before a keyboard-first target can advance from strategy to first image work:

```text
DEVICE_PROFILE_EXISTS=YES
DISPLAY_PROFILE_EXISTS=YES
PANEL_POWER_PROFILE_EXISTS=YES
ATTENTION_PROFILE_EXISTS=YES
KEYBOARD_PROFILE_EXISTS=YES
CRITICAL_TEXT_ENTRY_PROFILE_EXISTS=YES
APP_LAYOUT_PROFILE_EXISTS=YES
NO_PROFILE_INHERITANCE_WITHOUT_EVIDENCE=YES
```
