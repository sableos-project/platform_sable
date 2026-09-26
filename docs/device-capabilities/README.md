# Device capability records

Status: **planning records / not release qualification**

This directory records per-device hardware, display, keyboard, pointer,
attention and critical text-entry capabilities for keyboard-first SableOS
portability targets.

Start with:

- [Keyboard-device capability matrix](DEVICE_CAPABILITY_MATRIX.md)
- [Titan 2](TITAN2.md)
- [Titan 2 Elite](TITAN2_ELITE.md)
- [Zinwa Q27](ZINWA_Q27.md)

## Rules

```text
PUBLIC_SPEC_IS_NOT_RELEASE_EVIDENCE=YES
PER_DEVICE_PROFILE_REQUIRED=YES
PROFILE_PASS_INHERITANCE=NO
AOD_REQUIRES_PANEL_POWER_DOZE_PRIVACY_EVIDENCE=YES
CRITICAL_TEXT_ENTRY_IS_A_RELEASE_BLOCKER=YES
```

Titan 2, Titan 2 Elite and Q27 share common Sable application semantics. They do
not share hardware acceptance, keyboard maps, display assumptions, AOD behavior,
attention surfaces or critical text-entry PASS evidence.
