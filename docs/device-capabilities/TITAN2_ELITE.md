# Titan 2 Elite capability profile

Status: **future portability candidate / independent baseline required**

```text
DEVICE=titan2_elite
LANE=TREBLE_PORTABILITY
FIRST_SABLE_ARTIFACT=ABSENT
FIRST_SABLE_BOOT=NOT_RUN
PUBLIC_BUILD_IMAGE=FAIL_CLOSED
PUBLIC_FLASH=NO
PROFILE_INHERITS_TITAN2=NO
```

Titan 2 Elite must not inherit Titan 2 evidence. It can share common Sable
semantics and application behavior, but hardware, display, attention, keyboard,
pointer and critical-entry gates require independent validation.

## Display profile

```text
main_display:
  class: COMPACT_AMOLED_KEYBOARD
  public_size: 4.03 inch
  public_resolution: 1080x1200
  public_refresh: 120hz
  aspect_class: compact_near_square_portrait
  panel_power_class: amoled_candidate
  aod_policy: CANDIDATE_REQUIRES_VALIDATION
  pulse_policy: CANDIDATE_REQUIRES_VALIDATION
  layout_default: keyboard_first_compact_portrait

secondary_display:
  class: NONE_CLAIMED_BY_SABLE_UNTIL_VERIFIED
```

AOD is a candidate because the public display class is AMOLED, not because Sable
has validated doze, burn-in, panel low-power mode, idle drain, privacy behavior
or vendor SystemUI support. It remains disabled until evidence exists.

## Attention surfaces

```text
attention:
  aod: CANDIDATE_REQUIRES_VALIDATION
  pulse: CANDIDATE_REQUIRES_VALIDATION
  keyboard_backlight: CANDIDATE
  haptics: REQUIRED_FALLBACK
  sound: REQUIRED_FALLBACK
  notification_led: UNKNOWN
  secondary_display: NO_CLAIM
```

## Keyboard and pointer profile

```text
keyboard_profile: titan2_elite
keyboard_class: compact_physical_qwerty
public_keyboard_features:
  - classic phone-style qwerty layout
  - gesture controls candidate
  - flick typing candidate
  - mouse mode candidate
  - keyboard backlight candidate
  - universal shortcuts candidate
pointer_surface: keyboard_touchpad_candidate
mouse_mode: candidate_requires_event_evidence
```

Titan 2 Elite likely needs a different focus density, shortcut layout and
thumb-reach model than Titan 2. Its compact AMOLED display and keyboard/pointer
behavior make it closer to the BB Classic/Q-series design family than the Titan
2 square dual-screen family.

## Critical text-entry gates

These are independent release blockers.

```text
TITAN2_ELITE_BT_PAIRING_TEXT_ENTRY=REQUIRED
TITAN2_ELITE_ALT_ENTRY_SYSTEM_DIALOG=REQUIRED
TITAN2_ELITE_SYM_ENTRY_SYSTEM_DIALOG=REQUIRED
TITAN2_ELITE_SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
TITAN2_ELITE_SETUP_WIZARD_TEXT_ENTRY=REQUIRED
TITAN2_ELITE_LOCKSCREEN_TEXT_ENTRY=REQUIRED
TITAN2_ELITE_WIFI_PASSWORD_TEXT_ENTRY=REQUIRED
TITAN2_ELITE_ACCOUNT_SIGNIN_TEXT_ENTRY=REQUIRED
```

No Titan 2 critical-entry PASS may be copied to Titan 2 Elite.

## Layout acceptance

```text
sable_start:
  default: keyboard_first_compact_portrait
  command: required
  visible_focus: required

sable_hub:
  default: dense_keyboard_list
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
DISPLAY_ID_MAIN=UNKNOWN
DOZE_CONFIG=UNKNOWN
AOD_IDLE_DRAIN=UNKNOWN
BURN_IN_MITIGATION=UNKNOWN
KEYBOARD_EVENT_MAP=UNKNOWN
KEYBOARD_TOUCHPAD_EVENT_MAP=UNKNOWN
SOFTWARE_KEYBOARD_FORCE_SHOW=UNKNOWN
```

## Release posture

```text
TITAN2_ELITE_PROFILE=PLANNING_ONLY
TITAN2_ELITE_BUILD_READY=NO
TITAN2_ELITE_FLASH_READY=NO
```
