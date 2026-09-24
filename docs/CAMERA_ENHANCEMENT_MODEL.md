# Sable Camera architecture

Status: **current cross-device camera direction — 2026-09-24**

Sable Camera is a common system-image workstream for keyboard-first devices.
Panther's frozen R9 image keeps its documented upstream/preprocessed Camera
exception and is not reopened merely to adopt this work.

## Architecture

```text
camera-core/
camera-capabilities/
device-profiles/
ui/
platform-integration/
```

Common code owns capture/session semantics, capability interpretation and Sable
presentation. Device profiles own physical topology, vendor quirks and any
proven privileged-camera requirement.

## Capability rule

Keep these facts separate:

```text
sensor capability
HAL capability
ordinary-app-visible capability
system/privileged-app capability
```

Do not infer hidden camera support from marketing specs, a different Titan
family device or a community camera port.

## N0 strategy

For Titan-family N0, preserve the stock vendor camera HAL/ISP. Start with normal
Camera2 capability and ordinary CAMERA permission.

A Sable Camera system app may receive `SYSTEM_CAMERA` only on a device where
physical evidence proves useful system-only cameras and negative third-party
discovery/access tests preserve the intended boundary.

## Keyboard-first UI

Support square/near-square layouts and keyboard operation for:

- focus/shutter;
- video start/stop;
- zoom;
- camera switch;
- exposure adjustment where supported;
- gallery/open-last-capture;
- settings/mode navigation.

Bindings are device-profile aware rather than globally hard-coded.

## Titan 2

Current Titan 2 research already proves useful ordinary Camera2 capability,
including rear/front capture, high-resolution JPEG and rear RAW/DNG. That is
sufficient to start common camera-core design without privileged-camera hacks.

## Titan 2 Elite

Repeat ordinary-app capability inventory independently. Community reports about
system-only tele/logical cameras are hypotheses until the retail device proves
them.

## Q27

Remain research-only until shipped hardware and current firmware are available.

## Non-goals

- redistributing proprietary Google Camera-derived APKs;
- copying opaque proprietary processing code;
- granting privileged camera authority as a convenience;
- one device's camera ID map becoming common product logic.
