# Sable application reuse and integration model

Status: **current common integration model — 2026-09-24**

The historical R8 A1/A2/B1/B2/B3 sequence produced the accepted Panther
reference. Current integration policy keeps its useful evidence separation while
moving to a multi-device/keyboard-first architecture.

## Common rule

Reusable Sable applications are common product code. Device differences are
handled by interaction profiles and bounded adapters.

```text
app source/qualification
  != trusted artifact
  != product selection
  != image membership
  != runtime/default role
```

## Accepted common app family

Current product references include SableLauncher, Calculator, Sudoku,
Minesweeper, 2048, Media, Reader, Text Reader, Hub/Messages, Mail, Weather and
Calendar.

Reader/Text Reader are separate products.

## Integration evidence

For each app preserve:

- exact source and upstream pin;
- dependency/toolchain identity;
- package/module identity;
- tests/static/security results;
- final artifact hash;
- native/JNI identity where applicable;
- product selection/install partition;
- image/artifact membership;
- runtime behavior.

## Multi-device artifact model

K1 registry v2 allows target-files, full-device images, GSI/system images,
system/product bundles and boot/recovery bundles.

The artifact class is explicit. A physical serial is not artifact identity.

## Device portability

Panther is frozen reference evidence.

Titan 2 and Titan 2 Elite should consume the same common app source/artifacts
where substrate compatibility permits. Keyboard-first presentation changes
focus/layout/input behavior, not application ownership.

## Production signing

Production signing/update lifecycle remains separate and deferred.
