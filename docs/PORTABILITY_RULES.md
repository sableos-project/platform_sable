# Portability rules

1. Common product behavior goes into common repositories by default.
2. Device repositories may adapt capability, not redefine Sable semantics.
3. Android-version/substrate differences should be isolated behind small adapters.
4. Stable AIDL/C/NDK/JNI boundaries are preferred when compiler/toolchain universes differ.
5. Vendor and firmware limitations remain explicit release/support inputs.
6. A successful boot is not sufficient portability evidence; build, application, runtime, security and update behavior are separate claims.
7. Compatibility shims are version-bound maintenance debt and must be documented/tested.
8. Common Sable application source must not fork merely because a target has a different SoC, display, input device or vendor BSP.
9. Per-target OUT_DIRs are isolated. Shared source/artifact identity is proven explicitly rather than inferred from similar output paths.
10. Native common libraries must be compatible with the strictest accepted page-size/alignment requirement in the target set where practical.
11. Kernel-source availability alone does not promote a device into PORTABILITY; device/vendor/BSP, proprietary-blob, VINTF, hardware-subsystem and runtime feasibility remain separate gates.

## Current target roles

- **Panther / Pixel 7:** PRIMARY product-development/security/runtime reference.
- **Titan 2:** active R8 PORTABILITY target for a materially different MediaTek/QWERTY/square-display substrate.
- **Bramble:** future legacy-hardware regression/portability candidate after current R8 dual-target work.
- **Zinwa Q27:** deferred RESEARCH / future PRODUCT_CANDIDATE. It is not an R8 image target and must not divert current Panther/`ai-g732` execution.
- Other MediaTek/QWERTY targets: research/product-candidate work only when separately documented.

## R8 portability success condition

Where platform compatibility permits:

```text
same qualified common application source/artifacts
+ same common vendor_sable product integration
+ isolated target OUT_DIRs
+ bounded device adapters
+ target-specific runtime acceptance
+ no common application source fork
```

A Titan-specific keyboard/layout adapter is acceptable. A duplicate Calculator/Games/Reader/Media implementation solely because Titan uses MediaTek or a square display is not.

## Titan 2 R8 checks

Explicitly validate:

- arm64 application/JNI compatibility;
- 16 KiB native-library compatibility;
- touch plus physical-keyboard navigation/focus/text entry;
- square-display layout/readability;
- Reader OCR/TTS capability boundaries;
- Media3/audio behavior;
- runtime page size;
- any vendor/BSP limitation that materially changes Android framework behavior.

Secondary-display, programmable-key, FM-radio and other Titan-specific features are not common R8 requirements unless a later requirement explicitly adopts them.

## Deferred Q27 rule

The Q27 may later be evaluated in two independent tracks:

```text
stock Android 16 Q27
    -> Sable application compatibility only

future community/Lineage device enablement
    -> SableOS platform-port feasibility
```

Current project information indicates that Zinwa plans to publish kernel source but not the complete Android/device/vendor OS source. A future SableOS Q27 port should therefore prefer consuming a credible community/Lineage bring-up rather than independently reconstructing the entire platform during R8.

The project has access to Q27 OTA artifacts, but OTA analysis is intentionally on hold. Resuming it requires a separate research decision and must not preempt:

```text
ai-g732 migration/seal
-> R8 A2 trusted app build
-> R8 B1 pre-image integration
-> Panther B2 development/runtime closure
-> Titan 2 B3 portability closure
```

A new target should require less common-code change than device-adapter change. Repeated common semantic changes during porting are evidence that the abstraction boundary needs review.
