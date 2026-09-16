# Common Sable platform architecture

Status: **normative common product/application architecture.**

The platform layer sits between Sable applications/product semantics and substrate/device/product integration. It defines stable shared behavior; it does not own substantial application source, product package lists, or device-specific hardware policy.

## Layering

```text
Sable application source/workspaces
    UI/domain logic, app tests, standalone build/dependency graphs
            |
            | consume shared Sable contracts
            v
platform_sable
    semantic design/application contracts
    shared typed policy/models
    bounded Android-version adapters only when cross-app reuse requires them
            |
            v
Android / validated substrate APIs
            |
            v
vendor_sable product composition
    + device_sable_<target> bounded target adapter
            |
            v
product image / device runtime
```

`platform_sable` must not become an application monorepo or a vendor/device catch-all.

## R8 application-development boundary

R8 introduces a deliberate split:

### Process A — standalone application qualification

Applications and portable Rust cores are compiled/tested through their canonical Cargo/Gradle/upstream workflows. This is where ordinary correctness, static/security, JNI/native packaging and APK qualification should happen.

### Process B — SableOS integration

Only exact frozen qualified inputs reach product integration. The product layer then proves import/module semantics, selection, install path, target-files/image membership and runtime behavior.

A shared platform contract may be source-built inside Android when that is the correct architecture. A dependency-heavy ordinary Android app does not need its entire Gradle/Maven graph recreated in Soong merely to be part of SableOS; a sealed prebuilt path is allowed only after its exact semantics/provenance are proved.

## R8-A shared design contract

The shared visual/customization contract is application-neutral and typed. First-R8 user-facing choices are:

```text
Follow system
Light
Dark
bounded accent
reset/default
```

The platform contract owns semantic roles and behavior, not arbitrary per-app literal colors or independent preference schemas.

Sable Start, Calculator/Convert, Games, Reader and Media should consume the same concepts while retaining app-specific presentation.

## Application ownership

Substantial applications belong in their application repository/workspace once their stable source boundary is known.

This repository may define:

- shared semantic models;
- design tokens/contracts;
- cross-app preference/schema contracts that genuinely need common ownership;
- bounded Android-version adapters reused by multiple Sable components;
- application architecture requirements and interoperability contracts.

It should not contain:

- copied Vaachak source;
- complete Calculator/Games/Media implementations merely to avoid a repo decision;
- product APK blobs;
- Panther-specific makefiles/HAL/vendor logic;
- duplicated framework plumbing.

## Rust / Kotlin boundary

Common architecture follows the organization Rust policy:

```text
Kotlin / Android
    lifecycle
    accessibility
    permissions
    intents/providers
    CameraX
    Media3/MediaSession
    Readium Android integration
    platform storage/network APIs
        |
        | narrow typed FFI only where justified
        v
Rust
    deterministic domain logic
    parsers
    state/rule engines
    validation
    selected high-risk transformations
```

A Rust crate plus a compiling Kotlin shell is not end-to-end proof. Rust-backed Android applications need native target builds, ABI/package inspection and representative Kotlin->JNI->Rust tests before integration freeze.

## Reader architecture

Sable Reader should remain one product while reusing two separately qualified capability sources:

```text
Vaachak Mobile / Readium
    EPUB/publication/library/reader path

Vaachak Text Reader
    TXT/share/process-text/TTS/OCR path
```

The eventual Sable-owned composition/adaptation must keep these source/provenance paths visible. Network/model acquisition is explicitly gated rather than inherited accidentally.

## Media architecture

Android owns playback/session/storage/network behavior. Portable Rust media-domain work may parse station lists, validate URLs, probe simple formats or own deterministic state, but embedded ESP playback/task/I2S architecture does not cross the boundary.

## Device portability

Panther, future legacy/portability targets, and other device families consume the same shared Sable semantics where supported.

Device repositories may adapt capability; they must not redefine common application/design semantics to fit one target.

## Product integration

`vendor_sable` owns common package/integration selection. `device_sable_<target>` owns bounded target adaptation. Generated upstream/substrate product files are inputs, not owners of Sable semantics.

For sealed APK inputs, exact product integration must prove the current Android-tree mechanism rather than assuming `android_app_import` or any other module type has the required signing/partition/native-library behavior.

## Validation

Use layered claims:

```text
shared contract tests
 -> app/domain tests
 -> standalone APK/native qualification
 -> exact artifact freeze
 -> product wiring
 -> image membership
 -> runtime behavior
```

No earlier layer implies a later one.