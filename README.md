# Sable platform

Common SableOS semantic contracts, shared design/application architecture, release/support policy and bounded Android adapter guidance.

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

Organization-wide direction is maintained in `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md` and the requirements index.

## Current R8 architecture

```text
R8-A  shared design/test contract
R8-B  Calculator + Convert
R8-C  Games: Sudoku / Minesweeper / 2048
R8-D  Reader publication path via Vaachak Mobile / Readium
R8-D2 Reader TXT/share/TTS/OCR path via Vaachak Text Reader
R8-E  Media: local Music + Internet Radio
```

Execution is now:

```text
A1 disposable GitHub qualification
 -> A2 trusted standalone app build on ai-g732
 -> exact trusted application freeze
 -> B1 pre-image Android/Soong integration proof
 -> B2 Panther development image/runtime acceptance
 -> B3 Titan 2 portability development image/runtime acceptance
 -> later production-signing workstream
```

The Android product tree is an integration environment, not the everyday compiler for independent Rust/Kotlin apps.

## R8-A design baseline

```text
Follow system
Light
Dark
bounded accent
reset/default
shared semantic roles
accessibility/readability requirements
```

A theme marketplace, icon packs, grid/density editors, user-selectable corner systems, wallpaper editors and unrelated launcher personalization are not first-R8 requirements unless the product contract changes explicitly.

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

This repository owns shared contracts, not substantial application implementation source. Stable app source belongs in app-owned repositories/workspaces. `vendor_sable` owns common product inclusion of exact trusted inputs; device repos own only genuine target adaptation.

## Historical filenames

`docs/R9_SABLE_UTILITY_APP_MODEL.md` and `docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md` predate the consolidated R8 plan. Their old milestone assignment is superseded: Calculator/Convert now belong to R8-B. Unresolved Calculator semantics remain unresolved until documented decisions close them.

## Build/signing direction

`ai-g732` is the intended trusted A2/B1/B2/B3 development builder after storage/source/toolchain migration closes.

Production AVB/OTA/application signing is deliberately deferred until Panther and Titan 2 development qualification is satisfactory. The ThinkPad P50 is only a future signing-host candidate; it is not yet `sable-signer-01`. OptiPlex is not part of the current signing plan.

Do not introduce new shared services, privileges, cross-app stores or target-specific behavior solely because they simplify one implementation. Shared platform additions require an actual cross-product semantic requirement.
