# Keyboard-device Sable Tools

Status: **current keyboard-first tools architecture — 2026-09-26**

Keyboard-first SableOS devices should expose compact, keyboard-operable
diagnostics and bounded maintenance actions without turning the user-facing
Tools surface into a generic privileged mutation console.

## Targets

```text
Titan 2        PORTABILITY / TITAN2_N0_A16 planning
Titan 2 Elite  independent PORTABILITY candidate
Q27            RESEARCH / future
```

Panther remains the frozen touch-first reference.

## Common capabilities

Read-only by default:

- device/build identity;
- display/input inventory;
- panel/AOD/attention-surface inventory;
- keyboard profile/status;
- pointer/touch-surface profile/status;
- software-keyboard fallback status;
- critical text-entry gate status;
- camera capability report;
- network/telephony status summaries;
- artifact/build provenance;
- exportable diagnostics.

Maintenance actions must route through the Android/Sable owner of the capability
and require explicit authorization.

## Keyboard-first interaction

Tools must support:

- deterministic visible focus;
- type-to-filter/search;
- arrows/D-pad navigation;
- Enter activation;
- Back/Escape;
- keyboard-only export/report flows;
- touch as secondary input.

## Device adapter boundary

Device repositories may adapt physical keyboard, display, radio, partition,
firmware or vendor-service evidence. They must not fork the common Tools app.

Physical input differences belong in a device profile:

```text
identity
input devices
scan/keycode mapping
.kl/.kcm/.idc ownership
Fn/Sym/Alt/Ctrl/vendor keys
keyboard backlight
pointer/touch surface
mouse mode / capacitive keyboard mode
display association
programmable keys
critical text-entry fallback
```

## Critical text-entry diagnostics

Tools should eventually expose read-only status for:

```text
Bluetooth pairing text entry
Wi-Fi password text entry
Setup Wizard text entry
Lockscreen text entry
Alt/SYM/numeric/symbol entry
software keyboard fallback
input method switcher accessibility
```

A Tools report may record PASS/FAIL/UNKNOWN. It must not silently promote a
device or authorize flashing.

## Attention-surface diagnostics

Tools should distinguish:

```text
AOD
pulse
rear SubScreen / secondary display
LED
keyboard backlight
haptics
sound
```

Titan 2, Titan 2 Elite and Q27 must not share one AOD or attention policy.

## Security boundary

Do not expose arbitrary raw memory/NVRAM/kernel/storage mutation through the
production Tools UI.

K1/K2 deployment functions remain engineering tooling, not end-user Tools
actions. Titan-family release artifact registration/flash stays blocked until
the corresponding device adapter is qualified.
