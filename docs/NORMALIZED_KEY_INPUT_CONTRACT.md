# Normalized Keyboard Input Contract

Status: **accepted common keyboard-first platform contract**  
Date: **2026-10-03**

This contract closes the KF1/KF2 handoff between device-specific key evidence,
Android input/IME semantics and Sable-owned keyboard-first UI models.

## Layering

```text
physical key / scan code / vendor quirk
        |
        v
device profile + Android keylayout/KCM + bounded input adapter
        |
        v
Android KeyEvent / InputConnection / IME semantics
        |
        v
Sable normalized UI/navigation event
        |
        v
Sable Start / Setup / Weather / Settings / Hub / first-party apps
```

Common UI code never consumes raw scan codes or device model names.

## Ownership

```text
DEVICE_PROFILE
  owns observed hardware identity, scan-code quirks and capability evidence

ANDROID_KEYLAYOUT_KCM_INPUTREADER
  owns standard Android key-code and character normalization

IME_INPUTCONNECTION_FOCUSED_EDITOR
  owns text composition, dead keys, alternate characters and normal editor behavior

SABLE_KEYBOARD_POLICY
  owns explicitly reserved global Sable actions and lock-state routing

SABLE_UI_MODEL
  owns only context-local navigation/activation/command behavior after the
  focused editor/platform has had the correct right of refusal
```

## Normalized event fields

A common UI event may expose:

```text
semantic kind
platform key code, only where useful and public/device-independent
composed printable text or Unicode scalar where available
Shift
Ctrl
Alt
Meta
repeat state/count
event action when required
editable/focus ownership context
```

It must not expose a vendor scan code as application policy.

## Core semantic kinds

The V1 common vocabulary includes:

```text
Up
Down
Left
Right
Activate
Space
Back
Escape
Tab
Backspace
PageUp
PageDown
MoveHome
MoveEnd
Printable
Other
```

A Sable system/Home action is **not** inferred from an application-delivered
cursor-movement key. System/Home is a separate platform/host semantic action.

## HOME versus cursor Home

Android distinguishes:

```text
KEYCODE_HOME
  framework/system Home key; not an ordinary application navigation event

KEYCODE_MOVE_HOME
  Home Movement key; used for cursor/list movement to the start/top
```

Therefore:

```text
KEYCODE_MOVE_HOME -> MoveHome
KEYCODE_MOVE_END  -> MoveEnd
KEYCODE_HOME      -> never alias to MoveHome and never be synthesized as an
                     app-local root command by a generic mapper
```

Launcher/Home-host integration may create a Sable "return to Start root" action
through the canonical host/policy path. Generic app key mappers must not obtain
that action by conflating it with cursor Home.

## Editable-field precedence

A focused editor/InputConnection has first ownership of text and conventional
editing/navigation keys.

At minimum, Sable UI must not steal:

```text
printable text
Space
Backspace/Delete
Left/Right
Up/Down when the editor consumes them
MoveHome/MoveEnd
standard selection/edit chords
IME composition/dead-key sequences
```

Tab/Enter behavior is surface-specific only where Android/view behavior does not
already require the event and the UX contract explicitly defines traversal or
submission.

Canonical dispatch should prefer:

```text
focused View / editor / IME
 -> Sable contextual navigation
 -> application shortcut
 -> unhandled/default Android dispatch
```

Security/global reserved actions defined by Keyboard Policy remain above normal
application dispatch.

## Printable text

Common UI models may consume a printable character for type-to-search or command
entry only when no editable field owns input.

The canonical text path remains InputConnection/IME. The normalized navigation
event is not a replacement text protocol.

Dead-key/composition handling belongs to Android key mapping / IME. A Sable UI
must not independently reconstruct vendor Alt/Sym/dead-key composition.

## Modifiers

```text
Ctrl/Meta
  command shortcut candidates only where the surface explicitly binds them

Shift
  traversal/selection modifier according to focused context

Alt/Sym/Fn
  device profile + IME/platform ownership first
```

A device-specific Fn implementation must not leak into common app code.

## Repeat

Repeated key-down events are allowed for continuous navigation/scrolling where
the surface explicitly supports repeat.

One-shot actions must ignore repeat:

```text
launch/open
submit
destructive confirmation
Back/Escape transitions
toggle requiring one press
global reserved command
```

## Touch/pointer transition

Pointer hover never steals keyboard focus.

After touch/pointer activation, focus styling may temporarily hide according to
the common focus contract. The next keyboard navigation event restores visible
keyboard focus deterministically.

## Multi-device rule

Titan 2, Titan 2 Elite and Q27 each have independent hardware key evidence.

```text
COMMON_NORMALIZED_SEMANTICS=YES
COMMON_SCAN_CODE_TABLE=NO
DEVICE_PROFILE_REQUIRED=YES
DEVICE_MODEL_BRANCHING_IN_COMMON_APP=NO
```

## P1/P3/P4 handoff requirement

Parallel product-source models may implement pure/framework-free equivalents of
this contract, but canonical integration must bind them to the Android/platform
input path rather than copying raw key tables into each app.

P1 Sable Start, P3 SetupWizard flow and P4 Weather city management must consume
the same semantic boundary.

## Acceptance

```text
NORMALIZED_KEY_INPUT_CONTRACT=PASS
RAW_SCAN_CODES_IN_COMMON_UI=PASS_ABSENT
DEVICE_MODEL_KEY_ROUTING=PASS_ABSENT
KEYCODE_HOME_MOVE_HOME_CONFLATION=PASS_ABSENT
EDITABLE_TEXT_PRECEDENCE=PASS
IME_COMPOSITION_OWNERSHIP=PASS
CONTEXTUAL_SHORTCUTS=PASS
ONE_SHOT_REPEAT_GUARD=PASS
POINTER_HOVER_STEALS_FOCUS=NO
MULTI_DEVICE_PROFILE_SEPARATION=PASS
```
