# Zinwa Q27 future portability notes

Status: **DEFERRED RESEARCH CONTEXT — not an R8 execution target.**

This document preserves current Q27 planning context so the project can revisit it later without diverting the active Panther / `ai-g732` R8 execution path.

## Current project understanding

As of 2026-09-16, project monitoring of Zinwa announcements indicates:

- Q27 is expected to remain on a stock Android 16 baseline rather than move to Android 17;
- Zinwa does not plan to release the full Android/device/vendor OS source tree;
- Zinwa plans to release kernel source to support community custom-ROM work;
- Lineage/community device enablement is therefore expected to be the practical upstream path for a future custom ROM;
- the project has access to Q27 OTA artifacts for possible future analysis;
- no Q27 OTA analysis is authorized or required for current R8 closure.

These are project planning inputs based on monitored vendor/community announcements. They are not yet a sealed SableOS Q27 bring-up evidence package.

## What kernel source does and does not prove

Kernel source is useful for GPL compliance, kernel configuration, driver investigation and future device enablement. It does **not** by itself provide:

```text
complete device tree
vendor HAL source
proprietary blob inventory
VINTF closure
RIL / IMS behavior
camera/audio/display HAL closure
SELinux policy closure
AVB/update layout understanding
production security/update lifecycle
```

Therefore:

```text
Q27_KERNEL_SOURCE_AVAILABLE
!= Q27_SABLEOS_PORT_READY
```

## Preferred future sequencing

The project should not independently reconstruct Q27 during R8 while Panther and Titan 2 are active priorities.

Preferred order:

```text
Panther B2 closure
    -> Titan 2 B3 MediaTek portability learning
    -> observe credible Lineage/community Q27 bring-up
    -> evaluate reusable device/vendor enablement
    -> decide whether to promote Q27 from RESEARCH to PORTABILITY
```

A Q27 platform port should be considered only after either:

1. a credible Lineage/community Q27 device tree and subsystem bring-up exists; or
2. Titan 2 work has produced enough reusable MediaTek enablement knowledge that independent Q27 work is justified.

Ideally both are true.

## Two-track future model

Sable application compatibility and SableOS platform support are separate claims.

```text
Track A — stock Q27 Android 16
    Sable application compatibility
    keyboard/focus behavior
    unusual display/layout behavior
    Reader/Media/application runtime

Track B — future custom-ROM Q27
    boot/device tree/vendor compatibility
    VINTF/HAL/blob closure
    SELinux
    telephony/IMS
    hardware subsystem acceptance
    update/security lifecycle
```

Track A may become useful before Track B and does not require SableOS to maintain a parallel Android 16 platform branch.

## Android-version policy

The Q27 vendor choice to remain on Android 16 does not justify downgrading the common SableOS platform or creating parallel Android 16/17 SableOS framework branches.

Current policy remains:

```text
SableOS common platform      modern single baseline
Panther                      PRIMARY Android 17 development target
Titan 2                      active R8 PORTABILITY target
Q27                          deferred RESEARCH / future PRODUCT_CANDIDATE
Sable apps on Android 16     compatible where practical through minSdk/API guards
```

A newer `targetSdk` does not itself require the device to run the same Android release; future app compatibility should be governed by `minSdk`, actual API use and runtime testing.

## Future Q27 feasibility evidence

If Q27 research is resumed, collect evidence for at least:

```text
bootloader unlock / flashing path
stock firmware / OTA identities
kernel source identity
community device tree identity
proprietary-files inventory and extraction path
partition / dynamic-partition layout
AVB behavior
VINTF / vendor interface compatibility
GKI/kernel compatibility
RIL / IMS / VoLTE
Wi-Fi / Bluetooth
camera
video / graphics / display
audio
physical keyboard / trackpad / input mappings
NFC
fingerprint
sensors
power / charging / thermal behavior
encryption
SELinux enforcing state
update / rollback behavior
```

Each subsystem remains a separate claim; a successful boot does not close the device.

## OTA artifact boundary

Q27 OTA artifacts are known to be available to the project, but their contents have not been analyzed for this planning update.

Do not infer from their availability that vendor source, Android 17 compatibility, device-tree completeness or SableOS port feasibility has been proven.

Any future OTA analysis should be separately authorized, hash/seal the exact input artifacts, preserve read-only provenance, and answer a bounded feasibility question rather than becoming open-ended reverse engineering.

## Current execution priority

This document must not change R8 execution order.

```text
CURRENT_PRIORITY=Panther + ai-g732

1. finish ai-g732 migration / trusted-host seal
2. execute R8 A2 trusted standalone app build
3. execute R8 B1 pre-image Soong/product integration
4. authorize and close Panther B2 development image/runtime
5. proceed to Titan 2 B3 portability
6. revisit Q27 only through a separate future decision
```

`Q27_DEFERRED_RESEARCH=YES`
`Q27_R8_IMAGE_TARGET=NO`
