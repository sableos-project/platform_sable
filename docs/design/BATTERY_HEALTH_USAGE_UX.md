# Battery usage, health and charging UX

Status: **design contract — keyboard-first Settings extension**  
Date: **2026-10-04**

This document defines the SableOS Battery experience for Titan 2, Titan 2 Elite,
Q27 and the touch-first Panther reference. It extends Android's familiar
**Settings → Battery** destination rather than introducing a separate launcher
application.

The product goal is to make battery usage, battery health and charging behavior
understandable without requiring a third-party application, while remaining
truthful about what each device can actually report or control.

```text
ANDROID_SETTINGS_BATTERY_LOCATION=PRIMARY
SEPARATE_BATTERY_LAUNCHER_APP=NO
KEYBOARD_FIRST_NAVIGATION=REQUIRED
TOUCH_SECONDARY_ON_KEYBOARD_DEVICES=YES
DEVICE_CAPABILITY_GATING=REQUIRED
FABRICATED_BATTERY_HEALTH_VALUES=NO
DIRECT_COMMON_UI_SYSFS_ACCESS=NO
LOCAL_ONLY_ANALYTICS=YES
```

## 1. Product principles

Battery UI is a system configuration and diagnostics surface, not a scorecard
designed to produce reassuring numbers.

SableOS must prefer:

- measured platform/vendor facts over inferred values;
- explicit **Unavailable** over guessed capacity, cycle count or battery age;
- source and confidence labeling for derived estimates;
- Android-compatible Settings placement and terminology;
- one-handed physical-keyboard navigation on square displays;
- local processing with no cloud battery analytics;
- bounded device adapters for hardware-specific charging controls.

The UI must never imply that Titan 2, Titan 2 Elite, Q27 or Panther share the
same battery-health backend.

## 2. Information architecture

The Android-compatible top-level row remains:

```text
Settings
  Battery
```

The Battery landing screen is list-first on keyboard devices:

```text
Battery
  current level / charging state / time estimate when available
  Battery usage
  Battery health
  Charging & protection
  Battery saver
```

The four Sable-enhanced destinations are:

```text
Overview
Usage
Health
Charging & protection
```

These are sections within Android Settings, not independent applications.

## 3. Overview

The first screen must answer the questions a user usually has immediately:

```text
How much charge is left?
Am I charging?
How long until full / empty, if Android can estimate it?
Is anything consuming unusually high power?
Is battery health data available?
Are any protection features active?
```

Recommended square-display layout:

```text
Battery                          74%
Charging · 38 min until full

> Battery usage
  Today · Screen 24% · Mobile network 17%

  Battery health
  Partial data · temperature available

  Charging & protection
  Device controls not yet qualified

  Battery saver
  Off
```

The landing screen should avoid a tall dashboard of large decorative cards.
Titan-class displays need density, fast scanning and a deterministic focus path.

## 4. Battery usage

Battery usage should build on Android platform accounting rather than a
Sable-specific background profiler.

Required presentation, where platform data exists:

- battery drain over a recent period;
- screen-on time;
- system/component consumers such as Screen and Mobile network;
- application consumers;
- foreground/background contribution where Android exposes it reliably;
- time range selection;
- abnormal-use insight only when a baseline can be justified.

Recommended ranges:

```text
Today
24 hours
7 days
```

The UI must label the accounting window. Percentages from different accounting
models must not be mixed as if they have the same denominator.

### 4.1 Usage detail row

Example row:

```text
Mobile network                      17%
2 h 04 min active
```

Enter opens detail. Detail may show:

- foreground/background time;
- wakelock or radio contribution only when Android exposes a supported metric;
- battery optimization state;
- background restriction state;
- shortcut to the app's standard Android battery controls.

SableOS must not invent an opaque "battery score" for applications.

## 5. Battery health

Battery health is explicitly **availability-aware**.

Potential fields:

```text
maximum/full-charge capacity
design capacity
cycle count
battery temperature
battery status / charging condition
charge counter
manufacturing date, only when trustworthy
first-use date, only when trustworthy
```

Each field has:

```text
value
availability
source class
confidence class
sample timestamp where meaningful
```

Recommended source classes:

```text
ANDROID_FRAMEWORK
HEALTH_HAL
VENDOR_SUPPORTED_INTERFACE
DERIVED_LOCAL_ESTIMATE
UNAVAILABLE
```

Recommended confidence classes:

```text
MEASURED
PLATFORM_REPORTED
DERIVED_HIGH_CONFIDENCE
DERIVED_LOW_CONFIDENCE
UNAVAILABLE
```

### 5.1 Capacity health

A percentage such as "92% maximum capacity" is shown only when SableOS has a
defensible design-capacity and current full-charge-capacity relationship or a
platform/vendor value with equivalent meaning.

If that is not available:

```text
Maximum capacity
Not reported by this device
```

Do not infer a polished health percentage from a small number of charge samples.

### 5.2 Cycle count

Cycle count is displayed only when the platform/vendor reports a usable cycle
count or when a future Sable estimator has an independently reviewed,
persistent, local accumulation model.

Until then:

```text
Cycle count
Not reported
```

### 5.3 Battery age

Do not estimate a precise battery age merely from cycle count and user behavior.

Manufacture/first-use dates may be shown if they are trustworthy. Otherwise the
field is omitted.

## 6. Temperature

Current battery temperature is useful but should not encourage obsessive
monitoring.

The default Health screen may show:

- current temperature;
- a bounded recent trend when locally retained;
- a simple normal/warm/hot classification based on device-independent safety
  guidance plus device-specific limits where the platform exposes them.

SableOS must not promise that a fixed temperature threshold is universally safe
for every battery chemistry/device.

History is local-only and should use a bounded retention window.

## 7. Charging & protection

Charging controls are capability-gated.

Candidate controls include:

```text
Charge limit
Adaptive charging
Overnight / scheduled charging
Thermal charging protection
Battery saver automation
Extreme battery saver
```

A control is enabled only when there is a validated backend.

```text
CHARGE_LIMIT_VISIBLE=PROFILE_CAPABILITY_DEPENDENT
CHARGE_LIMIT_WRITABLE=VALIDATED_BACKEND_REQUIRED
ADAPTIVE_CHARGING=VALIDATED_BACKEND_REQUIRED
THERMAL_PROTECTION=PLATFORM_OR_VENDOR_OWNED
RAW_SYSFS_WRITE_FROM_COMMON_UI=NO
```

For an unqualified Titan 2 backend, the preferred UI is:

```text
Charge limit
Not available on this device yet
```

rather than a nonfunctional 80% toggle.

The common Settings UI must never write arbitrary vendor sysfs/procfs nodes.
Any future mutable device backend belongs behind a bounded device adapter with
explicit validation, SELinux ownership and negative tests.

## 8. Relationship to the frozen S1.7 power API

The historical S1.7 contract remains unchanged:

```text
com.sableos.power.IPowerService/default
    getLatestState() -> PowerSnapshot
```

That contract owns a narrow read-only semantic snapshot for battery percentage,
charging state and stable source enum. Battery usage/health work must not
silently expand that frozen API.

Conceptual composition:

```text
S1.7 PowerSnapshot
  current semantic power state
        +
Android BatteryStats / Settings accounting
  usage history and consumers
        +
Health/framework/vendor capability adapter
  health facts
        +
device charging-control adapter
  optional validated mutations
        =
Sable Settings Battery experience
```

A new privileged service is not justified merely to make the UI convenient.
Prefer existing Android framework/Settings services and narrow adapters.

## 9. Keyboard-first navigation

The Battery surface consumes the normalized keyboard input contract.

### 9.1 Base navigation

```text
Up / Down
  move deterministic row focus

Enter
  open focused row / confirm focused action

Space
  toggle focused switch when the control supports direct toggle

Back / Escape
  return to previous screen or leave a temporary inspection mode

PageUp / PageDown
  page-scroll long lists

MoveHome / MoveEnd
  first / last focusable row where appropriate

/
Search key / Ctrl+K or Command+K where available
  Settings search
```

System HOME is never conflated with MoveHome.

### 9.2 Type-ahead

When no editor is focused, printable keys participate in Settings type-ahead
focus navigation.

Examples:

```text
U -> Battery usage
H -> Battery health
C -> Charging & protection
B -> Battery saver
```

This uses the existing Settings type-ahead contract rather than introducing
Battery-specific raw-key handling.

### 9.3 Segmented/range controls

A time-range control such as Today / 24 hours / 7 days is one focusable group.

```text
Enter
  enter/select the group

Left / Right
  move between values

Enter / Space
  confirm value

Escape / Back
  leave group without trapping focus
```

### 9.4 Chart inspection mode

Charts must not be keyboard dead zones.

When the chart is focused:

```text
Enter
  enter inspection mode

Left / Right
  move previous/next time bucket

Up / Down
  move between series only if more than one meaningful series exists

PageUp / PageDown
  larger time jump where useful

Escape / Back
  exit inspection mode and restore focus to the chart row
```

A textual value/label for the selected bucket is always exposed. Chart color is
never the only information channel.

## 10. Focus and square-display layout

Titan 2 starts with:

```text
DISPLAY_CLASS=SQUARE_KEYBOARD
KEYBOARD=PRIMARY_INPUT
TOUCH=SECONDARY_INPUT
```

The Battery UI therefore requires:

- visible high-contrast focus;
- a single-column list by default;
- bounded summary content above the fold;
- no horizontally scrolling card carousel;
- no gesture-only chart controls;
- no bottom navigation bar that competes with the physical keyboard;
- stable focus restoration after opening/closing detail.

Two-pane layouts are allowed only when the active display profile proves enough
usable width and height.

## 11. Touch and pointer

Touch remains a complete secondary path. Pointer hover does not steal keyboard
focus.

The existing Settings last-input-wins contract applies:

```text
POINTER_HOVER_STEALS_KEYBOARD_FOCUS=NO
POINTER_CLICK_MOVES_FOCUS_AND_ACTIVATES=YES
NEXT_KEYBOARD_NAV_RESTORES_VISIBLE_FOCUS=YES
```

## 12. Search aliases

Settings search should index:

```text
battery
battery usage
battery drain
screen on time
background use
battery health
capacity
maximum capacity
cycle count
temperature
charging
charge limit
adaptive charging
overnight charging
battery saver
power
```

Unavailable hardware controls may appear in search only if the result clearly
states they are unsupported/unqualified on the active device.

## 13. Accessibility

Required:

- screen-reader labels for every metric and chart;
- high-contrast focus not dependent on accent color alone;
- text alternative for charts;
- font-scale resilience;
- no health state communicated only by green/yellow/red;
- touch targets follow Android guidance;
- keyboard-only completion of every action;
- unsupported controls expose disabled/unavailable semantics.

## 14. Privacy

Battery analysis is local by default.

```text
NETWORK_REQUIRED=NO
CLOUD_BATTERY_ANALYTICS=NO
ADVERTISING_PROFILE_USE=NO
PER_APP_BATTERY_HISTORY_EXPORT=USER_INITIATED_ONLY
```

Battery usage can reveal application habits. Exported diagnostic reports must
use the project's existing redaction/privacy rules.

## 15. Power/performance

The battery feature must not materially worsen the thing it measures.

Do not add:

- high-frequency polling;
- always-running chart collectors;
- decorative continuous animation;
- per-app background observers duplicating Android accounting.

Prefer event/callback/platform history and bounded sampling.

## 16. Device-profile capability schema

Battery-specific profile facts should be modeled generically:

```text
SableBatteryProfile:
  health_capacity_supported
  health_cycle_count_supported
  health_temperature_supported
  health_charge_counter_supported
  health_manufacturing_date_supported
  usage_platform_accounting_supported
  charge_limit_supported
  charge_limit_values
  adaptive_charging_supported
  scheduled_charging_supported
  thermal_protection_status_supported
  backend_provenance
  mutable_backend_qualified
```

Titan 2, Titan 2 Elite, Q27 and Panther each require independent evidence.

## 17. Initial implementation priority

V1 should prioritize read-only truth before charging mutation.

```text
BH1  Battery overview + keyboard-first navigation
BH2  Usage/accounting + app/system consumers
BH3  Health availability/source model
BH4  Local bounded temperature/health history where justified
BH5  Charging/protection capability discovery
BH6  Mutable charging controls only for validated device backends
BH7  physical keyboard/touch/accessibility/runtime qualification
```

BH1-BH4 can ship without BH6.

## 18. Acceptance

```text
BATTERY_SETTINGS_LOCATION_ANDROID_COMPATIBLE=PASS
SEPARATE_BATTERY_APP=PASS_ABSENT
BATTERY_KEYBOARD_NAVIGATION=PASS
BATTERY_VISIBLE_FOCUS=PASS
BATTERY_TYPE_AHEAD=PASS
BATTERY_CHART_KEYBOARD_INSPECTION=PASS
BATTERY_TOUCH_FALLBACK=PASS
BATTERY_USAGE_PLATFORM_ACCOUNTING=PASS
BATTERY_HEALTH_UNAVAILABLE_STATE=PASS
FABRICATED_HEALTH_PERCENTAGE=PASS_ABSENT
RAW_SYSFS_COMMON_UI_ACCESS=PASS_ABSENT
CHARGING_CONTROL_CAPABILITY_GATED=PASS
NETWORK_REQUIRED=PASS_ABSENT
CLOUD_ANALYTICS=PASS_ABSENT
DEVICE_PROFILE_SEPARATION=PASS
S1_7_FROZEN_API_MUTATION=PASS_ABSENT
```

Implementation and physical/runtime PASS remain separate from this design
contract.
