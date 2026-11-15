# SableOS editions: Android and Native (2026-11-15)

**Status: CURRENT / ADDS AN EDITION / DOES NOT CHANGE THE 2026-11-03 DEVICE DECISION**

## Decision

Effective 2026-11-15, SableOS has two editions that share one product idea and
one set of semantic contracts, but no runtime code:

| | SableOS (Android edition) | SableOS Native (Linux edition) |
|---|---|---|
| Base | LineageOS / AOSP | [Marathon OS](https://github.com/patrickjquinn/Marathon-Shell) on postmarketOS / Alpine, mainline Linux |
| Shell and UI | Android framework, Sable Start on Launcher3, Kotlin apps | Qt 6 / QML shell (Marathon), BB10-style Flow, Hub and Frames |
| System services | Android services and Sable app adapters | Rust daemons: `sable-hubd`, `sable-framesd`, `sable-inputd`, `sable-searchd`, `sable-settings` |
| Android apps | Native | Through Marathon's Deckard runtime |
| Telephony | Vendor RIL / IMS | ModemManager |
| Source | `sableos-project` Android repositories, `aimindseye/sableos`, `aimindseye/zinwaq25-sableos` | [`sableos-project/sableos-native`](https://github.com/sableos-project/sableos-native) only |

SableOS Native is a fork of Marathon OS (Apache-2.0). It keeps Marathon's
compositor, kernel, image pipeline and OTA, and merges Marathon's changes
regularly; SableOS work lives under `sable/` in that repository.

## What the editions share

The contracts in this repository are edition-neutral. SableOS Native implements
them in Rust and QML; the Android edition keeps implementing them in its own
adapters:

- `SABLE_HUB_PORTABILITY_CONTRACT.md` (Hub semantics, priority, privacy)
- `NORMALIZED_KEY_INPUT_CONTRACT.md`, `KEYBOARD_FIRST_DEVICE_PROFILE_MODEL.md`,
  `KEYBOARD_AND_POINTER_PROFILE_MODEL.md` (keyboard-first behaviour)
- `DISPLAY_AND_ATTENTION_PROFILE_MODEL.md`, `CRITICAL_TEXT_ENTRY_GATES.md`
- `PORTABILITY_RULES.md`, `DEVICE_SUPPORT_LEVELS.md`, `RELEASE_MODEL.md`

Android-only material here (Android adapters, AOSP integration boundaries,
Android app plans) does not apply to SableOS Native. Where a contract assumes an
Android API, the Native implementation records its equivalent in
`sableos-native/sable/docs/`.

## Device scope

| Edition | Device | Status |
|---|---|---|
| Android | Zinwa Q25 | Active (unchanged) |
| Android | Zinwa Q27 | Active (unchanged) |
| Native | Zinwa Q25 | Active: first Native target, on Marathon's Q25 platform |
| Native | Unihertz Titan 2 | Source-only portability assessment; no images, flashing or device work |
| Either | Titan 2 / Titan 2 Elite GSIs and custom ROMs | Stopped under the 2026-11-03 decision (unchanged) |

The 2026-11-03 decision stays in force. A SableOS Native image for the Titan 2
would be a custom OS for that device, so it is not authorised by this document;
only reading and comparing published sources (Marathon, Serlk's mainline
Titan 2 work) is. Any Titan 2 Native build or device work needs a separate,
explicit project-owner decision.

```text
DECISION_DATE=2026-11-15
SABLEOS_EDITIONS=ANDROID,NATIVE
SABLEOS_NATIVE_REPOSITORY=sableos-project/sableos-native
SABLEOS_NATIVE_BASE=MARATHON_OS_FORK
SABLEOS_NATIVE_ACTIVE_DEVICE=ZINWA_Q25
SABLEOS_NATIVE_TITAN2=SOURCE_ONLY_ASSESSMENT
DEVICE_PORTFOLIO_2026_11_03=UNCHANGED
NATIVE_RELEASE_AUTHORIZED=NO
```
