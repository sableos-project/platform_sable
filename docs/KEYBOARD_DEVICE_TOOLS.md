# Keyboard-device Sable Tools

Status: **current keyboard-first tools architecture — 2026-09-24**

Keyboard-first SableOS devices should expose compact, keyboard-operable
diagnostics and bounded maintenance actions without turning the user-facing
Tools surface into a generic privileged mutation console.

## Targets

```text
Titan 2        active PORTABILITY / N0
Titan 2 Elite  independent PORTABILITY candidate
Q27            RESEARCH / future
```

Panther remains the frozen touch-first reference.

## Common capabilities

Read-only by default:

- device/build identity;
- display/input inventory;
- keyboard profile/status;
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
Fn/Sym/vendor keys
keyboard backlight
pointer/touch surface
display association
programmable keys
```

## Security boundary

Do not expose arbitrary raw memory/NVRAM/kernel/storage mutation through the
production Tools UI.

K1/K2 deployment functions remain engineering tooling, not end-user Tools
actions. Titan-family release artifact registration/flash stays blocked until
the corresponding device adapter is qualified.
