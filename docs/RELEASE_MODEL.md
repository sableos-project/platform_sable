# SableOS release and support model

Status: **current normative model — 2026-09-26**

SableOS product identity is separate from Android substrate identity, internal
development milestone labels, device support level, capability profile, artifact
kind and production signing state.

## Internal milestone context

R8/R9 are historical engineering milestones that produced the accepted Panther
reference. K1/K2 is the merged multi-device artifact/deployment foundation.

The active product phase is keyboard-first design plus Titan-family/N0 profile
and capability planning.

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
titan2        PORTABILITY / TITAN2_N0_A16 planning
titan2-elite  PORTABILITY candidate / N0 pending
q27           RESEARCH
```

No new PRIMARY device is declared.

## Capability gates

Release planning must bind each target to a capability profile:

```text
hardware profile
display profile
panel power / AOD profile
attention surface profile
keyboard profile
pointer surface profile
critical text-entry profile
app layout profile
```

Capability profiles are not release evidence by themselves. They define what
must be proven before a user-facing artifact can make a claim.

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

Titan 2 first planning identity:

```text
TITAN2_N0_A16
    Android 16 / SDK 36 planning target
    stock-vendor-bound
    first substrate: AOSP16 clean GSI
    RestlessOS: reference/future fork only
    flash: closed
```

## Device acceptance

A PASS is device-specific. Panther does not qualify Titan 2; Titan 2 does not
qualify Titan 2 Elite; Titan 2 does not qualify Q27.

Functional portability and production-security ownership are separate claims.

Critical text-entry gates are release blockers for keyboard-first devices:

```text
Bluetooth pairing
Wi-Fi password entry
Setup Wizard
Lockscreen / PIN / password
Emergency text fields
Account sign-in
Recovery / restore prompts
```

## AOD and attention surfaces

AOD is a device capability, not a product-wide feature flag.

```text
Titan 2       AOD_DEFAULT=NO
Titan 2 Elite AOD=CANDIDATE_REQUIRES_VALIDATION
Q27           AOD=CANDIDATE_REQUIRES_VALIDATION
```

SubScreen/rear-display glance, LED, keyboard backlight, haptics, sound and pulse
are separate attention surfaces and require independent evidence.

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
CANDIDATE_REQUIRES_VALIDATION
UNSUPPORTED_BY_DEFAULT
```

Do not collapse partial success into a release verdict.
