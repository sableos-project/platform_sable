# Sable Platform

Common SableOS semantic contracts, services, portable policy, and bounded Android integration.

This repository is intended to contain functionality that should remain stable across devices and Android substrates. It is not a device tree and should not accumulate hardware-specific implementation merely for convenience.

## Core rule

Sable semantics should be device-independent. Android-version and hardware differences belong behind narrow adapters.

Examples of suitable common ownership include typed service contracts, portable transformation/policy logic, stable AIDL interfaces, provider capability models, and renderer-independent application semantics.

Examples that do not belong here include device-specific HAL wiring, vendor firmware assumptions, board configuration, and target-specific compatibility shims.

See `docs/ARCHITECTURE.md` and `docs/PORTABILITY_RULES.md`.
