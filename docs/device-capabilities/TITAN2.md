# Titan 2 capability profile

Status: **N0_A16 planning / first Sable boot absent**

```text
DEVICE=titan2
SABLE_RELEASE_ID=TITAN2_N0_A16
LANE=TREBLE_PORTABILITY
FIRST_SUBSTRATE=AOSP16_CLEAN_GSI
FIRST_SABLE_ARTIFACT=ABSENT
FIRST_SABLE_BOOT=NOT_RUN
PUBLIC_BUILD_IMAGE=FAIL_CLOSED
PUBLIC_FLASH=NO
```

Titan 2 is the first keyboard-first MediaTek portability target, but this file
is still an architecture/evidence scaffold. It does not authorize a build,
artifact registration or flash path.

## Display profile

```text
main_display:
  class: SQUARE_KEYBOARD_LCD
  public_size: 4.5 inch
  public_resolution: 1440x1440
  aspect_class: square
  panel_power_class: lcd_or_lcd_like_until_verified
  aod_policy: NO_BY_DEFAULT
  pulse_policy: TEST_ONLY_REQUIRES_POWER_EVIDENCE
  layout_default: keyboard_first_square

rear_subscreen:
  class: SECONDARY_GLANCE_DISPLAY
  public_size: 2 inch class
  roles:
    - clock
    - notification_glance
    - quick_action
    - music_camera_app_shortcut_candidate
  privacy_default: restricted
  app_surface_policy: not_general_app_surface_until_validated
```

## Attention surfaces

```text
attention:
  aod: NOT_SUPPORTED_BY_DEFAULT
  pulse: CANDIDATE_TEST_ONLY
  rear_subscreen: CANDIDATE_PRIMARY_GLANCE_SURFACE
  keyboard_backlight: CANDIDATE
  haptics: REQUIRED_FALLBACK
  sound: REQUIRED_FALLBACK
  notification_led: UNKNOWN
```

Titan 2 should not use AOD as the default always-on attention surface. The rear
SubScreen is the candidate glance surface, but SableOS must validate display-id
stability, lockscreen privacy, brightness ownership, notification routing,
input/touch ownership and suspend/resume behavior before using it in any
release claim.

## Keyboard and pointer profile

```text
keyboard_profile: titan2
keyboard_class: physical_qwerty_touch_enabled
special_keys:
  - shift
  - sym
  - fn
  - alt
  - back
  - recents
  - programmable_side_keys
pointer_surface: keyboard_touch_surface_candidate
mouse_mode: candidate_requires_event_evidence
key_swap_policy: stock_support_observed_publicly_but_requires_sable_mapping
```

Titan 2 cannot share a profile with Titan 2 Elite or Q27. The physical layout,
modifier semantics, stock intelligent-assistance settings, touch surface,
side-key policy and SubScreen coupling are device-specific.

## Critical text-entry gates

These are release blockers.

```text
TITAN2_BT_PAIRING_TEXT_ENTRY=REQUIRED
TITAN2_ALT_ENTRY_SYSTEM_DIALOG=REQUIRED
TITAN2_SYM_ENTRY_SYSTEM_DIALOG=REQUIRED
TITAN2_SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
TITAN2_SETUP_WIZARD_TEXT_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_TEXT_ENTRY=REQUIRED
TITAN2_WIFI_PASSWORD_TEXT_ENTRY=REQUIRED
TITAN2_ACCOUNT_SIGNIN_TEXT_ENTRY=REQUIRED
```

Observed problem to avoid: during stock research, Bluetooth keyboard pairing
could require entering a key/passcode while the hardware keyboard could not
reliably provide regular Alt/SYM entry and the software keyboard was not usable
as fallback. SableOS must guarantee a text-entry path in pairing, setup,
lockscreen and other critical system dialogs.

## Layout acceptance

```text
sable_start:
  default: keyboard_first_square
  command: required
  visible_focus: required

sable_hub:
  default: dense_keyboard_list_or_square_cards
  provider_boundary: required
  subscreen_privacy: required

sable_settings:
  default: list_first_square
  search: required

sable_media:
  square_safe_controls: required

sable_reader:
  square_text_layout: required
  line_page_keyboard_scroll: required
```

## Open evidence requirements

```text
DISPLAY_ID_MAIN=UNKNOWN_PUBLIC
DISPLAY_ID_SUBSCREEN=UNKNOWN_PUBLIC
DOZE_CONFIG=UNKNOWN_PUBLIC
SUBSCREEN_PRIVACY_BEHAVIOR=UNKNOWN_PUBLIC
KEYBOARD_EVENT_MAP=SABLE_RESEARCH_REQUIRED
KEYBOARD_TOUCH_EVENT_MAP=SABLE_RESEARCH_REQUIRED
SOFTWARE_KEYBOARD_FORCE_SHOW=SABLE_VALIDATION_REQUIRED
AOD_POWER_SAFE=NO_CLAIM
```

## Release posture

```text
TITAN2_N0_A16_PROFILE=PLANNING_READY
TITAN2_N0_A16_BUILD_READY=NO
TITAN2_N0_A16_FLASH_READY=NO
```
