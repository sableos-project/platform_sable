# Sable utility application model — historical R9 filename

Status: **application-model guidance retained for link/history stability; the original milestone assignment is superseded.**

This file was originally written when the plan was:

```text
R8 design/theme foundation
 -> R9 first Sable utility, beginning with Calculator
```

That sequencing is no longer current. The consolidated application plan now places Calculator/Convert, Games, Reader and Media in **R8**, with standalone qualification before a single deliberate product-integration tranche.

Use these current documents first:

- `SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`;
- `R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md`;
- `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md`;
- `sableos-project/.github/docs/RUST_APPLICATION_ARCHITECTURE.md`.

The filename remains because existing links/history refer to it.

## Application model that remains valid

The useful architectural principles from the original document remain current:

1. **Bounded applications first.** Prefer low-privilege, understandable utilities before taking ownership of high-integration privileged applications.
2. **Least privilege.** A simple offline app should not gain network, location, contacts, usage, accessibility-service or other sensitive authority without an explicit requirement.
3. **Android owns Android integration.** Kotlin/Compose remains the normal owner of lifecycle, permissions, accessibility, intents/providers and other framework APIs.
4. **Rust by risk.** Use Rust for deterministic/high-value domain logic where it materially improves correctness or reuse; do not introduce JNI simply to increase Rust percentage.
5. **Standalone qualification first.** Cargo/Gradle/application tests and static/security gates run before Panther product integration.
6. **Exact artifact provenance.** If the SableOS product consumes a standalone APK, freeze its exact source/workflow/package/permission/ABI/hash identity.
7. **Product adoption is separate.** A qualified APK is not automatically a product/default application.
8. **Shared design contract.** R8 applications consume R8-A semantic design behavior instead of each creating an independent theme store.
9. **Reuse proven code.** Prefer extraction/adaptation of suitable Rustmix/Vaachak/ESP-derived code to unnecessary rewrites.
10. **No Lua runtime in Sable applications.** Rustmix Lua/catalog/runtime architecture is not part of the current Android application model.

## Current R8 application set

```text
R8-B Calculator + Convert
R8-C Games: Sudoku / Minesweeper / 2048
R8-D Reader publication path
R8-D2 Reader text/accessibility path
R8-E Media: Music + Internet Radio
```

These workstreams may qualify independently but converge at one exact integration freeze.

## R9 now means

R9 is the next coherent productivity/application tranche after R8, not the first Calculator milestone.

Candidates may include Notes, Voice Notes, Flashcards, Calendar after provider/data policy, selected sensor-based games, future Sable Study/PDF workflows and selected Sable Start improvements.

Candidate status is not automatic implementation authorization. The chosen R9 set should again follow:

```text
standalone qualification
 -> exact source/artifact freeze
 -> bounded product integration
 -> one coherent image build
 -> one device campaign
```

Historical Git history preserves the original long-form R9 utility draft if its earlier wording is needed for audit.