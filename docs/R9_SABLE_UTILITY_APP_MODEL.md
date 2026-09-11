# R9 — Sable utility application model

Status: **normative direction for the first Sable-owned utility applications.**

R9 begins after the daily-driver foundation and shared Sable design/theme contract are sufficiently stable. The first planned Sable-owned utility is Calculator.

The purpose of R9 is not to recreate the entire AOSP application suite. It is to establish a repeatable, low-privilege Sable application pattern and then add utilities deliberately.

## 1. Goals

R9 should prove that SableOS can deliver native applications that are:

- useful;
- small in authority;
- independently testable;
- portable across supported devices;
- consistent with the shared Sable design system;
- reproducibly built and versioned;
- accessible;
- maintainable without relying on device-specific forks.

## 2. Repository ownership

Each substantial Sable application should eventually have a clear canonical repository under `sableos-project`, using Android checkout paths appropriate to the platform manifest.

Do not place unrelated application source in:

- `device_sable_*`;
- `vendor_sable`;
- `platform_sable` merely for convenience.

`platform_sable` owns shared semantic/design contracts, not every application implementation.

When a new app repository is created, its README and requirements must identify:

- product purpose;
- Android checkout path;
- package name;
- permissions;
- common Sable dependencies;
- build module name;
- test strategy;
- milestone/release relationship;
- explicit non-goals.

## 3. Default permission policy

A new Sable utility begins with **no sensitive permissions**.

Add a permission only when a written user requirement needs it.

In particular:

- no network permission by default;
- no location permission by default;
- no contacts/phone/SMS permissions by default;
- no media/storage broad access by default;
- no privileged/system permission merely to simplify architecture.

Any privileged application status must have a separate security justification.

## 4. Application architecture

Prefer a conventional bounded application architecture:

```text
UI
 |
 v
application/domain logic
 |
 v
narrow Android adapters only where needed
```

Pure computation/business logic should be testable without Android framework state.

Do not route simple local utility operations through a privileged Sable system service unless there is an actual cross-process/platform requirement.

## 5. Shared Sable design dependency

After R8, Sable applications should consume the common Sable design/theme contract for:

- semantic colors;
- typography;
- spacing;
- shapes;
- common appearance mode/accent state where designed;
- reusable common components only when the component is truly common.

Do not copy Sable Start theme constants into Calculator.

## 6. Sable Calculator — first application

### 6.1 Product goal

Provide a fast, trustworthy basic calculator suitable for everyday arithmetic and capable of serving as the reference template for future Sable application development.

### 6.2 Initial functional scope

The first version should support at minimum:

- digits 0–9;
- decimal input;
- addition;
- subtraction;
- multiplication;
- division;
- sign change where the UI model includes it;
- percentage only if its semantics are explicitly defined before implementation;
- clear/reset;
- backspace/delete where the UI model includes direct expression editing;
- equals/evaluation;
- sensible display of negative values;
- deterministic handling of division by zero and invalid/incomplete expressions.

Do not silently add scientific functions, unit conversion, currency conversion, graphing, programming bases, cloud history sync, or AI features to the first milestone.

### 6.3 Expression semantics

Before implementation, the Calculator repository must decide and document one of these interaction models:

- immediate-execution/basic handheld calculator semantics; or
- expression-entry semantics with defined operator precedence.

Do not mix both models accidentally.

If expression-entry is chosen, define precedence/associativity for every supported operator and test it.

If immediate-execution is chosen, tests must capture that behavior so a later refactor does not accidentally switch semantics.

### 6.4 Numeric representation

The Calculator requirements must explicitly choose a numeric strategy appropriate to expected user behavior.

Do not rely on raw binary floating-point formatting without tests for ordinary decimal calculations.

At minimum document:

- internal number representation;
- precision/rounding policy;
- display precision;
- overflow/underflow behavior if relevant;
- trimming of trailing zeroes;
- maximum expression/input/display length;
- behavior for repeated operators/equals.

The first version should favor predictable everyday decimal behavior over arbitrary complexity.

### 6.5 State/history

The first version may omit history entirely.

If history is included, document:

- whether it persists across process restart;
- storage location;
- maximum retention;
- clear/delete behavior;
- whether any data leaves the device (default: no).

Do not add account/cloud sync in the first Calculator milestone.

### 6.6 Permissions/privacy

Sable Calculator should require no network, location, contacts, phone, SMS, microphone, camera, or media permission for basic operation.

Unexpected permission additions are release blockers until justified.

### 6.7 Accessibility

The first release must provide:

- accessible labels/roles for every key/control;
- logical traversal order;
- readable result/expression semantics;
- practical font scaling behavior;
- sufficient touch targets;
- usable light/dark/accent contrast;
- no operation requiring color perception alone.

### 6.8 Tests

Calculator should establish a strong deterministic test suite before device runtime polish.

At minimum include tests for:

- every supported binary operation;
- negative operands/results;
- zero;
- decimals;
- repeated operations according to chosen model;
- invalid/incomplete input;
- divide by zero;
- clear/reset;
- precision/rounding examples;
- boundary input/display limits;
- any percent/sign semantics included.

UI/runtime tests should prove representative button input, result display, rotation/window behavior as applicable, theme changes, and process restart behavior for any persisted state.

### 6.9 Build/release evidence

The application should have:

- exact source commit/tree identity;
- Soong module build proof;
- package identity/version metadata;
- artifact hash;
- manifest/permission audit;
- runtime smoke/interaction proof;
- revision-pinned manifest inclusion once integrated into a product build.

## 7. Later utility candidates

Candidate status is not authorization to implement. Each candidate requires its own requirements document first.

### Notes

Potential value:

- simple offline notes;
- clear local-data ownership;
- no account requirement.

Risks/decisions before work:

- storage format;
- backup/export;
- encryption expectations;
- search;
- rich text vs plain text;
- sharing intents.

### Clock

Potential value:

- consistent Sable UI for alarms/timers/stopwatch.

Risks/decisions before work:

- exact alarm permission/platform APIs;
- reboot persistence;
- doze/background behavior;
- alarm audio/volume/DND interaction;
- lock-screen behavior.

This is more platform-sensitive than Calculator and should not be treated as equally trivial.

### Files

Potential value:

- Sable-oriented file browsing/presentation.

Risks/decisions before work:

- Storage Access Framework/document provider semantics;
- removable/media storage;
- sharing/open-with;
- permissions;
- avoiding broad filesystem privilege.

A Sable Files app should use supported storage/document abstractions and must not become a reason to weaken Android storage isolation.

## 8. High-complexity applications are not R9 utility targets

The following require separate product/security programs, not casual extension of R9:

- Dialer/Phone;
- SMS/MMS/RCS Messaging;
- browser engine/application;
- camera stack/application;
- email client;
- maps/navigation stack.

## 9. Code reuse policy

Shared code belongs in `platform_sable` only when it represents a real cross-application product contract or reusable implementation boundary.

Do not promote ordinary app-local code into `platform_sable` after one use simply to call it shared.

Likewise, do not duplicate platform_sable contracts inside each app.

## 10. Dependency discipline

A Sable utility should keep dependencies small and auditable.

Before adding a large third-party library, document:

- why platform/Kotlin/Compose functionality is insufficient;
- license;
- update/security ownership;
- binary/build impact;
- whether the dependency is used by multiple Sable components.

Avoid dependencies that introduce network/services or analytics not required by the application.

## 11. Telemetry/analytics

No telemetry or analytics is assumed for Sable utility applications.

Adding analytics requires an explicit privacy/product decision; it must not appear as a default library side effect.

## 12. Definition of R9 done

The first R9 milestone is done when Sable Calculator:

- has a dedicated documented product contract;
- builds from its canonical Sable repository/source composition;
- implements the agreed basic arithmetic model;
- has deterministic logic tests;
- has no unjustified sensitive permissions;
- consumes the shared R8 Sable design/theme contract;
- passes accessibility/runtime smoke expectations;
- is integrated through `platform_manifest`/product composition with exact source identity;
- demonstrates a repeatable pattern usable by the next Sable utility without forcing that next utility to copy Calculator-specific architecture.

Only after that should the next utility be selected and specified.