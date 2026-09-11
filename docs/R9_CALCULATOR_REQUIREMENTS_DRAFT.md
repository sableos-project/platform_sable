# R9 — Sable Calculator product requirements draft

Status: **pre-implementation requirements scaffold; explicitly marked decisions must be resolved before source implementation.**

This document captures what is already decided about the first Sable-owned utility without inventing calculator semantics that have not yet been chosen.

When the canonical Calculator repository is created, this document should be copied/refined into that repository and the canonical copy/path recorded in the requirements index.

## 1. Product intent — DECIDED

Sable Calculator will be the first native Sable utility application.

Its purpose is to:

- provide reliable everyday arithmetic;
- require minimal/no sensitive authority;
- serve as the reference application for R8 Sable design/theme consumption;
- establish the canonical Sable app repository/build/test/release pattern;
- remain simple enough that correctness can be strongly tested.

The first release is not intended to compete with a full scientific/programmer mathematics suite.

## 2. Permission/privacy baseline — DECIDED

The first Calculator must not require ordinary operation access to:

- network/Internet;
- location;
- phone/call state;
- SMS/MMS;
- contacts;
- camera;
- microphone;
- media/photos/audio;
- Bluetooth/nearby devices;
- broad storage/filesystem;
- account identity;
- privileged/system permissions.

If later requirements introduce a feature that would need one of these, update the requirements before adding the permission.

No telemetry/analytics is part of the initial Calculator requirement.

## 3. Baseline operations — PARTIALLY DECIDED

Required basic concepts:

- digits `0` through `9`;
- decimal input;
- addition;
- subtraction;
- multiplication;
- division;
- clear/reset;
- evaluation/result display;
- negative values/results;
- deterministic invalid/error behavior.

The following are **not yet automatically included** and must be explicitly chosen before implementation:

- percent key semantics;
- sign-change key semantics versus typing a unary minus;
- backspace/delete behavior;
- parentheses;
- repeated-equals behavior;
- memory keys;
- calculation history.

Do not implement these opportunistically before the interaction model is decided.

## 4. Calculation interaction model — TBD BEFORE CODING

Choose exactly one primary first-release model and document examples.

### Option A — immediate-execution handheld semantics

Example concept:

```text
2 + 3 × 4
```

may execute each operator as entered depending on the chosen handheld behavior.

This model needs explicit requirements for:

- chained operations;
- repeated equals;
- replacement of pending operator;
- percent behavior if present.

### Option B — expression-entry semantics

Example concept:

```text
2 + 3 × 4 = 14
```

using documented precedence/associativity.

This model needs explicit requirements for:

- operator precedence;
- parentheses if supported;
- incomplete expressions;
- editing/backspace;
- expression display length.

### Decision requirement

The implementation must not mix the two models accidentally. Before source work, update this section to `DECIDED` with concrete input/result examples.

## 5. Numeric representation/precision — TBD BEFORE CODING

The first version should prioritize predictable everyday decimal results.

Before implementation decide:

- internal numeric representation;
- maximum precision/scale;
- rounding mode when a result cannot be displayed exactly;
- display precision limit;
- whether trailing zeroes are trimmed;
- exponent/scientific notation policy for very large/small values;
- overflow/input-length behavior.

Do not rely on unexamined binary floating-point string conversion for user-visible decimal behavior.

Required examples should include cases such as:

```text
0.1 + 0.2
1 / 3
2 / 3
10 / 4
large-value multiplication
very small decimal input
```

Expected outputs must be documented after the numeric policy is selected.

## 6. Division by zero/error model — TBD DETAILS, ERROR MUST BE SAFE

Division by zero and malformed/incomplete input must:

- never crash the app;
- show a clear user-visible error/state;
- allow recovery with a documented next action such as Clear or new input.

Before coding decide whether the display shows text such as `Error`, `Cannot divide by zero`, or another Sable copy choice.

Do not expose Java/Kotlin exception strings to users.

## 7. Input/display constraints — TBD BEFORE FINAL UI

Decide/document:

- maximum number of entered digits;
- maximum expression length if expression mode is chosen;
- behavior when input exceeds the visible display width;
- horizontal scrolling/scaling/wrapping policy;
- decimal separator/localization policy;
- leading zero behavior;
- multiple decimal-point handling;
- orientation/window-size expectations.

The UI must not silently drop digits without feedback.

## 8. History — DEFAULT OUT OF SCOPE UNTIL CHOSEN

The initial Calculator may ship without persistent history.

If history is requested, update requirements first with:

- what entries are stored;
- retention limit;
- persistence across process restart/reboot;
- clear/delete behavior;
- local storage format;
- backup/export policy;
- privacy expectations.

Cloud/account synchronization is not part of the initial direction.

## 9. Clipboard — TBD

Copying a result may be useful but has not been explicitly selected.

Before implementing copy/paste, define:

- which display can be copied;
- whether paste into input is supported;
- validation/sanitization of pasted text;
- accessibility feedback.

Do not request broad clipboard/background authority beyond normal Android app behavior.

## 10. Design/theme integration — DECIDED AT ARCHITECTURE LEVEL

Calculator must consume the R8 shared Sable design/theme contract rather than define a parallel application theme.

Required modes after R8:

```text
Follow system
Light
Dark
```

and the supported Sable accent abstraction.

Calculator-specific visual needs may add local tokens only when they are genuinely application-specific.

Do not copy literal Sable Start colors/dimensions.

## 11. Initial visual direction — FUNCTIONAL, NOT YET PIXEL-LOCKED

The first release should prioritize:

- clear current input/expression/result hierarchy;
- large readable numeric display;
- consistent key layout;
- clear distinction for arithmetic/action keys;
- accessible touch targets;
- predictable portrait phone layout;
- adaptation to supported window/font sizes without losing critical controls.

Exact visual design should follow R8 tokens and may be refined later. Do not block Calculator correctness on elaborate animation or decorative effects.

## 12. Accessibility — DECIDED BASELINE

Required:

- every control has correct accessible label/role;
- traversal order follows calculation flow;
- current expression/result can be understood through accessibility services;
- key labels do not depend on icon-only ambiguity without content descriptions;
- practical Android font scaling support;
- touch targets meet accepted Android accessibility guidance;
- no functional distinction relies only on color;
- light/dark/accent variants retain readable contrast.

Accessibility is part of first-release correctness.

## 13. Localization — MINIMUM POLICY TBD, ARCHITECTURE MUST NOT BLOCK IT

At minimum, user-visible strings must use resources rather than hard-coded UI text.

Before public release decide:

- initial supported locales;
- decimal separator/localized number formatting behavior;
- operator glyph/text localization needs;
- RTL layout behavior.

Do not bake English formatting assumptions deeply into calculation logic.

## 14. Process/state behavior — TBD

Before final closure decide:

- whether an in-progress calculation survives process recreation;
- whether it survives app relaunch;
- whether it survives reboot;
- whether state is ephemeral or stored.

If state restoration is implemented, test it deterministically and ensure it does not create unnecessary persistent sensitive data.

## 15. Package/repository decisions — TBD WHEN REPOSITORY IS CREATED

Record before integration:

- canonical repository name;
- Android checkout path;
- package name;
- Soong module name;
- application label;
- versioning policy;
- minimum/target SDK inherited from product policy;
- signing/product inclusion policy.

Likely repository naming should follow existing Android component conventions, but do not create scripts that assume a repository name before it exists.

## 16. Build dependencies — MINIMIZE

Prefer platform/Kotlin/Compose capabilities already present in the Sable build.

Before adding a third-party math/parser/UI dependency document:

- why it is necessary;
- license;
- security/update owner;
- size/build impact;
- deterministic test implications.

For a basic calculator, a large networked or analytics-bearing dependency is not acceptable by default.

## 17. Deterministic logic tests — REQUIRED

Once semantics are decided, unit/host tests must cover at least:

- each supported operation;
- zero operands/results;
- negative operands/results;
- decimal inputs;
- selected chaining/precedence semantics;
- repeated operator input behavior;
- clear/reset;
- invalid/incomplete expression handling;
- division by zero;
- precision/rounding examples;
- maximum input/expression limits;
- state restoration if implemented;
- percent/sign/backspace/history semantics if included.

Business logic should be testable without needing a physical Android device.

## 18. Runtime/UI validation — REQUIRED

Device/runtime closure should prove:

- app appears in Sable Start All Apps/Search;
- launches normally;
- representative calculations work through touch UI;
- error recovery works;
- light/dark/accent integration works;
- no unexpected permission prompt appears;
- accessibility semantics are present for representative controls;
- no fatal exception during ordinary interaction;
- expected state behavior across activity/process recreation as specified.

## 19. Manifest/security audit — REQUIRED

Before R9 closure inspect the final manifest/package and confirm:

- package identity;
- version metadata;
- permissions;
- exported components;
- intent filters;
- services/providers/receivers;
- debuggable/build-type implications;
- signing identity appropriate to the build context.

Any permission or exported component not required by the documented Calculator behavior should be investigated.

## 20. Explicit first-release non-goals

Unless requirements are later updated, do not include in the first Sable Calculator milestone:

- scientific trigonometric/logarithmic functions;
- graphing;
- programmer/hex/binary modes;
- unit conversion;
- currency conversion;
- exchange-rate/network access;
- AI assistant features;
- cloud sync;
- user accounts;
- telemetry/analytics;
- widgets;
- Wear/desktop companion;
- arbitrary plugin system.

## 21. Pre-code decision checklist

Before creating Calculator implementation commits, this document or the future canonical Calculator requirements must answer:

```text
[ ] immediate-execution or expression-entry model?
[ ] exact supported first-release keys/operations?
[ ] percent included? exact semantics?
[ ] sign-change included? exact semantics?
[ ] backspace/editing included?
[ ] numeric representation?
[ ] precision/rounding/display policy?
[ ] division-by-zero/error text/recovery?
[ ] maximum input/expression length?
[ ] history included or excluded?
[ ] clipboard included or excluded?
[ ] state restoration/persistence behavior?
[ ] canonical repo/package/module names?
```

Do not answer these questions implicitly in code.

## 22. Definition of implementation-ready

Calculator is ready for source implementation when:

- all items in the pre-code decision checklist are explicit;
- R8 design/theme contract needed by the first UI is sufficiently defined/available;
- canonical repository/package/build ownership is selected;
- initial deterministic test vectors have expected outputs;
- the intended first-release non-goals remain explicit.

Until then, this document is a requirements scaffold, not authorization to invent missing Calculator behavior.