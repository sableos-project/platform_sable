# SableOS release and support model

Status: **current normative model — 2026-09-24**

SableOS product identity is separate from Android substrate identity, internal
development milestone labels, device support level, artifact kind and production
signing state.

## Internal milestone context

R8/R9 are historical engineering milestones that produced the accepted Panther
reference. K1/K2 is the merged multi-device artifact/deployment foundation.

The active product phase is keyboard-first design plus Titan-family N0 research.

## Device support state

```text
REFERENCE_FROZEN
    accepted full-stack device retained for regression/maintenance

PORTABILITY
    device used to prove common Sable product portability

RESEARCH
    evidence collection only; no support claim

PRODUCT_CANDIDATE
    future support candidate after meaningful portability evidence

PRIMARY
    current end-to-end active release/security target
```

Current roles:

```text
panther       REFERENCE_FROZEN
titan2        PORTABILITY / N0 active
titan2-elite  PORTABILITY candidate / N0 pending
q27           RESEARCH
```

No new PRIMARY device is declared.

## Non-Pixel assurance

```text
N0_GSI_USERSPACE_LAB
N1_INTEGRATED_VENDOR_BSP_PORT
N2_PRODUCTION_QUALIFIED
```

A booting GSI is not N1/N2.

## Artifact identity

A release candidate identifies exact source plus exact artifact kind/hash and,
where needed, exact stock/vendor basis.

Artifact identity never includes a physical device serial.

## Device acceptance

A PASS is device-specific. Panther does not qualify Titan 2; Titan 2 does not
qualify Titan 2 Elite.

Functional portability and production-security ownership are separate claims.

## Production signing

Development/test signing is separate from production release signing.

Production application keys, AVB hierarchy, OTA signing/update service, key
custody/rotation/recovery and public support lifecycle remain deferred until a
future production-qualified target and repeatable release process exist.

## Status vocabulary

Use bounded states such as:

```text
PASS
FAIL
BLOCKED
NOT_TESTED
UNKNOWN
REFERENCE_FROZEN
PORTABILITY
RESEARCH
```

Do not collapse partial success into a release verdict.
