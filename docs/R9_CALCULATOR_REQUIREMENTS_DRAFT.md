# Sable Calculator requirements draft — historical R9 filename

> **HISTORICAL / SUPERSEDED MILESTONE DOCUMENT — 2026-09-24:** retained for design/requirements provenance. Panther R9 is accepted/frozen and the described R8/R9 execution sequencing is no longer current. Consult this repository's README/ARCHITECTURE plus organization current-status docs for current product state.


Status: **HISTORICAL / SUPERSEDED MILESTONE DOCUMENT — retained for requirements/design provenance.**

The original document predated the consolidated R8 application plan and called Calculator an R9 application. That milestone assignment is superseded: Sable Calculator + Convert now belong to **R8-B**.

The central anti-drift rule is unchanged: unresolved Calculator interaction semantics must not be invented in source merely because an implementation can choose a reasonable behavior.

## 1. Product intent — DECIDED

Sable Calculator is a low-privilege, offline-first Sable-owned utility used to prove the repeatable standalone-application -> exact-artifact-freeze -> SableOS-product-integration model.

Core requirements:

- Android/Compose UI;
- shared R8-A design contract;
- deterministic/testable arithmetic domain;
- no network permission for ordinary calculator operation;
- no location/contacts/calendar/media/usage/accessibility-service authority for ordinary operation;
- no analytics/tracking/cloud account dependency;
- host/application tests before Panther product integration.

## 2. Rust/Kotlin boundary — DECIDED

A portable Rust arithmetic core is acceptable/preferred where it remains interaction-neutral and materially improves deterministic testing/reuse.

Kotlin/Android owns:

- Activity/application lifecycle;
- Compose UI and accessibility;
- text/button/input semantics;
- saved UI state as requirements eventually define it;
- Android integration.

Rust may own:

- exact finite-decimal/rational parsing/representation;
- checked arithmetic primitives;
- deterministic arithmetic errors/results;
- future pure conversion/math domains when explicitly required.

JNI must remain narrow and must be qualified end-to-end before product freeze.

## 3. Arithmetic primitives allowed before UI semantics are frozen — DECIDED

The domain core may provide exact/checked primitives for:

```text
+
-
multiply
divide
```

and exact finite-decimal parsing/format-independent result representation.

Providing these primitives does **not** decide the calculator's expression-entry model.

## 4. Interaction model — TBD

Do not silently choose any of the following as normative behavior until documented:

- immediate-execution vs expression-entry model;
- operator precedence;
- parentheses;
- repeated-equals semantics;
- chaining behavior after equals;
- operator replacement rules;
- keyboard/hardware-key mappings beyond basic accessible input expectations;
- swipe/gesture shortcuts.

Tests for one temporary UI prototype do not make that behavior a product requirement.

## 5. Numeric/display policy — TBD

Must be decided before shipping behavior becomes normative:

- maximum user-entered digits;
- maximum displayed digits;
- decimal separator/localization policy;
- scientific notation threshold;
- rounding mode;
- recurring/non-terminating division display;
- overflow/underflow presentation;
- negative zero handling if relevant;
- precision policy for very large/small values.

The Rust domain may remain exact internally while UI formatting stays TBD.

## 6. Percent / sign / special keys — TBD

Explicit requirements are still needed for:

- percent semantics;
- sign-toggle semantics;
- clear vs all-clear;
- backspace/delete behavior;
- memory keys;
- square root/powers/scientific functions;
- constants;
- copy/paste behavior;
- vibration/sound feedback.

Do not add scientific/programmer functions merely because the architecture supports them.

## 7. History / persistence — TBD

The first release may or may not contain calculation history. Before adding it, decide:

- whether history exists;
- persistence duration;
- user clear controls;
- backup/data-extraction behavior;
- whether history is included in search/share/copy flows.

No history store should be added just because it is easy.

## 8. Convert relationship — CURRENT DIRECTION

R8-B also contains unit conversion. Portable conversion logic may be independently qualified and the Android UI may currently be a separate application/module during development.

The final product decision on whether Convert is:

```text
separate Sable Convert app
or
surface inside Sable Calculator
```

remains open unless another current product document closes it. Standalone qualification does not need to wait for that packaging decision.

## 9. Accessibility — REQUIRED

The accepted UI must provide:

- meaningful semantics/content descriptions where visual symbols are used;
- sensible TalkBack traversal;
- readable large-font behavior;
- sufficient contrast under R8-A modes/accents;
- touch targets appropriate for ordinary calculator use;
- deterministic disabled/error state semantics.

## 10. Error behavior — PARTLY DECIDED

The domain must fail safely for invalid/undefined operations such as division by zero and arithmetic overflow where the chosen representation can overflow.

Exact user-visible wording, recovery and subsequent-key behavior remain UI requirements to be decided/tested.

A panic/crash is not an accepted user-visible arithmetic error path.

## 11. Qualification gates

Before the Calculator artifact can enter the R8 integration freeze:

```text
Rust fmt/clippy/tests PASS
Rust dependency/security gate PASS where applicable
Android/JVM tests PASS
Gradle APK compile PASS
Android lint/static PASS
JNI/native Android ABI build PASS if JNI is used
representative Kotlin -> JNI -> Rust behavior PASS
APK package/permission/component inspection PASS
APK native-library inventory PASS
APK SHA-256 sealed
source/workflow/dependency provenance recorded
```

No Panther full image build is needed to discover ordinary app compile/unit-test/JNI packaging failures.

## 12. Product integration gate

After qualification freeze, separately prove:

```text
sealed Calculator APK
 -> product import/module
 -> product selection
 -> PRODUCT_OUT path
 -> installed-file/target-files/image membership
 -> runtime package identity
 -> launcher/default/product behavior required by the accepted scope
```

A qualified APK is not automatically a product replacement/default.

## 13. Open decisions checklist

Before first shipping Calculator behavior is frozen, explicitly close or defer:

```text
interaction model
precedence/parentheses
percent
repeated equals
precision/display/rounding
clear/backspace semantics
history/persistence
Convert packaging relationship
scientific/programmer scope
copy/paste/share
localization details
```

Until then, the interaction-neutral exact Rust primitives are intentionally narrower than the final product.

Historical Git history preserves the original longer R9 draft and its earlier milestone wording.