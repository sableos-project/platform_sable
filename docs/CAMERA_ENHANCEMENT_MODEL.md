# Sable Camera architecture

Status: **current cross-device camera direction — 2026-10-02**

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

## Keyboard-first UI — Camera Control Deck

The accepted keyboard-device interaction principle is:

> **The screen is the viewfinder. The physical keyboard is the camera control
> surface.**

Portrait remains supported. Landscape should minimize persistent touch chrome
and treat the keyboard as the primary control deck. Touch remains a complete
fallback. The app must not globally force landscape.

Required semantic actions include:

- shutter / video start-stop;
- autofocus;
- autofocus lock;
- auto-exposure lock;
- keyboard focus-point movement;
- zoom;
- camera switch;
- ISO / white balance / shutter / exposure adjustment where supported;
- RAW / RAW+JPEG candidate;
- compare bracket;
- gallery/open-last-capture;
- settings/mode navigation;
- return selected Pro parameter to Auto.

Manual parameters should use transient strips instead of permanent virtual
camera dials. A control legend must make the physical layout discoverable.

The first release should use a strong Sable default key map rather than an
arbitrary full remapping UI. Raw device key codes are adapted into semantic
camera actions instead of being scattered through capture logic.

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
