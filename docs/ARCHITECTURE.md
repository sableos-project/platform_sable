# Common Sable platform architecture

The platform layer sits between Sable applications/product semantics and substrate/device integration.

```text
Sable applications / Sable Start
        |
        | stable Sable semantic + design contracts
        v
common Sable contracts and services
        |
        | bounded Android adapters
        v
Android framework / supported HAL interfaces
        |
        v
device/vendor implementation
```

The goal is not to hide Android completely. The goal is to keep user-facing/common Sable behavior stable while containing Android-version/device-specific mechanics behind narrow boundaries where such boundaries add real value.

## Design goals

- keep semantic APIs stable across Android releases and devices;
- keep framework hooks small and reviewable;
- preserve Android security boundaries such as Binder, SELinux, permissions, AppOps, PackageManager, roles, and supported HAL interfaces;
- avoid exposing raw device/vendor authority to presentation code;
- use stable AIDL/C/NDK boundaries where independent toolchains must interoperate;
- keep device-specific compatibility work out of common semantic code;
- keep application source in its owning app repository;
- establish common Sable design/customization contracts before multiple Sable apps invent incompatible styling/preferences;
- avoid privileged common services where ordinary application/framework APIs are sufficient.

## Repository/layer responsibility

### Application repositories

Examples: `packages_apps_SableStart` and future Sable utility repositories.

Own:

- application-specific UI;
- application/domain logic;
- app-specific Android adapters;
- package manifest/resources;
- app-specific requirements/tests.

Do not move ordinary app-local code into `platform_sable` solely to call it shared.

### `platform_sable`

Owns common cross-product contracts such as:

- stable semantic service/API contracts used by multiple Sable components;
- bounded Android adapters that genuinely isolate changing platform mechanics;
- shared Sable design/theme/customization contracts after R8;
- common typed data models where they represent product semantics rather than one application's implementation detail;
- release/support/portability architecture documentation.

### `vendor_sable`

Owns common Android product integration such as product package inclusion, common overlays, permissions integration, and default-package composition. It does not own application source.

### `device_sable_*`

Owns bounded target-specific integration/qualification. It must not fork common Sable application/platform semantics merely because a failure was first observed on that device.

### `platform_manifest`

Owns exact source composition/revision pinning. It encodes decided architecture; it should not be the place where ownership is accidentally decided by path collisions.

## Service pattern

Preferred pattern when a shared service is actually required:

```text
UI/app -> typed semantic request -> narrow Sable service -> Android framework/HAL
```

The service should expose the smallest authority required for the semantic operation.

Avoid:

```text
UI -> generic privileged command executor -> arbitrary system operation
```

A generic privileged bridge creates an authority boundary that is hard to reason about and undermines Android's existing security model.

## When NOT to create a Sable service

Do not add a common privileged service merely because:

- one application can call a supported Android API directly;
- a preference could be stored safely in ordinary app/product-owned storage;
- a device-specific workaround is still unproven as a common problem;
- a utility app needs only local computation;
- the service would save a few lines of code.

A new cross-process/common service should have a documented multi-component or platform-integration requirement, authority model, permission model, lifecycle, compatibility strategy, and tests.

## Android adapter boundary

Use an adapter when it provides one or more concrete benefits:

- isolates API-level change;
- normalizes substrate differences;
- centralizes permission/role/AppOps semantics that must remain consistent;
- converts raw platform data into stable Sable product semantics;
- enables deterministic testing of higher-level logic.

Do not wrap every Android method mechanically. Thin wrappers with no semantic boundary add indirection without portability value.

## Device-specific boundary

A target-specific adapter is appropriate when behavior genuinely depends on:

- hardware implementation;
- vendor/BSP/HAL difference;
- target-specific kernel/firmware behavior;
- platform/substrate difference that cannot remain in a common Android adapter.

Before moving code to a device repository, record evidence that the difference is target-specific.

A bug occurring only on the currently tested Panther device is not by itself proof that common code is correct or that the fix belongs in `device_sable_panther`.

## Application/default-app boundary

SableOS does not require a Sable-owned implementation of every user application.

The common platform should not depend on one specific Phone/Messaging/Browser/Camera implementation unless a real shared contract requires it.

Default application selection belongs to product composition policy (`vendor_sable`) plus the application's own source repository. See the organization-wide default-application/replacement policy.

## R6 relationship

Sable Start R6 application inventory/search/greeting behavior belongs primarily in `packages_apps_SableStart`.

A common platform API should be added for R6 only if supported Android launcher APIs cannot satisfy a documented common requirement directly and there is a clear cross-application/platform abstraction benefit.

Do not move launcher inventory logic into a privileged platform service merely because Sable Start is a system application.

## R7 relationship

R7 daily-driver capabilities such as Wi-Fi, cellular data, telephony framework, Android permissions, notifications, and Settings remain primarily platform/substrate responsibilities.

Sable may add bounded semantics/integration later, but R7 should first prove the validated substrate works rather than reimplement it.

## R8 relationship: design/customization

R8 introduces a common Sable design/customization contract. The common part belongs here because multiple Sable applications should consume the same semantics.

The contract should define semantic tokens and typed appearance settings, not screen-specific literal resources.

Read:

```text
docs/R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md
```

## R9 relationship: utility apps

The first planned native Sable utility is Calculator.

Calculator application source should live in its own canonical application repository once created. `platform_sable` may provide shared R8 theme contracts, but it should not absorb Calculator-specific arithmetic/UI logic.

Read:

```text
docs/R9_SABLE_UTILITY_APP_MODEL.md
```

## Stable API evolution

For a common Sable contract:

- make ownership explicit;
- keep data types narrow;
- document versioning/compatibility if used across independently evolving components;
- avoid leaking Android implementation classes unless the contract is intentionally Android-facing;
- add capability/feature detection rather than assuming every device/substrate supports every operation;
- fail safely when an optional capability is unavailable;
- keep privilege checks at the authority boundary.

## Rust/native boundary

Where independent Rust/native components are used:

- prefer stable generated AIDL/C/NDK boundaries where appropriate;
- avoid binding application semantics to unstable internal C++ symbols;
- document ownership/lifetime/threading/error behavior;
- do not move ordinary Kotlin application logic into native code without a concrete security/performance/portability reason.

## Privacy/security boundary

Common Sable services/adapters should request the least authority required.

Do not centralize sensitive data merely because a central service can access it. For each sensitive capability, document:

- data accessed;
- permission/role/AppOps required;
- which callers are authorized;
- whether data is persisted;
- whether data crosses process boundaries;
- error/denial behavior.

## Initial portability target

The same common contracts should be consumable by Panther/Android 17 and future Bramble/Android 16 or other qualified experiments without redesigning user-facing semantics.

This does not mean every capability is guaranteed on every device. Device support levels and capability availability remain explicit.

## Anti-drift rule

If an implementation requires a new common service, device adapter, privilege, or product contract that is not covered by current requirements, update architecture/requirements first.

Do not let a convenient code location become architecture by accident.