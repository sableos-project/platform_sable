# Titan 2 dual-track coordination status — 2026-10-18

> **Documentation-only current-state overlay.** This is a shared portability/status notice, not a device-specific integration change, release claim, signing permission or flash authorization. It supersedes the dated status framing in `ENGINEERING_STATUS_2026-10-13.md` without rewriting that historical evidence.

## Operator-verified October 18 state

- Legacy 2026 Titan 2 Android workspaces `titan2-n1d` (494.690 GiB reported allocated) and `titan2-n1d-c3b` (677.612 GiB) were retired. The host subsequently showed 3.6T total, 1.7T used, **2.0T free / 47% used**, with a retained retirement receipt.
- Independently sealed boot-qualified C3A `system.img` SHA-256 `4c292b9876a43f8df515a163a197e92d71deba598767790681f999ea3c554c16` and N1F `system.img` SHA-256 `dc1f917d5892afc04c479907913eb27a436d5abd881377d3e24fd1d7eb46299a` both passed a new `sha256sum -c` check; their original artifact paths remain outside the retired directories.
- Dual-track bootstrap V1 SHA verification and 16 hermetic tests passed. `/srv/sable-build/workspaces/titan2-track-a-v2` and `/srv/sable-build/workspaces/titan2-track-b` were created with separate local canonical SableOS clones, both at local commit `177ef4fb76d661725165c5224f6a8495b2b420ec`. Remote canonical `main` at review was `12e9793627babd0294a0022d8644360bc7727f6c`: **source update/pin still required before either build**.
- The bootstrap itself performed **no Android source sync, Android image build, device contact, Q25 access or flash**. The resulting `verify` JSON file was not supplied; tool checks are not independently attested here.
- **Later, on 2026-10-18, Track A V1.3 succeeded in a real host-only OEM-derived engineering `product` EROFS build** using the preserved quarantined P10 13-app input and original OEM metadata PAX. The operator log reports 13/13 APK signature verification (10 Sable engineering-signed; 3 existing signed), 462/462 OEM PAX metadata entries copied, `mkfs.erofs` PASS, `fsck.erofs` PASS and extraction PASS. New EROFS SHA-256: `fcd1bd62f4259222084fc9651186925cc42b59eb530f9cee66dfcbb4fa36dc45`. Path: `/srv/sable-build/workspaces/titan2-track-a-v2/dist/track-a-oem16-signed-app-product-20261018T155147318439Z/NOT_FLASH_READY_SIGNED_13_APPS_PRODUCT.erofs.img`. **This is not a complete firmware, OEM-native QuickStep/Sable Start integration, independently audited final EROFS metadata, AVB-admitted partition, or boot/flash test.**

## Two independent engineering lanes

| Lane | Intent | Not yet demonstrated |
|---|---|---|
| Track A V2 | OEM Android 16 stock-preserving SableOS; host-only signed-app `product` EROFS **built**, SHA `fcd1bd62...`, not flash ready | Native OEM QuickStep/Recents, full firmware/source integration, independent post-build inode/xattr/SELinux policy, compatible product AVB/LP/COW/restore and boot evidence. OEM production keys **not** a prerequisite for host engineering APK/product creation |
| Track B | Fresh public-only Android 16 GrapheneOS 2026061600 + pinned RestlessOS 16.0.0-202606230923/Treble source; N1F proven C3A-based compatibility reference; optional source-built FLOSS IMS | Android source sync and 369-patch compatibility-control reproduction, reviewed release patch allowlist, FLOSS IMS module signing/service/overlay tests, final image, cellular attach/data/IMS runtime evidence |

Track B must not treat cosmetic PHH provider/connectivity indication as a successful mobile data bearer. A downloaded FLOSS IMS APK is not itself a package signature/provenance or IMS registration proof. The C3A 369-patch control stack is not blanket SableOS release admission.

## Governance and boundaries

- Canonical device-integration handoff is proposed in [aimindseye/sableos PR #344](https://github.com/aimindseye/sableos/pull/344); that PR is documentation-only and is **not yet merged**.


- Canonical device-specific implementation plan is in `aimindseye/sableos`: `docs/titan2/TITAN2_DUAL_TRACK_FRESH_BUILD_HANDOFF_20261018.md`.
- Private Track A source [PR #343](https://github.com/aimindseye/sableos/pull/343) was **closed unmerged**; its changes are not inherited by branch name. Private N1F/M4 recovery and IMS [PR #342](https://github.com/aimindseye/sableos/pull/342) remains **open draft**, not Track B flash authority.
- Shared `sableos-project/platform_sable` repository owns common application semantics, UI contracts, capability profiles, portability and assurance standards; it does **not** own Titan 2 proprietary telephony, OEM signing keys, LP/AVB, recovery, or firmware source.
- Preserve Panther R9 frozen touch-first reference, Q25 unchanged, latest R8/Panther workspaces and provenance. No inter-device evidence inheritance.
- Any actual Android source sync, build, device inspection, flash or signing requires the appropriate bounded owner decision and subsequent recorded qualification. Flash must be separately authorized and Titan 2 mode/slot/restore-aware; never assume a generic Pixel fastboot flow.

```text
DATE=2026-10-18
TITAN2_HOST_CLEANUP=PASS_OPERATOR_REPORTED
C3A_N1F_REFERENCE_HASHES=PASS
TRACK_A_V2_WORKSPACE=BOOTSTRAPPED
TRACK_B_WORKSPACE=BOOTSTRAPPED
CANONICAL_LOCAL_HEAD=177ef4fb76d661725165c5224f6a8495b2b420ec
CANONICAL_REMOTE_MAIN_AT_REVIEW=12e9793627babd0294a0022d8644360bc7727f6c
CANONICAL_SOURCE_REFRESH=OPEN
ANDROID_SOURCE_SYNC=NOT_RUN
TRACK_A_V2_ENGINEERING_PRODUCT_EROFS=BUILT_HOST_NONFLASH
TRACK_A_V2_PRODUCT_SHA256=fcd1bd62f4259222084fc9651186925cc42b59eb530f9cee66dfcbb4fa36dc45
TRACK_A_V2_APK_SIGNATURES=PASS_13_OF_13
TRACK_A_V2_FULL_OEM_FIRMWARE=NOT_BUILT
TRACK_A_V2_NATIVE_QUICKSTEP=NOT_BUILT
TRACK_B_COMPILE=NOT_RUN
IMS_SOURCE_INTEGRATION=NOT_RUN
PHYSICAL_FLASH=NOT_AUTHORIZED
PRODUCTION_RELEASE=NOT_AUTHORIZED
```

The October 13 M4 recovery failure and October 11 exact-artifact ambiguity remain valid **historical** findings; neither changes the independent October 3 boot-qualified N1F control.
