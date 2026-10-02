# Titan 2 C3B integration boundary

Status: **current public integration guidance**

Date: 2026-10-01

This repository defines reusable Sable product semantics, interaction behavior,
device-profile contracts and visual-confirmation artifacts. It does not define
the private/canonical Android product graph, release signing state or target
workspace layout.

For Titan 2 C3B work, use these boundaries.

## Sable Start / HOME

Sable Start remains the product-facing HOME experience and semantic model:

- Start;
- All Apps;
- type-to-launch;
- Command/Search;
- app context;
- privacy summaries;
- keyboard-first focus/navigation;
- quick-bar / three-page behavior.

That does **not** imply a separate Android HOME APK.

The current canonical integration architecture after the R9L8 cutover hosts
Sable Start presentation/state source inside Launcher3/Launcher3QuickStep. A
parallel implementation must not create a second standalone HOME runtime merely
because these public design documents call the surface "Sable Start" or "HOME".

Public design rule:

```text
SABLE_START_PRODUCT_SURFACE=YES
STANDALONE_HOME_RUNTIME_REQUIRED=NO
ANDROID_HOME_RUNTIME_OWNER=CANONICAL_INTEGRATION_DECISION
CURRENT_CANONICAL_IMPLEMENTATION=LAUNCHER3_HOSTED_SABLE_START
HOME_AUTHORITY_SCOPE=SABLE_FIRST_PARTY_CANONICAL
THIRD_PARTY_HOME_SELECTION_ALLOWED=YES
FORCE_SABLE_HOME_AFTER_USER_SELECTION=NO
```

Reusable type-to-launch/search logic may be developed independently, but final
Sable first-party runtime ownership belongs to the canonical integration
repository. Android user selection of an installed third-party HOME remains
allowed; SableOS must not force Sable Start back after explicit user choice.

## Sable Keyboard / third-party IME boundary

The canonical lane may establish Sable Keyboard as the factory/default
first-party IME for critical-entry qualification, but that does not remove
Android's user-selectable IME model.

```text
SABLE_FIRST_PARTY_IME=SableKeyboard
THIRD_PARTY_IME_INSTALL_ALLOWED=YES
THIRD_PARTY_IME_ENABLE_ALLOWED=YES
THIRD_PARTY_IME_SELECTION_ALLOWED=YES
FORCE_SABLE_IME_AFTER_USER_SELECTION=NO
```

A third-party IME is not automatically covered by Sable's direct-boot,
lockscreen, pairing or physical-keyboard acceptance evidence.

## Camera Control Deck boundary

The public product direction for keyboard devices is a Sable-owned Camera2
application using a viewfinder-first **Camera Control Deck**. Portrait remains
supported; landscape may optimize for physical controls; touch remains a full
fallback; no global landscape lock is required.

This is a product/interaction contract. Camera HAL, SYSTEM_CAMERA privilege and
device-specific camera topology remain canonical/device evidence decisions.

## Titan 2 N1D/C3B product model

Current canonical private-integration identity:

```text
release track: N1D
product:       sable_titan2_n1d_arm64
stage target:  vendor/sable/n1d
```

This public repository does not create a second `vendor/sable/titan2` product
authority and does not publish or prescribe private signing material.

## Parallel product-source development

A parallel product repository may implement:

- Kotlin/Java applications;
- keyboard/IME behavior;
- Camera/UI modules;
- Display Compatibility UI;
- diagnostics UI;
- UX/visual traceability;
- product-source qualification;
- explicit platform-interface requests.

It must treat the canonical integration repository as authoritative for:

- Android/AOSP/Graphene/Treble composition;
- HOME runtime hosting;
- SELinux and privileged-service changes;
- radio/IMS/QNS/IWLAN behavior;
- device/vendor compatibility patches;
- product assurance properties;
- Android image composition authority;
- development/release signing policy;
- physical flash/release qualification.

## Source-built rule

Reusable product code should be handed off as source.

```text
SOURCE_AUTHORITATIVE=YES
CHECKED_IN_PRESIGNED_PRODUCT_APKS=NO
PRODUCTION_SIGNING_MATERIAL_IN_PUBLIC_REPO=NO
```

Generated APKs may be build evidence; they are not the canonical source of the
product implementation.

## Sable Messages

The integrated product should preserve the Sable Messages product direction.
Stock/AOSP Messaging may be used temporarily for bring-up, but it is not a
silent replacement for the Sable Messages product surface.


## Sable Hub parity

C3B must preserve the common Sable Hub behavior proven on Panther. The Hub is a
separate canonical surface from Sable Messages and its Connected Apps semantics
must not be removed by Messages reconciliation or device-specific work.

See `SABLE_HUB_PORTABILITY_CONTRACT.md`. In particular, Titan profiles preserve
generic package+user Connected Apps configuration, Android
notification/conversation ingestion, source-authorized RemoteInput reply,
Open-app fallback, bounded local derived history and dynamic provider discovery.
Provider names are compatibility/evidence targets, not a hard-coded allowlist.

## Design-document interpretation

Documents under `docs/design/` are normative for:

- information architecture;
- keyboard/touch/pointer behavior;
- privacy presentation;
- focus/navigation expectations;
- product labels;
- visual hierarchy;
- acceptance intent.

They are **not** authority for:

- Android package/module ownership;
- Soong/product inheritance;
- signing;
- privileged permission grants;
- system-service ownership;
- SELinux policy;
- radio/vendor behavior.

Where a design document describes "Sable Start as HOME", interpret that as the
Sable-facing HOME product surface unless it explicitly discusses Android runtime
hosting.

## Validation boundary

The public design/source lane may run its own syntax, unit, lint, static and
performance checks.

Canonical CI and image qualification occur in the canonical integration
repository on the controlled build host. A public/parallel source PASS is not a
canonical Android image or release PASS.
