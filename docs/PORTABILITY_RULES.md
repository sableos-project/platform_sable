# Portability rules

1. Common product behavior goes into common repositories by default.
2. Device repositories may adapt capability, not redefine Sable semantics.
3. Android-version differences should be isolated behind small adapters.
4. Stable AIDL/C/NDK boundaries are preferred when compiler/toolchain universes differ.
5. Vendor and firmware limitations must remain explicit release inputs.
6. A successful boot is not sufficient portability evidence; build, functionality, security, and update behavior must be qualified separately.
7. A compatibility shim is version-bound maintenance debt and must be documented and tested.
8. Old hardware may remain valuable for regression/portability experiments without being declared production-security supported.

## Target roles

- Panther: primary product-development and security/reference target.
- Bramble: candidate legacy-hardware portability/regression target.
- MediaTek/QWERTY: future portability axis after common boundaries are proven.

A new target should require less common-code change than device-adapter change. If porting repeatedly changes product semantics, the abstraction boundary should be reconsidered.
