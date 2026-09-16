# SableOS release model

Status: **normative product release/support identity model.**

SableOS product identity is separate from Android substrate identity, internal development milestones, application qualification status and device support level.

## Identity layers

A release/build record must distinguish:

```text
SableOS semantic product version
Android/substrate release identity
platform_manifest/source-composition identity
qualified external application artifact identities, when used
build/toolchain/host identity
device/product/release/variant identity
artifact hashes
signing/update identity
device support/qualification level
known limitations
```

No single layer substitutes for the others.

## Development milestones

`R5`, `R6`, `R7`, `R8`, `R9+` are internal development/validation milestones, not product version numbers.

Current sequencing:

```text
R5/R6  source + launcher foundation
R7     Panther product/daily-driver evidence baseline
R8     shared design + native application qualification/integration
R9+    next coherent productivity/replacement tranche
```

The superseded sequence "R8 design only -> R9 first Calculator" is no longer current. Calculator/Convert, Games, Reader and Media are part of the consolidated R8 application train.

## R8 release-candidate formation

R8 application qualification and R8 image qualification are separate stages.

```text
standalone source/application PASS
    |
    v
exact R8 application freeze
    |
    v
SableOS product wiring/image build PASS
    |
    v
Panther runtime/device PASS
    |
    v
approved release candidate
    |
    v
signing/release provenance
```

A standalone APK is never a SableOS release candidate by itself.

## Source-built and sealed-artifact inputs

A SableOS image may contain both:

1. Android/Sable components built from the revision-pinned OS source composition; and
2. exact qualified application artifacts whose canonical build/dependency graph remains outside AOSP.

When sealed external APKs are used, the release record must bind at least:

```text
application source repository + commit
upstream/reuse source commit where applicable
qualification workflow/run
APK SHA-256
package/application ID + version
permissions/components
native ABI/library inventory
third-party dependency/provenance inventory
product import/module identity
install partition/path
signing/update model
```

The release manifest/provenance record must not imply an APK was source-built inside AOSP when it was actually imported as a qualified artifact.

## Device support

Device support is explicit and separate from product version.

See `DEVICE_SUPPORT_LEVELS.md`.

A Panther build/device PASS does not automatically qualify Bramble, MediaTek/QWERTY or another target.

## Panther reference line

Pixel 7 (`panther`) remains the PRIMARY product-development/security/reference target. Exact GrapheneOS/Android substrate revisions are build inputs and must be rebound for every new validated build rather than inferred from an old reference workspace.

The next R8 Panther integration image is planned on the migrated `ai-g732` trusted builder after its host/storage/source/toolchain environment is sealed.

## Signing

Unsigned/development build success is distinct from signed release provenance.

Production signing occurs only after the exact release candidate input/output identities have been approved and verified. Signing material is not present on disposable CI or the normal trusted Android builder.

## Update/rollback

Every release should preserve enough identity to support:

- update provenance;
- component/app security-update ownership;
- rollback/fallback knowledge where supported;
- historical reconstruction/audit;
- clear distinction between current and deprecated supported device states.

Replacing a product application must retain the prior implementation/release identity in history even after it stops shipping.

## Release closure

A formal release claim requires more than a successful Android build. At minimum close the applicable claims for:

```text
source composition
qualified external inputs
build target/configuration
required images
product application inclusion
runtime/device behavior
security/permission policy
artifact checksums
signing provenance
support/known limitations
```

Use `PASS`, `FAIL`, `BLOCKED`, `NOT_TESTED` or equivalent bounded status rather than collapsing partial success into one release verdict.