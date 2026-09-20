# SableOS release model

> **Current execution overlay — 2026-09-20:** R9 is the active development milestone. R8 established the application/design/product-composition foundation; R9 closes Launcher3/Sable Start, fresh Panther build causality and physical Pixel 7 runtime acceptance before Titan 2 keyboard-first portability work. GitHub-hosted build CI is not current release authority; local direct CI is canonical.


Status: **normative product release/support identity model.**

SableOS product identity is separate from Android substrate identity, internal development milestones, application qualification status, device support level and production signing state.

## Identity layers

A build/release record must distinguish:

```text
SableOS semantic product version when applicable
Android/substrate release identity
platform_manifest/source-composition identity
qualified external application artifact identities
build/toolchain/host identity
device/product/release/variant identity
artifact hashes
development-signing identity when applicable
production-signing/update identity when later activated
device support/qualification level
known limitations
```

No single layer substitutes for the others.

## Development milestones

```text
R5/R6  source + launcher foundation
R7     Panther product/daily-driver evidence baseline
R8     shared design + app qualification + trusted artifact freeze
       + Panther development integration + Titan 2 portability
R9+    next coherent productivity/replacement tranche
```

The superseded sequence `R8 design only -> R9 first Calculator` is no longer current.

## R8 development-candidate formation

```text
A1 disposable standalone qualification
    |
    v
A2 trusted standalone app build on ai-g732
    |
    v
exact trusted application freeze
    |
    v
B1 pre-image Android product-integration PASS
    |
    v
B2 Panther development image/runtime PASS
    |
    v
B3 Titan 2 portability development image/runtime PASS
    |
    v
R8 development architecture closure
```

This is deliberately **not** yet a production signed release claim.

## Source-built and sealed-artifact inputs

A SableOS image may contain both:

1. Android/Sable components built from revision-pinned OS source composition; and
2. exact qualified application artifacts whose canonical build/dependency graph remains outside AOSP.

For sealed external APKs record at least:

```text
application source repository + commit
upstream/reuse source commit where applicable
trusted A2 build/toolchain identity
trusted APK SHA-256
package/application ID + version
permissions/components
DEX/JNI inner-content identities
native ABI/16 KiB compatibility
third-party dependency/provenance inventory
product import/module identity
install partition/path
signing/transformation behavior
```

Do not imply an APK was source-built inside AOSP if it was intentionally imported as a sealed artifact.

## Device support

See `DEVICE_SUPPORT_LEVELS.md`.

Panther remains PRIMARY. Titan 2 is the active R8 PORTABILITY target. A Panther PASS does not automatically qualify Titan 2, and Titan 2 functional portability does not automatically establish production-security support.

## Trusted development builder

The next R8 application/product/image work is planned on `ai-g732` after host/storage/source/toolchain migration is sealed.

GitHub-hosted runners remain disposable qualification infrastructure; the trusted standalone application artifact is rebuilt on `ai-g732` before product integration.

## Signing

Development/test signing required for functional engineering images is separate from production signing architecture.

Production signing is deliberately deferred until Panther and Titan 2 development qualification is satisfactory.

The later signing workstream must define:

```text
production app-key policy
AVB key hierarchy
OTA signing
sign_target_files_apks flow
key custody / backup / recovery / rotation
approved artifact handoff
signing-host hardening/offline policy
signed-output provenance
```

The ThinkPad P50 is a future signing-host candidate after Android building has moved to `ai-g732`. It is not yet `sable-signer-01`. OptiPlex is not part of the current signing plan.

Production signing material must not be present on disposable CI or the ordinary trusted development builder.

## Update/rollback

Every eventual release should preserve enough identity for:

- update provenance;
- component/app security-update ownership;
- rollback/fallback knowledge where supported;
- historical reconstruction/audit;
- clear distinction between current and deprecated supported device states.

Replacing a product application must retain the prior implementation/release identity in history even after it stops shipping.

## Formal release closure

A future formal release claim requires more than R8 development closure. At minimum close applicable claims for:

```text
source composition
qualified external inputs
build target/configuration
required images
product application inclusion
runtime/device behavior
security/permission policy
artifact checksums
production signing/update provenance
support/known limitations
```

Use bounded statuses such as `PASS`, `FAIL`, `BLOCKED`, `NOT_TESTED` and `UNKNOWN`; do not collapse partial success into one release verdict.
