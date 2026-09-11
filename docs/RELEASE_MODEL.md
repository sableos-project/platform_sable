# SableOS release model

SableOS product identity is separate from Android substrate identity and device qualification.

A SableOS product release may have multiple device builds which share the same common Sable release while using different Android versions, upstream substrates, and device adapters.

A complete build identity should record:

- Sable product release
- exact Sable component revisions
- device target
- device support level
- Android platform/API release
- upstream substrate and exact tag/revision
- vendor/BSP/firmware provenance
- revision-pinned manifest identity
- signing identity
- build number/date
- resulting artifact hashes

Conceptually:

```text
SableOS 0.1
  panther / PRIMARY / Android 17 / GrapheneOS 2026081300
  bramble / PORTABILITY / Android 16 / qualified LineageOS revision
```

The common product version does not imply that both targets have the same support or security status.

Do not encode Android version or device name into the primary Sable product version. Device images may use target-qualified filenames, while the semantic product version remains common.

Repository identity rules:

- `main` is current development state.
- component tags identify known component release or validation points.
- revision-pinned manifests identify exact complete source compositions.
- artifact hashes identify exact outputs.

Branches are development mechanics, not sufficient release provenance.
