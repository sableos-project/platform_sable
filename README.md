# Sable platform

![Local CI](https://img.shields.io/badge/CI-local%20direct-active-2ea44f)
![R9 Launcher](https://img.shields.io/badge/R9%20launcher%20visual-PASS-2ea44f)
![Fresh Panther](https://img.shields.io/badge/fresh%20Panther%20build-IN%20PROGRESS-f0ad4e)
![Pixel 7](https://img.shields.io/badge/Pixel%207%20physical-PENDING-lightgrey)
![Titan 2](https://img.shields.io/badge/Titan%202-keyboard--first%20QUEUED-6f42c1)

## Current R9 architecture status

R8 established the shared Sable design and first-party application foundation. R9 is the current release milestone: Sable Start runs as a Launcher3/Quickstep-hosted presentation, the visual contract is accepted, and fresh Panther/full-device qualification is underway. Titan 2 follows as a keyboard-first portability target after Panther acceptance.

CI/build qualification is now local-direct on the controlled build machine; GitHub-hosted Actions are not the authoritative build path.

Common SableOS semantic contracts, shared design/application architecture, security-quality requirements, release/support policy and bounded Android adapter guidance.

This repository sits between Sable-owned application behavior and product/device integration. It must not become a dumping ground for device-specific compatibility code, application implementation source or opaque prebuilt APKs.

## Current architecture documents

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- [`docs/PORTABILITY_RULES.md`](docs/PORTABILITY_RULES.md)
- [`docs/DEVICE_SUPPORT_LEVELS.md`](docs/DEVICE_SUPPORT_LEVELS.md)
- [`docs/RELEASE_MODEL.md`](docs/RELEASE_MODEL.md)
- [`docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md`](docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md)
- [`docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`](docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md)
- [`docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md`](docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md)
- [`docs/KEYBOARD_DEVICE_TOOLS.md`](docs/KEYBOARD_DEVICE_TOOLS.md)

Organization-wide direction is maintained in `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md`, the requirements index and `sableos-project/.github/docs/SECURITY_QUALITY_ENGINEERING.md`.

## Current product architecture

The validated Panther baseline is the reference for the next source tranche. Current direction is:

```text
R8-A    shared Sable design/test/accessibility/localization contract
R8-B    Sable Calculator: Standard + Scientific + offline conversion
R8-C1   Sable Sudoku
R8-C2   Sable Mines
R8-C3   Sable 2048
R8-D    Reader publication path via Vaachak Mobile / Readium
R8-D2   Reader TXT/share/TTS/OCR path via Vaachak Text Reader
R8-E    Media: local Music + Internet Radio / Zune-Metro influenced UX
R8-SHELL shared system/application design foundation
R9-L     Launcher3/Quickstep HOME + Sable Start presentation
R9-P     fresh Panther build + physical runtime closure
```

A separate Sable Convert APK is no longer the preferred direction; conversion is intended to become part of Calculator. The three game modes are intended to become separate polished applications rather than one diagnostic-style combined Games surface.

Execution remains:

```text
local direct CI / source qualification
 -> trusted standalone app build on ai-g732
 -> exact trusted application freeze
 -> Android/Soong product integration proof
 -> fresh source-bound Panther build
 -> physical Panther R9 acceptance
 -> Titan 2 keyboard-first portability qualification
 -> later production-signing workstream
```

The Android product tree is an integration environment, not the everyday compiler for independent Rust/Kotlin apps.

## Security and engineering assurance

Shared platform/application architecture is constrained by the organization-wide assurance policy rather than defining ad-hoc security rules per app.

Key requirements include:

- OWASP MASVS/MASTG-aligned mobile-security evidence where applicable;
- least privilege and explicit permission/AppOps/exported-component/network/data-flow review;
- `RUST_BY_RISK, NOT_RUST_BY_BRANDING`;
- narrow typed JNI/FFI boundaries with explicit validation, ownership and failure contracts;
- layered static analysis, dependency/advisory checks, source coverage, property/fuzz testing and runtime/device tests;
- action/toolchain/dependency provenance and trusted-artifact sealing;
- measured performance rather than assumptions based on implementation language;
- accessibility, localization readiness and privacy requirements treated as correctness constraints rather than post-design polish.

The canonical detailed policy is `sableos-project/.github/docs/SECURITY_QUALITY_ENGINEERING.md`. A planned control remains documented as planned until CI/build evidence proves enforcement.

## Rust/Kotlin ownership rule

Rust is preferred for deterministic/high-value domain logic where it materially improves memory safety, state correctness, fuzzability or reuse. Kotlin/Android remains the owner of Activity/service lifecycle, permissions, roles, accessibility, PackageManager/LauncherApps, providers, Media3/MediaSession and other framework-facing integration.

New native boundaries require explicit tests and review. Do not create broad JNI surfaces merely to increase the amount of Rust in the product.

## Performance contract

Platform/app requirements should identify performance dimensions that can materially regress and how they will be measured. Depending on the component this may include startup, frame/jank behavior, input latency, memory, CPU/I/O, power and focused Rust-domain benchmarks.

Panther and Titan 2 measurements are separate evidence. Thresholds should be established from representative baselines and ratcheted rather than invented before measurement.

## R8 design baseline

Sable's visual direction is evolving toward an original Metro-influenced design language using Sable-owned identity, logo-derived color roles, large typography, low-chrome information surfaces and consistent motion/hierarchy.

The design system must remain compatible with:

```text
Follow system / Light / Dark where applicable
shared semantic color roles
accessibility/readability
localization/pseudo-localization
RTL-safe layout where relevant
bounded motion
clear enabled/disabled/focus states
```

Visual influence does not authorize copying Microsoft proprietary assets, fonts or branding.

## Native/dual-target portability

R8 native libraries must be verified compatible with 16 KiB page-size systems using the pinned toolchain. The requirement is measured output compatibility, not one hard-coded linker flag for all environments.

Where compatible, Panther and Titan 2 should consume the same frozen common application artifacts and same common `vendor_sable` product composition. Device repositories own bounded target-specific adaptation only.

Titan 2 adds explicit physical-keyboard/navigation/focus/input and square-display layout validation. Secondary-display/program-key/FM features are not common R8 requirements unless separately approved.

## Reader composition

One Sable Reader product composes separately qualified capability sources:

- Vaachak Mobile / Readium: publication/EPUB/library path;
- Vaachak Text Reader: TXT/share/process-text/TTS/OCR capability.

Network/model-download behavior remains an explicit product/privacy gate.

## Application-source ownership

This repository owns shared contracts, not substantial application implementation source. Stable app source belongs in the private canonical application/source repository. `vendor_sable` owns common product inclusion of exact trusted inputs; device repos own only genuine target adaptation.

Source, dependency and test ownership must remain clear enough that CodeQL/static analysis, coverage, fuzzing, artifact provenance and later security updates can be traced to one canonical implementation rather than divergent copies.

## Build/signing direction

`ai-g732` is the intended trusted A2/B1/B2/B3 development builder after private-source/storage/runner/toolchain hardening closes.

Production AVB/OTA/application signing is deliberately deferred until repeatable Panther and Titan 2 development qualification is satisfactory. The ThinkPad P50 is only a future signing-host candidate; it is not yet `sable-signer-01`. OptiPlex is not part of the current signing plan.

Do not introduce new shared services, privileges, cross-app stores or target-specific behavior solely because they simplify one implementation. Shared platform additions require an actual cross-product semantic requirement plus security/test/performance evidence appropriate to the risk.
