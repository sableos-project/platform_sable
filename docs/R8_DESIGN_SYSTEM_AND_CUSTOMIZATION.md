# R8-A — Sable design system and customization foundation

> **HISTORICAL / SUPERSEDED MILESTONE DOCUMENT — 2026-09-24:** retained for design/requirements provenance. Panther R9 is accepted/frozen and the described R8/R9 execution sequencing is no longer current. Consult this repository's README/ARCHITECTURE plus organization current-status docs for current product state.


Status: **normative architecture and product requirements for the shared Sable visual/customization layer.**

R8-A is the design/customization workstream inside the broader R8 native-application foundation defined by `SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`. It exists before the R8 application family is integrated so Sable Start, Calculator, Convert, Games, Reader, Media, and future UI do not each invent their own colors, dimensions, preference keys, and behavior.

R8-A is not permission to redesign every screen or create a broad theming marketplace. The first release should establish a stable shared contract and a small, useful user-facing settings surface.

## 1. Goal

Provide one common Sable design/customization model that can be consumed by Sable-owned applications across supported devices and Android substrate revisions.

The model must:

- separate semantic product intent from raw Android/Compose values;
- support system-following, light, and dark appearance modes;
- support a bounded accent choice;
- persist Sable-owned customization state safely;
- allow future Sable applications to consume the same theme without copying constants;
- remain portable across device repositories;
- not require privileged platform access for ordinary visual preferences;
- remain accessible and testable.

## 2. Architecture boundary

Preferred conceptual layering:

```text
Sable application UI
       |
       | semantic Sable tokens/components
       v
Sable design contract
       |
       | resolved appearance/customization state
       v
Android/Compose theme primitives
```

Do not make device repositories authoritative for Sable colors, typography, or application spacing.

Do not make `vendor_sable` a storage location for application UI constants. Product overlays may choose platform resources where appropriate, but Sable application design contracts belong in common Sable code.

## 3. Required semantic token groups

### 3.1 Color

Define semantic roles instead of screen-specific literal color names. Expected roles include, where needed:

- background;
- surface;
- elevated/alternate surface;
- primary text;
- secondary text;
- disabled text/content;
- accent/primary action;
- on-accent content;
- positive/success;
- warning;
- destructive/error;
- separators/outlines;
- selection/focus indication;
- scrim/overlay where used.

A semantic role can resolve differently in light/dark modes.

Avoid names such as `startBlue3` or `calculatorButtonGray2` in the common contract unless the value is genuinely application-specific and cannot be semantic.

### 3.2 Typography

Define roles rather than raw sizes scattered through applications. At minimum consider:

- display/hero;
- screen title;
- section title;
- body primary;
- body secondary;
- label;
- button/action;
- caption/metadata;
- numeric/display role for Calculator if a shared role is justified.

Respect Android font scale. Do not hard-code layouts that only work at the default text size.

### 3.3 Spacing

Define a small consistent spacing scale used for:

- screen margins;
- section spacing;
- component internal padding;
- list row spacing;
- icon/text gaps;
- compact/comfortable layout choices later.

The goal is consistency, not a large token taxonomy.

### 3.4 Shapes

Define a bounded shape/corner scale for:

- cards/surfaces;
- buttons/actions;
- fields/search surfaces;
- dialogs/sheets if used.

Do not let each application pick unrelated corner radii.

### 3.5 Icon treatment

Document:

- recommended icon sizes by component role;
- tint rules for monochrome Sable action icons;
- when full-color application icons must retain their original appearance;
- accessibility/content-description rules;
- profile/app icon badging behavior where Android supplies it.

Sable theme must not recolor arbitrary third-party app icons in a way that destroys app identity unless a future icon-treatment feature explicitly defines that behavior.

### 3.6 Motion

Define a small motion policy rather than application-specific animations:

- default transition duration ranges;
- reduced-motion behavior if/when the platform accessibility setting can be respected;
- avoid animation as a prerequisite to understand state;
- no continuous decorative animation that wastes power by default.

Motion can remain minimal in the first R8 implementation.

## 4. Appearance modes

R8 must support exactly these baseline modes:

```text
Follow system
Light
Dark
```

`Follow system` should be the safe initial/default behavior unless an existing product decision states otherwise.

The mode must be Sable product state, not a per-screen local toggle.

Switching mode should update active Sable UI predictably without requiring process restart where practical.

## 5. Accent selection

R8 should define an accent abstraction rather than scattering one literal accent color.

First-release constraints:

- bounded curated accent choices or one well-defined platform-derived accent mode;
- every accent must be checked against required foreground/background combinations;
- accent choice must not reduce critical text/control contrast below the accepted accessibility baseline;
- destructive/error semantics must remain distinguishable from ordinary accent choices;
- the user must be able to return to the default accent.

Dynamic wallpaper-derived color may be evaluated later. It is not automatically required for first R8 closure.

## 6. Persistence

Theme/customization preferences must have:

- one documented owner;
- stable typed keys/schema;
- safe defaults when data is absent/corrupt;
- migration strategy if schema changes;
- no sensitive-data implication;
- no network dependency.

Do not define separate `darkMode`/`accent` preference stores independently in every Sable app.

The exact cross-application persistence mechanism must be selected deliberately. Options may include a common settings provider/service or another supported product-owned mechanism. If the architecture introduces a shared privileged service, justify why a normal application-scoped mechanism is insufficient before adding privilege.

Until cross-app synchronization is intentionally designed, avoid prematurely creating a broad privileged settings daemon.

## 7. Initial user-facing customization scope

The first R8 settings surface should expose only:

- appearance: Follow system / Light / Dark;
- accent choice;
- reset-to-default where appropriate.

The following are candidates for later work and **not first-R8 requirements**:

- launcher density;
- app-grid column count;
- tile sizing;
- icon shapes/packs;
- background/wallpaper editor;
- greeting customization;
- greeting time-bucket customization;
- clock format override;
- animation level controls;
- complex per-app themes;
- downloadable theme packs;
- font-family packs.

Do not block daily-driver progress by turning R8 into a complete launcher personalization suite.

## 8. Relationship to R6 greeting

R6 owns the initial deterministic greeting behavior:

```text
05:00–11:59  Good morning
12:00–16:59  Good afternoon
17:00–21:59  Good evening
22:00–04:59  Good night
```

R8 may create the architectural place where greeting preferences could later live, but R8 must not silently change the R6 greeting text or bucket policy. Any new greeting customization needs explicit requirements.

## 9. Relationship to Android system theme

Sable design should cooperate with Android rather than duplicate platform authority.

Examples:

- `Follow system` consumes the platform light/dark state;
- font scale/accessibility settings remain Android-owned;
- system bars should be made legible with the active Sable scheme using supported APIs;
- Sable-specific accent/theme state remains Sable-owned unless a later requirement intentionally maps it to Android dynamic color.

Do not attempt to globally skin arbitrary third-party applications through unsupported mechanisms.

## 10. Accessibility requirements

At minimum:

- semantic text/content contrast must meet the project's accepted Android accessibility target;
- focus/selection state cannot rely only on subtle color differences;
- controls must expose accessible labels/roles;
- font scaling must not cause major functional controls to disappear at practical supported sizes;
- touch targets should follow accepted Android guidance;
- light and dark variants must both be tested;
- every offered accent must remain usable in both required appearance modes.

Accessibility failures are correctness failures, not post-R8 polish.

## 11. Test requirements

### 11.1 Unit/host tests

Where possible test:

- appearance mode resolution;
- preference serialization/default/migration behavior;
- accent selection mapping;
- token invariants that can be expressed programmatically;
- invalid/corrupt preference fallback.

### 11.2 UI/runtime tests

Prove at least:

- Follow system responds to system appearance state;
- Light forces Sable UI light appearance;
- Dark forces Sable UI dark appearance;
- accent changes are reflected in at least Sable Start and the first additional Sable app once available;
- setting persists across process restart;
- no crash when preference data is absent;
- system bars/content remain legible;
- representative screens remain usable with increased font scale.

### 11.3 Cross-app proof

R8's design contract is not considered complete merely because Sable Start can theme itself. Once Sable Calculator exists, at least one closure gate should prove the same token/settings contract is consumed by more than one Sable application.

## 12. Dependency rules for future Sable apps

New Sable applications after R8 should:

- consume shared Sable tokens/contracts rather than copy values;
- declare any application-specific visual token separately and explain why it is not common;
- reuse the common appearance/customization state where designed;
- not create incompatible per-app `Light/Dark` semantics without explicit product reason.

## 13. Portability

Theme behavior must not be Panther-specific.

Device-specific code may be required for platform integration edge cases, but common appearance semantics must work across supported devices/substrates.

Avoid using a vendor property, device codename, or Panther resource to decide ordinary Sable colors/layout.

## 14. Performance/power

Theme changes should not cause continuous polling or unnecessary background work.

Do not use high-frequency wallpaper/color sampling or continuous animation to maintain theme state.

## 15. Security/privacy

Basic theme/customization must not require:

- location;
- network;
- contacts;
- media access;
- phone/SMS permission;
- broad system package authority.

If wallpaper-derived color is added later, use the least authority necessary and document data access.

## 16. Definition of R8-A done

The R8-A design workstream is done when:

- the common design/theme contract is documented and implemented in common Sable code;
- Sable Start uses the shared semantic tokens;
- Follow system / Light / Dark work and persist;
- bounded accent selection works and persists;
- accessibility checks cover representative token combinations;
- no device-specific fork owns the common design semantics;
- the same contract is consumable by the R8 application workstreams without copying theme constants;
- no unapproved broad customization features were pulled into the milestone.

R8-A completion is not the full R8 integration closure. Calculator/Convert, Games, Reader and Media proceed as independently qualified R8 workstreams under `SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`, followed by the deliberate R8 product-integration freeze and image build.
