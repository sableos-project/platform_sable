# DESIGN-KF-C — Sable Tools Product Consolidation

Status: **accepted implementation-ready design contract**  
Date: **2026-10-03**

This contract resolves the overlap between the earlier "Sable Tools" diagnostic
direction and "Toolbox" hardware-utility design.

## Product decision

SableOS ships **one launcher-visible product: Sable Tools**.

```text
PRODUCT_NAME=Sable Tools
CANONICAL_PACKAGE=org.sableos.tools
SEPARATE_SABLE_TOOLBOX_APP=NO
SEPARATE_DIAGNOSTICS_LAUNCHER_APP=NO
ONE_COMMON_PRODUCT=YES
```

The earlier SableScreens "Toolbox" surface becomes the **Utilities** section of
Sable Tools.

Sable Tools has three top-level destinations:

```text
Utilities
Diagnostics
Reports
```

Optional Recently used / Favorites may appear as a small home section but do not
become a fourth authority domain.

## Ownership split

### Utilities

User-facing hardware-backed tasks:

```text
Compass
Bubble level
Plumb bob
Protractor
Picture hanging / level guide
Flashlight
Magnifier
Height estimate
Noise meter
Speedometer
Pedometer
IR Remote
hardware-backed quick utilities proven by device capability
```

### Diagnostics

Read-only inspection and qualification aids:

```text
Device/build/security identity
Keyboard event viewer
keylayout/KCM/profile summary
modifier/meta-state viewer
pointer/touch-surface state
display/input geometry
camera capability report
sensor inventory
radio/IMS status
network/IP/DNS/VPN summary
storage/slot/partition summary
application/process inventory where platform policy permits
critical text-entry test
SubScreen/attention capability status
```

### Reports

User-authorized export:

```text
device report
keyboard/input report
radio/network report
selected app inventory
selected logs/diagnostic state
bugreport handoff where platform allows
```

Reports must clearly identify sensitive content before export/share.

## Settings relationship

Settings owns configuration and policy.

Sable Tools may inspect, test and deep-link.

```text
SETTINGS_OWNS_CONFIGURATION=YES
TOOLS_DUPLICATE_POLICY_STORE=NO
TOOLS_MAY_DEEP_LINK_TO_SETTINGS=YES
TOOLS_MAY_SHOW_EFFECTIVE_STATE=YES
TOOLS_MUTATION_CONSOLE=NO
```

Examples:

- keyboard remapping/settings -> Settings > Keyboard & input;
- app display profiles -> Settings > Display > App display compatibility;
- notification policy -> Settings > Notifications;
- network access -> Settings-owned Network Manager;
- privacy permissions -> Android App info / Sable privacy detail.

## App Security relationship

Sable Tools does not create a second AppManager-style database.

It may summarize installed app/process facts and deep-link to App Security &
Privacy / Android controls.

```text
APP_SECURITY_AUTHORITY=EXISTING_SABLE_APP_SECURITY_MODEL
SECOND_APP_SECURITY_DATABASE=NO
TOOLS_PACKAGE_INVENTORY=READ_ONLY_SUMMARY
```

## Privilege model

Default:

```text
READ_ONLY_BY_DEFAULT=YES
NO_AMBIENT_ROOT_DAEMON=YES
NO_SHARED_UID_FOR_CONVENIENCE=YES
NO_RAW_DEVICE_NODE_ACCESS_FROM_UI=YES
NO_ACCESSIBILITY_AUTOMATION_FOR_PLATFORM_ACTIONS=YES
NO_ARBITRARY_SHELL_FRONTEND=YES
```

When privileged information/action is justified, the app calls a narrowly scoped
platform-owned service/API with explicit authorization. Broad privilege does not
accumulate in the UI app.

## Developer / service mode

A deeper diagnostics tier may be exposed only when developer/service mode is
explicitly enabled.

Developer mode may reveal:

```text
partition/slot detail
VINTF/vendor summary
raw-but-safe input identifiers
extended radio state
extended logs
unsupported/unproven capability status
```

Still prohibited:

```text
raw memory read/write
kernel/process patching
security/trust bypass
NVRAM mutation
raw block writes
unsigned flashing
hidden root shell
unauthenticated remote control/file server
```

## Capability gating

Utilities are visible only when the backing capability is proven for the active
device profile.

```text
CAPABILITY_UNKNOWN_DEFAULT=HIDDEN
CAPABILITY_UNSUPPORTED=HIDDEN
CAPABILITY_DIAGNOSTIC_ONLY=DEVELOPER_MODE_ONLY
CAPABILITY_SUPPORTED=VISIBLE
```

Titan 2, Titan 2 Elite and Q27 require independent evidence.

## IR Remote

IR Remote is a feature/activity within Sable Tools, not a separate mandatory
launcher package.

It may expose a pin/shortcut into Sable Start for frequent use.

```text
IR_REMOTE_IN_SABLE_TOOLS=YES
SEPARATE_REMOTE_PACKAGE_REQUIRED=NO
PIN_REMOTE_SHORTCUT_TO_START=ALLOWED
NETWORK_REQUIRED_FOR_REMOTE=NO
LEARNING_MODE=ONLY_IF_IR_RECEIVER_PROVEN
```

## FM Radio

FM Radio remains owned by Sable Media if hardware/vendor support is proven.

Sable Tools exposes only capability/diagnostic state and may deep-link to Media.

```text
FM_RADIO_OWNER=Sable_Media
TOOLS_FM_PLAYER=NO
TOOLS_FM_DIAGNOSTIC_STATUS=YES
```

## SubScreen

SubScreen integration is curated, non-editing and capability-gated.

Allowed Sable Tools companion modes include:

```text
compass
media quick control
safe IR quick control
camera/magnifier preview only if validated
battery/device status
```

No arbitrary app mirroring or text entry.

## Keyboard-first UX

Top level:

```text
printable key
  type-to-filter tool names when no text field is active

Up/Down/Left/Right
  move focus

Enter
  open

Menu / Fn+Enter
  actions/details

Back/Esc
  return one level

/
  explicit search

C
  calibrate, only within calibration-capable utility

R
  reset current measurement, only in tool context

H
  hold reading, only in measurement context

I
  info/details
```

Single-letter commands are local to a tool context and disabled while a text
field owns input.

## Home layout

Sable Tools home:

```text
title + device-profile summary
recent/favorite tools, optional bounded row
Utilities
Diagnostics
Reports
```

Do not show a grid of unavailable hardware features.

Capability badges may show:

```text
Available
Needs permission
Needs calibration
Developer only
Unavailable on this device
```

"Unavailable" appears only in diagnostics/developer views when useful.

## Permission behavior

Sensors/camera/microphone/location permissions are requested only when the user
opens a feature that requires them.

```text
SILENT_MIC_ACCESS=NO
SILENT_CAMERA_ACCESS=NO
SILENT_LOCATION_ACCESS=NO
PERMISSION_REQUEST_AT_FEATURE_USE=YES
NO_BROAD_PERMISSION_REQUEST_ON_FIRST_LAUNCH=YES
```

## Report privacy

Before export, show included categories and warnings.

Default report excludes:

```text
message bodies
contact contents
photos/media
authentication secrets
full private logs
precise location history
provider credentials
```

Sensitive optional sections require explicit user selection.

## Implementation packages

Suggested implementation split:

```text
TOOLS-I1  common shell, capability model, Utilities/Diagnostics/Reports IA
TOOLS-I2  read-only device/keyboard/display/radio diagnostics
TOOLS-I3  sensor utilities and local IR remote
TOOLS-I4  report/export and Settings/App Security handoff
TOOLS-I5  device-profile physical qualification
```

## Acceptance

```text
SABLE_TOOLS_SINGLE_PRODUCT=PASS
SEPARATE_TOOLBOX_APP=PASS_ABSENT
TOOLS_UTILITIES_SECTION=PASS
TOOLS_DIAGNOSTICS_SECTION=PASS
TOOLS_REPORTS_SECTION=PASS
SETTINGS_OWNS_CONFIGURATION=PASS
TOOLS_DUPLICATE_POLICY_STORE=PASS_ABSENT
TOOLS_READ_ONLY_DEFAULT=PASS
TOOLS_NO_ROOT_DAEMON=PASS
TOOLS_CAPABILITY_GATING=PASS
TOOLS_KEYBOARD_FIRST=PASS
TOOLS_PERMISSION_JUST_IN_TIME=PASS
TOOLS_REPORT_PRIVACY=PASS
FM_RADIO_OWNER_MEDIA=PASS
IR_REMOTE_LOCAL_FIRST=PASS
DEVICE_MODEL_BRANCHING_COMMON_UI=PASS_ABSENT
```
