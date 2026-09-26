# Zinwa Q27 capability profile

Status: **research candidate / no Sable baseline**

```text
DEVICE=q27
VENDOR=Zinwa
LANE=TREBLE_PORTABILITY_RESEARCH
FIRST_SABLE_ARTIFACT=ABSENT
FIRST_SABLE_BOOT=NOT_RUN
PUBLIC_BUILD_IMAGE=FAIL_CLOSED
PUBLIC_FLASH=NO
PROFILE_INHERITS_TITAN2=NO
PROFILE_INHERITS_TITAN2_ELITE=NO
```

Q27 is a future candidate only. Public reports are useful for architecture
planning, but no SableOS release decision may depend on Q27 until retail or
otherwise accepted hardware, stock firmware, restore path and input/display
baselines are independently captured.

## Display profile

```text
main_display:
  class: COMPACT_AMOLED_KEYBOARD_CANDIDATE
  public_size: about_3.92_to_4.0_inch
  public_resolution: 1080x1240
  public_refresh: 90hz_candidate_if_vendor_verified
  aspect_class: compact_near_square_portrait
  panel_power_class: amoled_candidate
  aod_policy: CANDIDATE_REQUIRES_VALIDATION
  pulse_policy: CANDIDATE_REQUIRES_VALIDATION
  layout_default: keyboard_first_compact_portrait

secondary_display:
  class: NONE_CLAIMED_BY_SABLE_UNTIL_VERIFIED
```

Q27 must not inherit Titan 2 or Titan 2 Elite display behavior. Its display,
keyboard geometry, pointer/touch capability and system software must be treated
as separate until measured.

## Attention surfaces

```text
attention:
  aod: CANDIDATE_REQUIRES_VALIDATION
  pulse: CANDIDATE_REQUIRES_VALIDATION
  keyboard_backlight: UNKNOWN
  notification_led: UNKNOWN
  haptics: REQUIRED_FALLBACK
  sound: REQUIRED_FALLBACK
  secondary_display: NO_CLAIM
```

## Keyboard and pointer profile

```text
keyboard_profile: q27
keyboard_class: physical_qwerty_candidate
modifier_model: UNKNOWN
special_keys: UNKNOWN
pointer_surface: UNKNOWN
mouse_mode: UNKNOWN
keyboard_backlight: UNKNOWN
```

Q27 keyboard behavior is not a Titan 2 derivative for Sable purposes. It may be
closer to BlackBerry Q-series/Classic ergonomics, but that is a UX inspiration,
not an evidence claim.

## Critical text-entry gates

These are independent release blockers.

```text
Q27_BT_PAIRING_TEXT_ENTRY=REQUIRED
Q27_ALT_ENTRY_SYSTEM_DIALOG=REQUIRED
Q27_SYM_ENTRY_SYSTEM_DIALOG=REQUIRED
Q27_SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
Q27_SETUP_WIZARD_TEXT_ENTRY=REQUIRED
Q27_LOCKSCREEN_TEXT_ENTRY=REQUIRED
Q27_WIFI_PASSWORD_TEXT_ENTRY=REQUIRED
Q27_ACCOUNT_SIGNIN_TEXT_ENTRY=REQUIRED
```

No Titan 2 or Titan 2 Elite PASS may be copied to Q27.

## Layout acceptance

```text
sable_start:
  default: keyboard_first_compact_portrait_candidate
  command: required
  visible_focus: required

sable_hub:
  default: dense_keyboard_list_candidate
  peek_flow: candidate
  aod_privacy: required_if_aod_enabled

sable_settings:
  default: list_first_compact
  search: required

sable_media:
  compact_portrait_controls: required

sable_reader:
  compact_text_layout: required
  line_page_keyboard_scroll: required
```

## Open evidence requirements

```text
RETAIL_HARDWARE_BASELINE=ABSENT
FACTORY_FIRMWARE_BASELINE=ABSENT
RESTORE_PATH=ABSENT
DISPLAY_ID_MAIN=UNKNOWN
DOZE_CONFIG=UNKNOWN
AOD_IDLE_DRAIN=UNKNOWN
BURN_IN_MITIGATION=UNKNOWN
KEYBOARD_EVENT_MAP=UNKNOWN
POINTER_SURFACE_EVENT_MAP=UNKNOWN
SOFTWARE_KEYBOARD_FORCE_SHOW=UNKNOWN
VENDOR_API=UNKNOWN
VNDK=UNKNOWN
PARTITION_MODEL=UNKNOWN
```

## Release posture

```text
Q27_PROFILE=RESEARCH_ONLY
Q27_BUILD_READY=NO
Q27_FLASH_READY=NO
```
