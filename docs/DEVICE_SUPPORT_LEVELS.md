# SableOS device support levels

SableOS separates product identity from device qualification. A device booting SableOS does not automatically make it a supported production target.

## Levels

### PRIMARY

A current reference device used for active product, security, integration and runtime qualification.

Requirements include:
- reproducible upstream/source composition;
- validated build and runtime closure;
- supported or explicitly qualified vendor/firmware lifecycle;
- verified-boot/update behavior understood before production support;
- broad hardware/app compatibility acceptance;
- release provenance tracked when production releases begin.

Current device:
- Google Pixel 7 (`panther`) — PRIMARY.

### PORTABILITY

A device used to prove that common Sable product semantics and applications remain portable across another Android substrate, SoC generation, display/input model or vendor BSP.

A PORTABILITY target may be fully functional without being production-security supported.

Current active R8 target:
- Unihertz Titan 2 — PORTABILITY development target.

R8 uses Titan 2 specifically to test the common application/product boundary across a MediaTek/QWERTY/square-display platform after Panther qualification. Its production-security/support status remains separate and unproven unless a later support program closes that evidence.

Future/secondary candidate:
- Google Pixel 4a 5G (`bramble`) — legacy-hardware regression/portability candidate, not current R8 priority.

### RESEARCH

A hardware/BSP/platform used to learn integration patterns, compatibility techniques or vendor boundaries. Boot or partial functionality is not a support claim.

Examples may include:
- Brax3 / other MediaTek reference work;
- Q25 low-level MediaTek research;
- other BSP/GSI experiments not yet promoted to PORTABILITY.

### PRODUCT_CANDIDATE

A device being evaluated for future supported-product status. It has not yet met PRIMARY acceptance requirements.

Examples may include future QWERTY hardware only after real hardware/BSP/security-lifecycle qualification.

## Rules

1. Common Sable product code must not be forked merely because a target has a different Android version, SoC, display shape or physical keyboard.
2. Device-specific repositories are adapters, not copies of SableOS.
3. Support level is explicit metadata in manifests and validation records.
4. Promotion between levels requires evidence; successful boot alone never promotes support.
5. Security support and functional portability are separate claims.
6. A deprecated PRIMARY device may remain PORTABILITY/historical without retaining production support.
7. Production signing/release readiness is separate from development image acceptance.

## R8 dual-target matrix

| Device | Role | R8 purpose | Status |
| --- | --- | --- | --- |
| Pixel 7 / panther | PRIMARY | reference product/runtime qualification | active |
| Titan 2 | PORTABILITY | same common R8 app/product artifacts across MediaTek/QWERTY/square-display substrate | active R8 target |
| Pixel 4a 5G / bramble | PORTABILITY candidate | future legacy/regression work | deferred |
| Other MediaTek/BSP devices | RESEARCH / PRODUCT_CANDIDATE | future exploration | not current R8 closure target |

Production signing is deferred until Panther and Titan 2 development builds/runtime behavior are satisfactory. Device support promotion after that still requires a separate security/update/firmware/release assessment.
