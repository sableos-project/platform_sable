# Hardware diagnostics and dialer-code UX

Status: **design contract — diagnostics bridge**

Titan-class devices expose factory and engineering surfaces through vendor
mechanisms. On Titan 2, the dialer code `*#*#3377#*#*` opens a Factory Test
surface with hardware-testing categories.

The attached Titan 2 evidence showed:

```text
Factory Test
  Mtklog
  Ygps
  Gravity Calibration
  Distance Calibration
  Smartpa calib
  Single Test
  Aging Test

Single Test
  TouchPanel
  TouchPad
  RF information
  Smartpa calib
  LCD
  BackLED
  Key
  KB light
  Two lights flash
  Power slow charge
  Vibrator
  LoudSpeaker
  Receiver
  Microphone1
  Microphone2
  Gravity Sensor
  Gyro
  Compass
```

SableOS should learn from these factory-test categories, but it should not expose
raw vendor engineering tools as ordinary Settings panels.

## Where this belongs

Dialer codes and factory-test capabilities belong under:

```text
Settings > Diagnostics
Sable Tools > Diagnostics
Sable Command search result: Diagnostics
```

They do not belong as top-level user Settings items and should not be marketed as
a normal feature.

## Role split

```text
Sable Diagnostics
  user-safe, keyboard-operable, read-only or explicitly gated diagnostics

Vendor Factory Test
  OEM/engineering surface, may include calibration, logging or mutation actions

Dialer code
  compatibility or bridge entry point, not the primary UX contract
```

## Default policy

```text
RAW_DIALER_CODE_COMPATIBILITY=PRESERVE_WHERE_AVAILABLE
RAW_FACTORY_TEST_DIRECT_LAUNCH=NO_BY_DEFAULT
SABLE_DIAGNOSTICS_BRIDGE=YES
CALIBRATION_ACTIONS=GATED
LOGGING_ACTIONS=GATED
READ_ONLY_TESTS=ALLOWED_WITH_PROFILE_POLICY
```

Normal users should see Sable Diagnostics first. Engineering/factory surfaces can
be exposed only when the device profile marks them available and the action is
safe, read-only or explicitly gated.

## Sable Diagnostics categories

Map observed factory-test concepts into Sable-owned categories:

```text
Display
  LCD
  BackLED
  brightness / color / touch association where supported

Keyboard & pointer
  Key
  KB light
  TouchPad
  TouchPanel

Audio
  LoudSpeaker
  Receiver
  Microphone1
  Microphone2
  SmartPA status / calibration note where safe

Sensors
  Gravity Sensor
  Gyro
  Compass
  Distance / proximity where supported

Radio
  RF information
  YGPS / GNSS

Power
  Power slow charge
  charging status
  battery health where available

Attention
  Two lights flash
  vibrator
  keyboard backlight
  notification flash where supported

Engineering
  Mtklog
  Aging Test
  Factory Test bridge
```

## Keyboard-first test requirements

Diagnostic tests must be operable without touch unless the specific test is for a
touch surface.

```text
VISIBLE_FOCUS=REQUIRED
ARROW_NAVIGATION=REQUIRED
ENTER_START_TEST=REQUIRED
BACK_ESCAPE_EXIT_TEST=REQUIRED
HARDWARE_KEY_TEST=REQUIRED
SOFTWARE_KEYBOARD_FALLBACK_TEST=REQUIRED
```

The `Key` test is release-critical for keyboard-first devices. It should report:

```text
key label
scan code where available
Android keycode
modifier state
Alt/Sym/Fn/Shift/Ctrl behavior
repeat behavior
software keyboard fallback state
```

## Critical text-entry diagnostics

Because Titan 2 stock behavior showed a Bluetooth-pairing text-entry failure,
Sable Diagnostics must include a critical text-entry test.

```text
Critical text-entry test
  letters
  numbers
  symbols
  Alt
  Sym
  Fn
  Bluetooth pairing code simulation
  Wi-Fi password simulation
  lockscreen/PIN simulation
  software keyboard fallback
```

This is not optional for Titan-family release readiness.

## Search and dialer-code behavior

Settings search and Sable Command should understand both friendly names and the
observed code.

```text
query: factory test
query: hardware test
query: single test
query: key test
query: keyboard light
query: *#*#3377#*#*
```

Search result policy:

```text
friendly query -> Sable Diagnostics category
raw dialer code query -> Diagnostics > Factory test bridge
```

If the raw factory bridge is unavailable or intentionally disabled, Settings
should explain the reason instead of failing silently.

## Safety boundary

Factory test surfaces can contain calibration/logging/aging-test actions that may
change device state. Those must not be exposed as one-tap Settings results.

```text
CALIBRATION_FROM_NORMAL_SETTINGS=NO_BY_DEFAULT
AGING_TEST_FROM_NORMAL_SETTINGS=NO_BY_DEFAULT
MTKLOG_FROM_NORMAL_SETTINGS=NO_BY_DEFAULT
DIAGNOSTIC_EXPORT=YES
DEVICE_MUTATION_REQUIRES_EXPLICIT_ENGINEERING_GATE=YES
```

## Release gates

```text
TITAN2_DIAGNOSTICS_ENTRY=PASS_REQUIRED
TITAN2_KEY_TEST=PASS_REQUIRED
TITAN2_TOUCHPAD_TEST=PASS_REQUIRED
TITAN2_KB_LIGHT_TEST=PASS_REQUIRED
TITAN2_CRITICAL_TEXT_ENTRY_TEST=PASS_REQUIRED
TITAN2_BT_PAIRING_CODE_SIMULATION=PASS_REQUIRED
RAW_FACTORY_TEST_BRIDGE=GATED
```
