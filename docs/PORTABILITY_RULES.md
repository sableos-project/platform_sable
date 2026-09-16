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

## Current target roles

- **Panther / Pixel 7:** PRIMARY product-development/security/runtime reference.
- **Titan 2:** active R8 PORTABILITY target for a materially different MediaTek/QWERTY/square-display substrate.
- **Bramble:** future legacy-hardware regression/portability candidate after current R8 dual-target work.
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

A new target should require less common-code change than device-adapter change. Repeated common semantic changes during porting are evidence that the abstraction boundary needs review.
