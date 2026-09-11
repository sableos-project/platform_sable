# Sable platform

Common SableOS semantic contracts, services, Android adapters, and shared product contracts.

This repository sits between Sable-owned applications/shell behavior and device/substrate integration. It must not become a dumping ground for device-specific compatibility code or unrelated application implementations.

## Current architecture documents

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — common platform layering and service pattern.
- [`docs/PORTABILITY_RULES.md`](docs/PORTABILITY_RULES.md) — common/device portability boundaries.
- [`docs/DEVICE_SUPPORT_LEVELS.md`](docs/DEVICE_SUPPORT_LEVELS.md) — target qualification semantics.
- [`docs/RELEASE_MODEL.md`](docs/RELEASE_MODEL.md) — semantic product releases versus exact build/device identity.

## Current development direction

Organization-wide product direction is defined in `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md`.

For upcoming common-platform work, the normative documents are:

- [`docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md`](docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md) — shared Sable design tokens, Follow system/Light/Dark behavior, accent abstraction, persistence, accessibility, and anti-drift rules.
- [`docs/R9_SABLE_UTILITY_APP_MODEL.md`](docs/R9_SABLE_UTILITY_APP_MODEL.md) — application ownership/permission/testing model and the first native Sable utility direction, beginning with Calculator.
- [`docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md`](docs/R9_CALCULATOR_REQUIREMENTS_DRAFT.md) — pre-code Calculator requirements scaffold with unresolved semantics explicitly marked `TBD` so they are decided in documentation rather than invented during implementation.

The current development order is intentionally daily-driver first:

```text
R6 Sable Start completeness
 -> R7 daily-driver phone qualification
 -> R8 shared design/customization foundation
 -> R9 first Sable utilities
```

Do not introduce a new common service, privileged API, theme store, or cross-app contract solely because it simplifies one implementation. Common platform additions need an actual cross-product semantic requirement.