# Keyboard-device capability matrix

Status: **planning matrix / evidence scaffold**

This matrix translates the profile-first architecture into per-device capability
records for the current keyboard-first portability candidates.

It is not a release qualification record. Public vendor pages and press reports
are useful planning inputs, but SableOS release claims require device-owned
evidence from controlled hardware, stock firmware binding, and first-Sable
runtime validation.

## Evidence classes

```text
OBSERVED_RESEARCH
    Recorded in SableOS/Titan research artifacts or public Sable device docs.

PUBLIC_SPEC
    Vendor page, manual, tutorial, crowdfunding page, or public reporting.
    Useful for planning; not accepted as release evidence by itself.

CANDIDATE
    Architecture assumption that must be proven on hardware.

UNKNOWN
    Not yet known; no inherited claim allowed.

BLOCKER
    Must be resolved before a user-facing image for that device class.
```

## Device rows

| Capability | Titan 2 | Titan 2 Elite | Zinwa Q27 |
| --- | --- | --- | --- |
| Sable lane | `TREBLE_PORTABILITY / N0_A16` | `TREBLE_PORTABILITY / FUTURE` | `TREBLE_PORTABILITY / RESEARCH` |
| Baseline status | Titan 2 research handoff exists; first Sable boot absent | Independent baseline required | Independent baseline required; current public info is not enough |
| Main display class | `SQUARE_KEYBOARD_LCD` | `COMPACT_AMOLED_KEYBOARD` candidate | `COMPACT_AMOLED_KEYBOARD` candidate |
| Main display geometry | 4.5-inch, 1440 × 1440 public spec | 4.03-inch, 1080 × 1200 public spec | about 3.92–4.0-inch, 1080 × 1240 public reports |
| Secondary display | rear SubScreen present | no Sable claim yet | no Sable claim yet |
| AOD policy | `NO_BY_DEFAULT` | `CANDIDATE_REQUIRES_VALIDATION` | `CANDIDATE_REQUIRES_VALIDATION` |
| Pulse/ambient policy | test-only until power/display evidence | candidate | candidate |
| Attention surfaces | SubScreen, sound, vibration, keyboard backlight candidate | AOD/pulse candidate, sound, vibration, keyboard backlight candidate | AOD/pulse candidate, sound, vibration, keyboard/LED candidates unknown |
| Keyboard class | Titan 2 touch-enabled QWERTY profile | Elite compact QWERTY/touchpad profile candidate | Q27 QWERTY profile candidate |
| Pointer/touch keyboard | present on Titan 2 keyboard per public guide; exact event path requires evidence | mouse/touchpad mode candidate per public spec | unknown until hardware baseline |
| Critical text entry | **BLOCKER until pairing/setup/lockscreen gates pass** | independent blocker | independent blocker |
| Software keyboard fallback | required | required | required |
| Device profile inheritance | cannot inherit Elite/Q27 | cannot inherit Titan 2 | cannot inherit Titan 2 |
| First build relevance | highest; first N0_A16 target | later | later |

## Matrix decisions

```text
ONE_KEYBOARD_PHONE_PROFILE=NO
PER_DEVICE_HARDWARE_PROFILE=REQUIRED
PER_DEVICE_DISPLAY_PROFILE=REQUIRED
PER_DEVICE_KEYBOARD_PROFILE=REQUIRED
PER_DEVICE_POINTER_PROFILE=REQUIRED
PER_DEVICE_CRITICAL_TEXT_ENTRY_GATE=REQUIRED
```

## Release blockers before any user-facing image

```text
CRITICAL_TEXT_ENTRY=PASS
SOFTWARE_KEYBOARD_FALLBACK=PASS
ALT_SYM_SYSTEM_DIALOG_ENTRY=PASS
BLUETOOTH_PAIRING_TEXT_ENTRY=PASS
SETUP_WIZARD_TEXT_ENTRY=PASS
LOCKSCREEN_TEXT_ENTRY=PASS
DISPLAY_GEOMETRY_ACCEPTANCE=PASS
ATTENTION_SURFACE_POLICY=PASS
AOD_POWER_POLICY=PASS_OR_NOT_SUPPORTED
```

A device may pass with `AOD=NOT_SUPPORTED`. It may not pass by enabling AOD
without panel, power, doze, burn-in, privacy and idle-drain evidence.

## Source notes

- Titan 2 public product pages describe a 4.5-inch 1440 × 1440 main display and a rear secondary display.
- Unihertz Titan 2 SubScreen documentation describes clock, notification, quick action, music/camera/app shortcut behavior.
- Unihertz Titan 2 keyboard documentation describes a touch-enabled keyboard surface, special Shift/Sym/Fn/Alt keys and user-configurable key swaps.
- Titan 2 Elite public product pages describe a 4.03-inch 1080 × 1200 AMOLED display, 120Hz refresh rate, mouse/touchpad mode, flick typing and keyboard backlight behavior.
- Zinwa Q27 public information is still treated as research input only; independent Sable evidence is required.

## Per-device records

- [Titan 2](TITAN2.md)
- [Titan 2 Elite](TITAN2_ELITE.md)
- [Zinwa Q27](ZINWA_Q27.md)
