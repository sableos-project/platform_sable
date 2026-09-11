# SableOS device support levels

SableOS separates product identity from device qualification. A device booting SableOS does not automatically make it a supported production target.

## Levels

### PRIMARY

A current reference device used for active product, security, integration and release qualification.

Requirements include:
- reproducible upstream/source composition;
- validated build and runtime closure;
- supported or explicitly qualified vendor/firmware lifecycle;
- verified-boot/update behavior understood;
- broad hardware and app-compatibility acceptance;
- release provenance tracked.

Current device:
- Google Pixel 7 (`panther`) — PRIMARY.

### PORTABILITY

A device used to prove that Sable common product semantics and applications remain portable across another Android version, SoC generation or substrate.

A PORTABILITY target may be fully functional without being production-security supported.

Planned candidate:
- Google Pixel 4a 5G (`bramble`) on a modern Android/LineageOS substrate — PORTABILITY candidate.

Its older proprietary firmware/vendor lifecycle remains a security ceiling and must not be hidden by a newer Android userspace.

### RESEARCH

A hardware/BSP/platform used to learn integration patterns, compatibility techniques or vendor boundaries. Boot or partial functionality is not a support claim.

Examples may include:
- Brax3 / MediaTek reference work;
- Titan 2 GSI/BSP experiments;
- Q25 low-level MediaTek research.

### PRODUCT_CANDIDATE

A device being evaluated for future supported-product status. It has not yet met PRIMARY acceptance requirements.

Example:
- future Q27 full-QWERTY hardware, only after real hardware/BSP/security-lifecycle qualification.

## Rules

1. Common Sable product code must not be forked merely because a target has a different Android version or SoC.
2. Device-specific repositories are adapters, not copies of SableOS.
3. Support level is explicit metadata in manifests and validation records.
4. Promotion between levels requires evidence; it is never implied by a successful boot.
5. Security support and functional portability are separate claims.
6. A deprecated PRIMARY device may remain a PORTABILITY or historical reference without retaining production support.

## Initial matrix

| Device | Role | Substrate | Status |
| --- | --- | --- | --- |
| Pixel 7 / panther | PRIMARY | GrapheneOS 2026081300 / Android 17 | active reference |
| Pixel 4a 5G / bramble | PORTABILITY candidate | modern Android / LineageOS candidate | not yet qualified |
| Brax3 / Titan-class MediaTek | RESEARCH | vendor BSP + modern userspace | research only |
| Q27 | PRODUCT_CANDIDATE | TBD | future evaluation |
