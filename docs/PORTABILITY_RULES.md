# Portability rules

Status: **current normative portability model — 2026-09-26**

## Core rule

One Sable product core, multiple hardware adapters, multiple interaction
profiles and multiple capability profiles.

## Common vs device-specific

Common source owns:

- application semantics and package identity;
- Sable design tokens and appearance;
- launcher inventory/search/peek/app-context semantics;
- Hub/People/Messages behavior;
- Camera core/capability model;
- Keyboard/IME text composition;
- command/hub/shortcut semantics;
- build evidence and artifact contracts.

Device adapters/profiles own:

- display geometry, panel type, refresh and power behavior;
- AOD/pulse/attention-surface capability;
- secondary display association and policy;
- physical-keyboard scan/keylayout/keycharacter behavior;
- Fn/Sym/Alt/Ctrl/vendor keys and keyboard backlight;
- touch surface, capacitive keyboard, trackpad and mouse mode behavior;
- camera device profile and proven privileged-camera requirement;
- boot/vendor/AVB/VINTF integration;
- telephony/IMS/device firmware;
- partition/flash/restore transport.

## Current roles

- **Panther / Pixel 7:** frozen accepted touch-first reference.
- **Titan 2:** active keyboard-first PORTABILITY / `TITAN2_N0_A16` planning target.
- **Titan 2 Elite:** independent keyboard-first PORTABILITY/N0 candidate.
- **Q27:** RESEARCH / future product candidate.
- **Bramble:** historical reference.

## Portability success condition

Where substrate compatibility permits:

```text
same qualified common application source/artifacts
+ same common product semantics
+ isolated target OUT_DIRs/artifact descriptors
+ bounded device adapters
+ interaction-profile-specific presentation
+ hardware/display/input/attention profiles
+ target-specific runtime acceptance
+ no common application fork
```

## Build/deployment portability

Build identity is:

```text
device class + release + source + toolchain + product inputs
```

A physical serial is required only for device-contact operations.

The shared deployment layer should own authorization, serial binding, artifact
hashes and evidence. The device adapter owns transport and partition semantics.

Do not generalize Panther's A/B `fastboot flashall` assumptions to MediaTek
devices until each Titan target proves its own bootloader/fastbootd/partition
contract.

## Keyboard-first portability

Required common behavior:

- deterministic visible focus;
- arrow/D-pad navigation;
- Enter/Space activation;
- Back/Escape;
- printable-key type-to-search;
- stable focus restoration;
- shortcuts/command palette;
- no focus traps;
- touch as a secondary path;
- display-profile-aware square/compact/touch layouts.

Required device-specific behavior:

- physical-keyboard identity and scan/keycode evidence;
- modifier behavior for Shift/Alt/Sym/Ctrl/Fn/vendor keys;
- software keyboard fallback;
- Bluetooth pairing and setup-critical text entry;
- pointer/touch-surface behavior where present;
- per-device keyboard and pointer profiles.

## Display and attention portability

AOD, pulse, rear display, LED, keyboard backlight, haptics and sound are separate
attention surfaces.

AOD is never inherited from device family membership:

```text
Titan 2       AOD_DEFAULT=NO
Titan 2 Elite AOD=CANDIDATE_REQUIRES_VALIDATION
Q27           AOD=CANDIDATE_REQUIRES_VALIDATION
```

## Camera portability

Keep four facts separate:

```text
sensor capability
HAL capability
ordinary-app-visible capability
system/privileged-app capability
```

Do not infer hidden tele/logical camera support from marketing specs or from a
different Titan-family device.

## Non-Pixel assurance

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

N0 preserves stock kernel/vendor/ODM/firmware where practical and proves Sable
userspace/UX compatibility. N1 adds owned device/product/VINTF/SELinux/vendor
integration. N2 additionally closes security/update/AVB/signing/recovery and
production runtime gates.

## Evidence inheritance rule

```text
Panther PASS != Titan 2 PASS
Titan 2 PASS != Titan 2 Elite PASS
Titan 2 PASS != Q27 PASS
Public specification != release evidence
```
