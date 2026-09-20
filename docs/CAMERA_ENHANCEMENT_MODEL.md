# Sable camera enhancement model

Status: **current cross-device camera architecture direction.**

Targets:

```text
Unihertz Titan 2
Unihertz Titan 2 Elite
Zinwa Q27
```

## Baseline

Prefer the open-source GrapheneOS Camera / CameraX architecture as the Sable camera application baseline. Keep the stock vendor camera HAL/ISP during GSI work.

Community GCam/LMC/SGCam ports are useful behavioral references for quality and device quirks, but SableOS does not redistribute proprietary Google Camera-derived APKs or copy opaque proprietary processing code.

## Required capability-first design

Probe each physical device for Camera2/CameraX capabilities before deciding features:

- public/system camera IDs;
- logical/physical camera membership;
- stream resolutions and FPS;
- RAW/manual support;
- AF/AE/AWB;
- OIS/EIS/video stabilization;
- dynamic-range/color-space profiles;
- CameraX vendor extensions;
- maximum-resolution modes and vendor tags.

Common UI/domain code consumes capability data rather than device-name conditionals.

## Square/near-square requirements

Titan 2 (1440x1440), Titan 2 Elite (1080x1200) and Q27 (1080x1240) require explicit camera geometry qualification:

- no overlapping controls;
- accurate preview transform;
- accurate tap-to-focus coordinates;
- 1:1 composition where supported;
- truthful 4:3/video framing;
- high-resolution preview where stable;
- keyboard shutter/focus/zoom where useful.

## Titan 2 Elite system-camera opportunity

The reviewed community GCam adaptation reports that the Elite physical 2x camera is exposed as Android `SYSTEM_CAMERA`, which blocks ordinary third-party camera apps.

Android supports system camera devices for system/privileged apps holding both normal `CAMERA` and `android.permission.SYSTEM_CAMERA`. SableOS controls the system image, so a narrowly privileged Sable Camera may be able to use the real telephoto camera without rooting.

This remains a hypothesis until physical Camera2 metadata and capture tests prove it. The privilege must be device/product allowlisted and accompanied by negative third-party-access tests.

## Quality goals

GCam-inspired outcomes, implemented with open/licensable components and vendor HAL capability:

- reliable HDR/exposure fusion;
- low-light denoise;
- controlled sharpening/tone mapping;
- neutral skin/color rendering;
- motion-aware frame selection;
- super-resolution crop only where justified;
- stabilization only when it improves video;
- local QR/barcode scanning;
- RAW/manual modes where hardware supports them.

Do not use Google proprietary feature names such as HDR+ unless the implementation and licensing actually justify them.

## References

- GrapheneOS Camera: https://github.com/GrapheneOS/Camera
- Android system cameras: https://source.android.com/docs/core/camera/system-cameras
- Android GSI requirements: https://source.android.com/docs/core/tests/vts/gsi
- Titan 2 Elite community GCam adaptation: https://www.reddit.com/r/unihertz/comments/1vzxax6/gcam_port_for_the_unihertz_titan_2_elite_square/
