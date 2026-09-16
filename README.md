# Sable platform

Common SableOS semantic contracts, shared design/application architecture, release/support policy, and bounded Android adapter guidance.

This repository sits between Sable-owned application behavior and product/device integration. It must not become a dumping ground for device-specific compatibility code, application implementation source, or opaque prebuilt APKs.

## Current architecture documents

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — common platform/application layering and ownership.
- [`docs/PORTABILITY_RULES.md`](docs/PORTABILITY_RULES.md) — common/device portability boundaries.
- [`docs/DEVICE_SUPPORT_LEVELS.md`](docs/DEVICE_SUPPORT_LEVELS.md) — target qualification semantics.
- [`docs/RELEASE_MODEL.md`](docs/RELEASE_MODEL.md) — semantic product releases versus exact source/artifact/device identity.
- [`docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md`](docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md) — **R8-A** shared design contract.
- [`docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`](docs/SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md) — detailed R8 application qualification/integration architecture.
- [`docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md`](docs/SABLE_READER_TEXT_ACCESSIBILITY_CAPABILITY.md) — **R8-D2** Reader TXT/share/TTS/OCR capability contract.

Organization-wide current direction is maintained in `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md` and the requirements index.

## Current R8 architecture

R8 is a consolidated application foundation, not a design-only milestone.

```text
R8-A  shared Sable design/test contract
R8-B  Calculator + Convert
R8-C  Games: Sudoku / Minesweeper / 2048
R8-D  Reader publication path via Vaachak Mobile / Readium
R8-D2 Reader text/accessibility path via Vaachak Text Reader
R8-E  Media: local Music + Internet Radio
```

These workstreams are qualified independently before one deliberate product integration freeze.

```text
standalone Rust/Kotlin/Gradle/upstream qualification
        |
        v
exact application artifact/source freeze
        |
        v
prove Android product integration
        |
        v
one Panther image build
        |
        v
one bounded Panther device campaign
```

The Panther/AOSP tree is an integration environment, not the everyday compiler for independently developed applications.

## R8-A design baseline

The first R8 design contract remains deliberately bounded:

```text
Follow system
Light
Dark
bounded accent
reset/default
shared semantic roles
accessibility/readability requirements
```

A theme marketplace, icon packs, grid/density editors, user-selectable corner systems, wallpaper editors and unrelated launcher personalization are not first-R8 requirements unless the product contract is changed explicitly.

## Reader composition

Sable Reader is one product identity composed from separately qualified capability sources where useful:

- Vaachak Mobile / Readium: publication/EPUB/library path;
- Vaachak Text Reader: TXT/share/process-text/TTS/OCR capability.

Qualification against two upstream repositories does not authorize two competing Sable Reader launcher apps.

Network/model-download behavior remains an explicit product/privacy gate.

## Application-source ownership

This repository owns the shared contract, not substantial application implementation source.

When an application's stable Sable-owned source boundary is established, it should live in an application-owned repository/workspace. `vendor_sable` later owns product inclusion of exact qualified inputs; device repositories own only genuine target-specific adaptation.

## Historical filenames

`docs/R9_SABLE_UTILITY_APP_MODEL.md` and `docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md` predate the consolidated R8 plan. Their filenames remain for link/history stability, but their old milestone assignment is superseded: Calculator/Convert now belong to **R8-B**. Any unresolved Calculator semantics in the requirements draft remain unresolved until documented decisions close them.

## Build-host direction

The next full R8 Panther product/image build is planned on the migrated `ai-g732` trusted builder environment after the storage/source/toolchain migration gate passes. Application qualification occurs before that build and must not consume the full-image budget.

Do not introduce new shared services, privileges, cross-app stores or device-specific behavior here solely because they simplify one implementation. Shared platform additions require an actual cross-product semantic requirement.