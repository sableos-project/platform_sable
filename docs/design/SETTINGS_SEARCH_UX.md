# Settings search UX

Status: **design contract — required for keyboard-first devices**

Settings search is a release-critical UX surface for Titan-class devices. The
stock Titan 2 experience showed that merely having a Settings search field is not
enough: users must be able to find hardware, keyboard, display, SubScreen,
attention and diagnostics controls by the words they naturally use.

```text
SETTINGS_SEARCH=REQUIRED
SEARCH_INDEX_QUALITY=RELEASE_GATE
KEYBOARD_FIRST_SEARCH=REQUIRED
```

## Search entry points

Settings search must be reachable from:

```text
Settings top bar
Sable Command
Sable Start search result handoff
Hardware keyboard shortcut where configured
```

On keyboard-first devices, printable typing from the Settings root may focus the
search field when no text field is already active.

## Search scope

Search must index both Android-familiar names and Sable/device-profile names.

```text
Network & internet
Connected devices
Apps
Notifications
Battery
Display
Sound & vibration
Keyboard & input
Attention & SubScreen
Privacy
Security & emergency
Location
System
Diagnostics
About phone
```

## Alias requirements

The search index must include aliases, not just visible row titles.

Examples:

```text
keyboard
physical keyboard
hardware keyboard
key test
keys
alt
sym
fn
function key
keyboard backlight
KB light
touchpad
touch pad
trackpad
mouse
pointer

subscreen
sub screen
rear screen
back screen
external display
glance display
notification display

always on display
AOD
ambient display
doze
notification pulse
attention
LED
flash
haptics
vibration

bluetooth pairing
pairing code
software keyboard
critical text entry
setup text entry
password entry
PIN entry

factory test
hardware test
single test
diagnostics
sensor test
LCD
BackLED
microphone
speaker
receiver
gravity sensor
gyro
compass
RF information
Mtklog
YGPS
```

Aliases should map to stable settings destinations, not to undocumented raw
activities unless the target is a gated diagnostic bridge.

## Result behavior

Each result should show enough context to avoid ambiguity.

```text
Keyboard & input > Physical keyboard > Key test
Attention & SubScreen > Rear SubScreen > Notification privacy
Diagnostics > Hardware tests > Microphone test
Display > App layout compatibility
```

When a searched capability is unavailable on the active device profile, the
result should explain why instead of disappearing silently where possible.

Example:

```text
Always-on display
Unavailable on Titan 2 profile: LCD panel / no validated low-power doze mode.
```

## Keyboard navigation

Search results must support:

```text
arrow navigation
Enter open
Back/Escape return
Ctrl/F or configured shortcut to refocus search
focus restoration after returning from a result
```

## Device-profile awareness

Search ranking should prefer the active hardware profile.

```text
Titan 2:
  keyboard, SubScreen, critical text entry, Diagnostics and Display compatibility
  rank highly

Titan 2 Elite / Q27:
  keyboard, pointer/touch surface, AOD candidate controls, Display compatibility
  rank highly
```

## Diagnostics search

Factory-test and dialer-code concepts should be searchable, but normal users
should land on Sable Diagnostics, not raw engineering menus.

```text
query: *#*#3377#*#*
result: Diagnostics > Factory test bridge
state: gated / engineering surface

query: factory test
result: Diagnostics > Hardware tests
state: read-only or explicitly gated
```

## Privacy and safety

Settings search must not expose hidden privileged actions merely because they are
indexed. Sensitive actions should be discoverable only as explanations or gated
entry points.

```text
RAW_FACTORY_ACTIVITY_DIRECT_LAUNCH=NO_BY_DEFAULT
PRIVILEGED_MUTATION_FROM_SEARCH=NO
READ_ONLY_DIAGNOSTICS_FROM_SEARCH=YES
EXPORTABLE_REPORT_FROM_SEARCH=YES
```

## Acceptance

```text
ANDROID_SECTION_SEARCH=PASS
SABLE_PROFILE_SEARCH=PASS
ALIAS_SEARCH=PASS
KEYBOARD_NAVIGABLE_RESULTS=PASS
UNAVAILABLE_CAPABILITY_EXPLANATION=PASS
DIALER_CODE_SEARCH_BRIDGE=PASS
RAW_FACTORY_TEST_DIRECT_LAUNCH=NO_BY_DEFAULT
```
