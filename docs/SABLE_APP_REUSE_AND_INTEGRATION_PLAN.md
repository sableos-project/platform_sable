# Sable application reuse and integration plan

Status: **R8 normative application-qualification and product-integration architecture.**

R8 separates application development from the SableOS image build. New applications are qualified as Android applications first and become system-image components only after exact source and artifact identities are frozen.

This document broadens the R8 train without changing the existing R8 design-system contract. `R8_DESIGN_SYSTEM_AND_CUSTOMIZATION.md` remains authoritative for first-R8 appearance behavior: **Follow system / Light / Dark**, a bounded accent choice, reset, shared semantic roles, and no first-R8 density/grid/icon-shape/theme-pack expansion.

## 1. Two independent processes

### Process A — application qualification

This process must not require a Panther/AOSP full image build.

```text
Git source
   |
   +-- Rust domains
   |     cargo fmt --check
   |     cargo clippy -D warnings
   |     cargo test
   |
   +-- Android/Kotlin applications
   |     unit tests
   |     Android lint / static analysis
   |     debug or qualification APK build
   |
   +-- Reader
         pinned Vaachak source SHA
         core tests
         Sable flavor tests/lint/build

ALL REQUIRED GATES PASS
        |
        v
R8 application qualification freeze
        |
        +-- exact Git commit(s)
        +-- exact dependency inputs / lock state
        +-- exact APK SHA-256
        +-- permission/package inspection
```

The application pipeline may run repeatedly on developer machines and CI without consuming the product-image build budget.

### Process B — SableOS product integration

This process consumes only qualified, pinned application artifacts.

```text
qualified APKs + frozen identities
            |
            v
Sable product integration
            |
            v
prebuilt/application wiring proof
            |
            v
ONE normal Panther image build
            |
            v
artifact/package fidelity
            |
            v
ONE Device1 integration campaign
```

The Panther/AOSP tree is not the everyday compiler for independently developed applications.

## 2. R8 workstreams

R8 is one integration train with independently closable source workstreams.

### R8-A — shared Sable design and test foundation

Owns:

- typed semantic design roles;
- Follow system / Light / Dark appearance mode;
- bounded accent selection;
- shared accessibility/readability expectations;
- shared test conventions and deterministic state models.

The existing R8 design/customization requirements remain authoritative for this workstream.

### R8-B — Calculator + Convert

**Sable Calculator** uses a Rust arithmetic domain plus an Android/Compose shell.

The Calculator source gate must not silently select interaction behavior that remains TBD in the product requirements. Until the interaction contract is frozen, the Rust core may provide exact decimal parsing and checked `+`, `-`, multiply and divide primitives, but must not make expression precedence, parentheses, percent, repeated-equals, history, display precision, or final rounding policy normative.

**Sable Convert** should substantially reuse the Rustmix Wave hardware-independent fixed-point conversion domain. Android owns presentation/input; conversion arithmetic remains in the tested Rust domain.

### R8-C — Sable Games

Initial portable domains:

- Sudoku;
- Minesweeper;
- 2048.

Rust owns deterministic game state/rules. Android owns rendering, touch/input, lifecycle and accessibility. Embedded e-paper canvas code and Lua bridges are not Android dependencies.

### R8-D — Sable Reader

Primary source: **Vaachak Mobile**, pinned to an exact upstream commit for qualification.

Reuse substantially:

- Readium publication opening/rendering architecture;
- bookshelf/database;
- reading position/progress;
- bookmarks;
- highlights;
- search;
- table of contents;
- TTS;
- reader preferences;
- existing e-ink-oriented behavior and tests where applicable.

Preferred product structure is a **Sable Reader flavor**, not a copied/reimplemented reader engine. The Sable flavor uses its own application identity, branding, Sable design adapter and feature policy while retaining shared Vaachak reader/core implementation.

Initial Sable Reader policy is offline-first. Network-backed sync, BYOK AI, OPDS/Calibre and other network surfaces are not accepted merely because reusable upstream code exists. They require explicit product/privacy gates before image inclusion.

EPUB support may be claimed when source/build/runtime evidence proves it. TXT support must not be claimed until equivalent evidence exists.

Rustmix X4 reader code is reference material for reader concepts, data compatibility and tests, not a second Android rendering engine.

### R8-E — Sable Media

Sable Media contains:

- local Music;
- Internet Radio.

Reuse from the ESP32 Assistant domain where portable:

- station identity/model;
- text/CSV/M3U-style station-list parsing conventions;
- HTTP/HTTPS URL validation;
- WAV/MP3 header/probing concepts;
- deterministic control/state concepts.

Do **not** port the embedded playback backend:

- FreeRTOS;
- PSRAM StreamBuffer;
- ESP I2S;
- PCM5101-specific output;
- HELIX decoder integration;
- fixed embedded `/sdcard/AUDIO` assumptions.

Android owns Media3/codec playback, MediaSession, audio focus, Bluetooth/headset routing, lifecycle and background playback.

Local Music should use Android SAF/MediaStore-style user-granted access rather than broad storage authority. Internet Radio necessarily requires network access; that permission belongs to Sable Media, not to unrelated offline applications.

## 3. Reuse matrix

| Sable component | Primary source | Reuse directly / substantially | Extract/refactor | Android adapter | Do not port |
| --- | --- | --- | --- | --- | --- |
| Sable Start / shared design | existing Sable Start + platform contract | production launcher behavior, semantic design requirements | shared design tokens/state contract | Compose surfaces | per-app divergent palettes/stores |
| Sable Calculator | new Sable code | exact arithmetic tests/domain | interaction-neutral Rust core | Compose shell and later product interaction policy | premature precedence/percent/history semantics |
| Sable Convert | Rustmix Wave | fixed-point conversion model | portable Rust crate | Compose UI/binding | embedded UI/hardware glue |
| Sable Games | Rustmix Wave | Sudoku, Minesweeper, 2048 rules | portable deterministic Rust crates | Compose rendering/input | Lua/e-paper hardware bridge |
| Sable Reader | Vaachak Mobile | Readium/core/reader architecture | Sable flavor + bounded product policy | Sable branding/theme/product integration | second reader engine; unapproved network surfaces |
| Sable Media | ESP32 Assistant | station formats/models and probe concepts | platform-neutral Rust media domain | Media3 playback/service + SAF/MediaStore | FreeRTOS/I2S/PCM5101/HELIX/PSRAM backend |

## 4. Qualification gates

### Rust correctness gate

Required for every R8 Rust workspace change:

```text
cargo fmt --all -- --check
cargo clippy --workspace --all-targets --all-features -- -D warnings
cargo test --workspace --all-targets
```

Portable domain code must have no Android or embedded-hardware dependency unless the crate is explicitly an adapter crate.

### Android compile gate

Each Android application must independently pass:

- Kotlin/Java compilation;
- relevant unit tests;
- Android lint;
- APK assembly;
- manifest/permission inspection;
- package/application ID inspection.

Compilation failures are application-workstream failures and must be resolved before product integration.

### Reader gate

Reader qualification additionally requires:

- exact upstream Vaachak repository + commit pin;
- deterministic application of the Sable flavor patch/overlay;
- upstream/core unit tests used by the reader path;
- Sable-flavor test/compile/lint;
- exact APK SHA-256;
- proof that the first accepted flavor does not silently expose network surfaces outside the approved Sable policy.

### Static/security gate

Static/security checking is a separate CI concern from compilation so failures remain diagnosable. At minimum the R8 train should maintain separate checks for:

- Rust Clippy and dependency/security audit;
- Android Lint;
- Java/Kotlin static analysis where configured;
- dependency/provenance inventory;
- APK permission/package inspection;
- repository policy/secret hygiene.

A static-analysis failure must not be hidden by a successful APK build.

### Artifact seal gate

Before product integration, each accepted application artifact must record at least:

```text
source repository
source commit SHA
build workflow/run identity
application/package ID
version code/version name
APK SHA-256
manifest permission inventory
relevant dependency/provenance inventory
```

The image build must consume the sealed artifact, not an untracked local rebuild.

## 5. Product integration gate

The exact Android 17 / GrapheneOS semantics for prebuilt application integration must be proven in the target tree before becoming normative. `android_app_import` is a candidate mechanism, not an assumption.

The product-wiring proof must establish:

```text
qualified APK
   -> declared Sable product module
   -> selected product package
   -> PRODUCT_OUT install path
   -> target-files/image membership
   -> package identity and APK byte/hash fidelity where applicable
```

Do not mutate unrelated generated substrate files merely to force application inclusion.

## 6. Build-budget rule

A Panther full image build is authorized only after the R8 source workstreams required for the integration tranche have passed their standalone gates and the exact integration inputs are frozen.

Normal iteration budget:

```text
Rust/unit/static CI       many times
Android/Gradle CI         many times
standalone device tests   as needed
Panther module/prebuilt integration proof  bounded
full Panther image        once per frozen integration tranche
Device1 campaign          once per accepted image tranche
```

A full image build must not be used as a substitute for Rust, Kotlin/Gradle or application-level correctness testing.

## 7. Local developer-machine role

A Mac or Linux workstation may run the same qualification commands before GitHub CI to shorten feedback loops. Local success is useful preflight evidence but does not replace the pinned GitHub qualification run used for the integration freeze.

The CI definition must remain reproducible enough that a local build and hosted build exercise the same source and dependency contract.

## 8. Licensing and provenance

Code ownership does not erase third-party obligations. Before product inclusion, independently inventory the terms for at least:

- Readium and other Vaachak dependencies;
- bundled fonts/assets;
- dictionary datasets/providers;
- Media3 and other Android dependencies;
- any imported game/media data/assets.

The ESP HELIX component is not part of the Sable Media Android architecture.

## 9. R8 closure condition

R8 application-foundation closure requires all selected workstreams to reach their source qualification gates, followed by a deliberate integration freeze. Only then does the image integration process begin.

R8 application CI success is **not** itself an image-release claim. Image closure still requires product wiring, target-files/image membership, runtime package identity, device behavior, and the normal SableOS image acceptance evidence.
