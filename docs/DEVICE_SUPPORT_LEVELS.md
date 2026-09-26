# SableOS device support levels

Status: **current normative support model — 2026-09-26**

SableOS separates product identity, functional portability, hardware capability
profiling and production security support.

A device-capability matrix does not promote a device. It records known, candidate
and unknown capabilities so build/design work does not rely on false inheritance.

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

- Unihertz Titan 2 — keyboard-first PORTABILITY / `TITAN2_N0_A16` planning.

Candidate:

- Unihertz Titan 2 Elite — independent keyboard-first PORTABILITY/N0 candidate;
  its own stock, boot, recovery, Treble, camera, telephony, display, AOD,
  keyboard, pointer and critical text-entry evidence is required.

PORTABILITY does not imply production-security ownership.

## RESEARCH

A hardware/BSP/platform used to learn integration patterns without a support
claim.

Current device:

- Zinwa Q27 — future product candidate only after shipped/current
  hardware/firmware qualification.

## PRODUCT_CANDIDATE

A device under evaluation for future supported-product status after meaningful
PORTABILITY evidence exists.

## PRIMARY

A current device chosen for active end-to-end release/security qualification.

No new PRIMARY is declared by this transition. Panther is frozen; Titan-family
and Q27 work remains PORTABILITY/RESEARCH until independently promoted.

## Non-Pixel assurance levels

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A successful GSI boot never promotes a target automatically.

## Capability profile states

Use explicit capability status values:

```text
PASS
FAIL
BLOCKED
NOT_TESTED
UNKNOWN
CANDIDATE_REQUIRES_VALIDATION
UNSUPPORTED_BY_DEFAULT
```

Public marketing specs are planning inputs only. Release claims require local or
physical evidence bound to the exact device, firmware and build basis.

## Rules

1. Common Sable product code must not fork because of SoC, display shape,
   physical keyboard, pointer surface, panel type or Android/vendor substrate.
2. Device repositories adapt capability; they do not redefine Sable semantics.
3. Interaction profile, hardware profile and support level are independent
   metadata.
4. Titan 2, Titan 2 Elite and Q27 require independent physical evidence.
5. Titan 2 PASS never qualifies Titan 2 Elite or Q27.
6. Security/update support and functional portability are separate claims.
7. Build outputs are isolated per device/release/source.
8. A physical serial is not part of artifact identity.
9. Device mutation remains disabled until the adapter's transport, partition,
   restore and acceptance contract is qualified.
10. Production signing/release readiness is a separate gate.

## Current matrix

| Device | Support level | Assurance | Interaction | Capability record | State |
| --- | --- | --- | --- | --- | --- |
| Pixel 7 / panther | REFERENCE_FROZEN | accepted R9 reference | touch-first | historical reference | feature development held |
| Titan 2 | PORTABILITY | N0_A16 planning | keyboard-first square + rear glance | `device-capabilities/TITAN2.md` | active planning |
| Titan 2 Elite | PORTABILITY candidate | N0 pending | keyboard-first compact AMOLED candidate | `device-capabilities/TITAN2_ELITE.md` | independent baseline pending |
| Q27 | RESEARCH | unqualified | keyboard-first compact AMOLED candidate | `device-capabilities/ZINWA_Q27.md` | shipped/current hardware gate |
| Pixel 4a 5G / bramble | historical | frozen | touch-first | none active | no active investment |
