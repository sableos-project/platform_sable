# SableOS release model

SableOS product identity is separate from Android substrate identity and device qualification.

A SableOS product release may have multiple device builds which share the same common Sable release while using different Android versions, upstream substrates, and device adapters.

A complete build identity should record:

- Sable product release;
- exact Sable component revisions;
- device target;
- device support level;
- Android platform/API release;
- upstream substrate and exact tag/revision;
- vendor/BSP/firmware provenance;
- revision-pinned manifest identity;
- signing identity;
- build number/date;
- resulting artifact hashes.

Conceptually:

```text
SableOS 0.1
  panther / PRIMARY / Android 17 / GrapheneOS 2026081300
  bramble / PORTABILITY / Android 16 / qualified LineageOS revision
```

The common product version does not imply that both targets have the same support or security status.

Do not encode Android version or device name into the primary Sable product version. Device images may use target-qualified filenames, while the semantic product version remains common.

## Development milestones are not product versions

Internal milestones such as:

```text
R5  migrated-source build/reconstruction closure
R6  real Sable Start launcher + local-time greeting
R7  daily-driver phone validation/default-app decisions
R8  Sable design/theme/customization foundation
R9  first native Sable utilities
R10+ deliberate application replacement/expansion
```

are development/validation checkpoints. They do **not** become semantic SableOS versions merely because a gate closes.

A milestone can close for one reference target while the overall product release remains unreleased or another target remains at a lower support level.

The current normative milestone scope is maintained in `sableos-project/.github/docs/DEVELOPMENT_RELEASE_PLAN.md`. Milestone-specific requirements live in the owning component repository.

## Repository identity rules

- `main` is current development state.
- component tags identify known component release or validation points.
- revision-pinned manifests identify exact complete source compositions.
- artifact hashes identify exact outputs.
- development branches and milestone names are workflow/evidence references, not sufficient release provenance.

Branches are development mechanics, not sufficient release provenance.

## Release qualification rule

A semantic SableOS release claim must be backed by an exact source-composition identity and the validation appropriate to the claimed support level.

At minimum, release documentation should distinguish:

```text
source/component identity
complete manifest identity
build/toolchain identity
artifact identity
supported device(s)
runtime/daily-driver qualification status
known limitations
signing/update channel identity where applicable
```

Do not turn an application-level success such as an R6 launcher gate into a complete OS release claim.

Likewise, do not block development milestone progress merely because the final public version number has not yet been chosen.