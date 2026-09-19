# Sable application reuse and integration plan

Status: **R8 normative application-qualification, trusted-artifact and product-integration architecture.**

R8 separates ordinary application development from trusted artifact production and Android image integration. New applications are qualified first, rebuilt on the trusted development builder, frozen by exact identity, then integrated into Panther and Titan 2 development images.

`R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md` remains authoritative for first-R8 appearance: Follow system / Light / Dark, bounded accent choice, reset/default, shared semantic roles and accessibility. R8 does not authorize a general theme marketplace, density/grid editor, icon-pack system or user-selectable corner framework.

## 1. R8 execution pipeline

### A1 — disposable qualification

GitHub-hosted CI and developer machines provide fast feedback for:

```text
Rust fmt / Clippy / unit-property-fuzz tests
Rust dependency/advisory checks
Kotlin/JVM tests
Gradle Android builds
Android Lint/static analysis
Compose/emulator/instrumentation tests where useful
pinned Reader/Text Reader qualification
manifest/package/permission inspection
qualification APK hashes
```

A1 artifacts are qualification evidence, not automatically trusted product inputs.

### A2 — trusted standalone application build

`ai-g732` rebuilds the accepted source with pinned/recorded JDK, SDK/NDK, Rust and Gradle toolchains.

A2 records:

```text
exact source/upstream commits
lockfile/dependency provenance
trusted APK SHA-256
package/version/permissions/components
classes*.dex extracted-content SHA-256
lib/<abi>/*.so extracted-content SHA-256
native ABI inventory
16 KiB ELF compatibility
APK native-library ZIP alignment
```

A2 produces the artifact eligible for the R8 integration freeze.

### R8 application freeze

Only exact A2-qualified artifacts enter Android product integration. Workstreams may be deferred instead of forcing incomplete code into the image.

### B1 — pre-image Android integration

B1 runs on `ai-g732` against the exact Android 17 / GrapheneOS target tree.

```text
frozen trusted APK
 -> discover generated Soong/Ninja graph
 -> prove import/module input identity
 -> record signing/certificate behavior
 -> record JNI processing
 -> record dexpreopt/uses-library configuration
 -> build minimum import dependencies
 -> compare processed DEX/JNI content identity
 -> prove product selection
 -> prove PRODUCT_OUT install
```

`android_app_import` is the preferred candidate but its exact target-tree behavior must be empirically proved before becoming a normalized contract.

### B2 — Panther development image

After A1/A2/freeze/B1 close for the selected tranche, build one coherent Panther development image and run one bounded Panther device campaign.

### B3 — Titan 2 portability image

After Panther acceptance, integrate the same frozen common application artifacts into Titan 2 wherever compatible and run the portability/runtime campaign with an isolated target OUT_DIR.

The purpose is to prove the common application/product boundary survives a materially different SoC/input/display platform without a common app fork.

Production AVB/OTA/release signing is outside the R8 development critical path and is deferred until Panther and Titan 2 development qualification is satisfactory.

## 2. R8 workstreams

### R8-A — shared design/test foundation

- typed semantic roles;
- Follow system / Light / Dark;
- bounded accent selection;
- reset/default behavior;
- accessibility/readability expectations;
- shared deterministic state/test conventions.

### R8-B — Calculator + Convert

Calculator uses deterministic arithmetic/domain logic plus an Android/Compose shell. Interaction semantics that remain TBD must not be silently selected by the domain core.

Convert should reuse/refactor portable Rustmix Wave conversion logic where suitable. Android owns presentation/input.

### R8-C — Games

Initial games: Sudoku, Minesweeper and 2048. Rust may own deterministic game state/rules; Android owns presentation, touch/keyboard input, lifecycle and accessibility. No Lua/e-paper hardware runtime is part of Sable Games.

### R8-D — Reader publication path

Primary source: Vaachak Mobile / Readium. Reuse publication opening/rendering, bookshelf/database, progress, bookmarks, highlights, search, TOC, TTS and existing reader behavior where qualified.

Prefer a Sable product flavor/adaptation over creating another Android EPUB renderer.

### R8-D2 — Reader text/accessibility path

Primary source: Vaachak Text Reader. Qualify TXT, Android share/process-text, TTS/audio export and OCR as capability inputs to the same Sable Reader product.

Network/model-download behavior remains a separate explicit privacy/product gate.

### R8-F — Sable Hub / Messages

Sable Hub is promoted from later product planning into the R8 first-usable common application set.

Stable semantic model:

```text
ALL
MESSAGES
PEOPLE
SERVICES

provider class:
  NATIVE_DATA
  ANDROID_NOTIFICATION
  SUPPORTED_API
  WEB
  UNAVAILABLE
```

The initial Sable Messages surface uses Android-owned communication capability instead of replacing mature transport solely for branding.

Requirements:

- SMS conversation presentation over Android Telephony provider data;
- SMS compose/send through supported Android telephony APIs;
- ContactsProvider person identity;
- retain existing proven Android transport for MMS/RCS until Sable has complete default-handler coverage;
- isolated provider sessions for WhatsApp, Instagram, Facebook/Messenger and LinkedIn when web capability is the honest supported path;
- no provider password database owned by Sable;
- no private protocol/database scraping;
- provider web state is not reclassified as native/API state;
- shared Sable design/accessibility;
- shared interaction semantics so Panther and Titan 2 consume one app source.

The common architecture must allow Titan 2 to render the same conversation/service semantics with keyboard-first focus/search/compose behavior without a Titan-specific application fork.

### R8-E — Media

Local Music + Internet Radio. Reuse portable domain/parsing concepts where valuable. Android owns Media3/codec playback, MediaSession, audio focus/routing, lifecycle/background behavior, storage/document access and network behavior.

## 3. Rust/Kotlin/JNI boundary

Prefer:

```text
Kotlin / Compose
    Android lifecycle
    UI/accessibility
    permissions/intents/providers
    Media3/CameraX/Readium/platform APIs
            |
            | narrow typed JNI where justified
            v
Rust
    deterministic domain logic
    parsers/state machines
    validation
    reusable computation
```

JNI must remain narrow and tested. A Rust implementation is not automatically safer if it expands FFI/unsafe/privilege surface.

## 4. Native 16 KiB compatibility

Every R8 APK containing native libraries must be verified compatible with 16 KiB page-size systems.

The requirement is measured output compatibility, not one hard-coded linker flag for every NDK version.

Evidence includes:

```text
ELF PT_LOAD alignment >= 0x4000
APK ZIP alignment suitable for uncompressed native libraries
runtime page size measured on accepted devices
representative JNI execution
```

If the pinned NDK/toolchain already emits compliant binaries, verify them. If not, apply the required toolchain/linker configuration and verify the result.

## 5. Common product integration ownership

Common imported-module definitions and common `PRODUCT_PACKAGES` composition belong in `vendor_sable`.

Device products should inherit common Sable composition rather than each duplicating the application list.

```text
vendor_sable
    common imported modules
    common R8 product selection

Panther adapter
    Panther-only exceptions

Titan 2 adapter
    Titan-only exceptions
```

A device difference must not become a common application fork merely because it was first observed on one target.

## 6. Dual-target compatibility matrix

| Dimension | Panther | Titan 2 |
| --- | --- | --- |
| Android ABI | `arm64-v8a` | `arm64-v8a` |
| Rust target | `aarch64-linux-android` | `aarch64-linux-android` |
| 16 KiB native compatibility | required | required |
| common frozen app artifacts | baseline | same where compatible |
| common product composition | `vendor_sable` | `vendor_sable` |
| device adapter | Panther-specific | Titan-specific |
| OUT_DIR | isolated | isolated |
| runtime page size | measured | measured |
| touch | validate | validate |
| physical keyboard | baseline | explicit navigation/focus/input gate |
| square display | baseline | explicit layout gate |
| Reader OCR/TTS | capability gate | capability gate |
| Media3/audio | capability gate | capability gate |
| production signing | deferred | deferred |

Titan-specific secondary-display, programmable-key, FM-radio and similar vendor capabilities are not common R8 requirements unless separately documented.

R8 common-app portability closes only when both targets use the accepted common application source/artifacts and common product composition with bounded target adapters and no common application source fork.

## 7. Product/evidence claim ladder

Keep these separate:

```text
A1 source/application qualification
 -> A2 trusted artifact
 -> B1 Soong import processing
 -> product selection
 -> PRODUCT_OUT
 -> target-files
 -> filesystem image
 -> runtime package/JNI/user behavior
```

An earlier PASS never implies a later PASS.

Outer APK hashes may change during legitimate Soong container/signing processing. Therefore record both whole-file hashes and extracted DEX/JNI inner-content identities.

## 8. Build budget

```text
A1 CI                               many times
local developer preflight           many times
A2 trusted app build                as needed per accepted source freeze
B1 pre-image integration            bounded
B2 Panther full image               once per frozen tranche
Panther device campaign             once per accepted image
B3 Titan 2 portability image        once after Panther acceptance
Titan 2 device campaign             once per accepted portability image
```

A full image must not substitute for ordinary application correctness testing.

## 9. Licensing/provenance

First-party ownership does not erase third-party obligations. Inventory licenses/provenance for Readium/Vaachak dependencies, fonts/assets, datasets, Media3/Android dependencies and reused game/media assets.

## 10. Signing boundary

Development/test signing is allowed as required for functional engineering images.

Production application keys, AVB hierarchy, OTA signing, `sign_target_files_apks`, key custody/backup/recovery/rotation and release handoff are a later release-security workstream.

The ThinkPad P50 is a future signing-host candidate only after the Android build has migrated to `ai-g732` and both Panther and Titan 2 development qualification are satisfactory. It is not yet `sable-signer-01`.

## 11. R8 closure

R8 closes only when the selected app workstreams have accepted A2 artifacts, B1 integration is proven, Panther development-image/runtime acceptance passes, and Titan 2 portability acceptance passes within its stated boundary.

That still does not constitute a production signed release claim.
