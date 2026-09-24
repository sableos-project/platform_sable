# SableOS device support levels

> **Current tooling note — 2026-09-24:** K1/K2 is merged. Artifact schema and common deployment orchestration are multi-device, but Titan 2, Titan 2 Elite and Q27 remain fail-closed for release artifact registration/flash until independently qualified.


SableOS separates product identity, functional portability and production
security support.

## REFERENCE_FROZEN

A previously accepted full-stack device retained for regression, architecture
comparison and maintenance without driving new product design.

Current device:

- Google Pixel 7 (`panther`) — R9 physical acceptance PASS; active feature
  development on hold.

A frozen reference may receive security-critical or common-regression fixes, but
new form-factor/product behavior is not designed around it by default.

## PORTABILITY

A device used to prove that common Sable product semantics remain portable
across another Android substrate, SoC, display/input model or vendor BSP.

Current active target:

- Unihertz Titan 2 — keyboard-first PORTABILITY / N0 target.

Candidate:

- Unihertz Titan 2 Elite — independent keyboard-first PORTABILITY/N0 candidate;
  its own stock, boot, recovery, Treble, camera, telephony and display evidence
  is required.

PORTABILITY does not imply production-security ownership.

## RESEARCH

A hardware/BSP/platform used to learn integration patterns without a support
claim.

Current device:

- Zinwa Q27 — future product candidate only after shipped hardware/firmware
  qualification.

## PRODUCT_CANDIDATE

A device under evaluation for future supported-product status after meaningful
PORTABILITY evidence exists.

## PRIMARY

A current device chosen for active end-to-end release/security qualification.

No new PRIMARY is declared by this transition. Panther is frozen; Titan-family
devices begin at PORTABILITY/N0.

## Non-Pixel assurance levels

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A successful GSI boot never promotes a target automatically.

## Rules

1. Common Sable product code must not fork because of SoC, display shape,
   physical keyboard or Android/vendor substrate.
2. Device repositories adapt capability; they do not redefine Sable semantics.
3. Interaction profile and support level are independent metadata.
4. Titan 2 and Titan 2 Elite require independent physical evidence.
5. Security/update support and functional portability are separate claims.
6. Build outputs are isolated per device/release/source.
7. A physical serial is not part of artifact identity.
8. Device mutation remains disabled until the adapter's transport, partition,
   restore and acceptance contract is qualified.
9. Production signing/release readiness is a separate gate.

## Current matrix

| Device | Support level | Assurance | Interaction | State |
| --- | --- | --- | --- | --- |
| Pixel 7 / panther | REFERENCE_FROZEN | accepted R9 reference | touch-first | feature development held |
| Titan 2 | PORTABILITY | N0 active | keyboard-first | active |
| Titan 2 Elite | PORTABILITY candidate | N0 pending | keyboard-first | independent baseline pending |
| Q27 | RESEARCH | unqualified | keyboard-first future | shipped-hardware gate |
| Pixel 4a 5G / bramble | historical | frozen | touch-first | no active investment |

