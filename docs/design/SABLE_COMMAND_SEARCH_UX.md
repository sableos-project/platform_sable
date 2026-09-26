# Sable Command / Search keyboard-first UX

Status: **design contract — keyboard-first command/search surface**

Sable Command is the global keyboard-first command and search surface. It gives
Titan 2 and other keyboard devices a fast, predictable way to launch apps, open
Settings, search Hub content, start calls/messages, run safe actions, and jump to
system surfaces without turning every screen into a separate search model.

This document is a design and interaction contract only.

```text
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Product role

Sable Command is not a separate launcher replacement. It is a shared command
layer used by Sable Start, All Apps, Settings, Hub and other Sable apps.

```text
SABLE_COMMAND_SURFACE=YES
GLOBAL_COMMAND_SEARCH=YES
LOCAL_CONTEXT_COMMANDS=YES
KEYBOARD_FIRST=YES
TOUCH_SECONDARY=YES
PROVIDER_SAFE_ACTIONS=YES
NO_UNSUPPORTED_PROVIDER_ACTIONS=YES
```

Command must feel like one system-wide behavior, not a different search design in
each app.

## Entry points

Command must be reachable without hunting for a small touch target.

```text
Start center page
  base quick bar Command slot
  / key
  Search key where present
  type-to-command when no row consumes text

All Apps
  / opens Command or local app search depending focus/context
  Ctrl/Alt+K, if available, opens global Command

Settings
  / opens Settings search by default
  global Command available through shortcut or command quick bar

Hub
  / opens Hub search by default
  global Command available through shortcut

System-wide
  configured Command shortcut opens global Command overlay
```

Recommended Titan 2 default:

```text
Fn + 3 = Command from Start base bar
/      = open search/command for current surface
Alt+Space or Ctrl+K, if available = global Command
```

If a text field is focused, printable keys and symbol modifiers must enter text,
not trigger Command.

## Result model

Command results are grouped but still navigable as one list.

```text
Apps
Settings
Contacts / People
Hub content
Actions
Files / documents, if provider-supported
Web/browser handoff, if configured
System diagnostics, where safe
```

Default result order should favor local, safe, high-confidence results:

```text
1. Exact app/action/settings match
2. Recently used app/action
3. Frequent communication/contact target
4. Local Hub result
5. Broader app/settings/content match
6. External provider handoff
```

Command must make action source clear. Example:

```text
Settings
  Open Settings
  source: Settings

Call Alex
  Phone call
  source: Dialer / Contacts

Reply to message
  provider-safe reply
  source: Messages notification / provider
```

## Search and command syntax

Command must support plain typing first. Prefixes are optional accelerators, not
required knowledge.

```text
plain text
  app, settings, contact, action and Hub search

@person
  people/contact-focused search

#tag
  optional future tag/filter search

?query
  help/discoverable command examples

/settings query
  settings-focused search

/apps query
  app-focused search

/hub query
  Hub-focused search
```

The prefix model must not break normal app names containing punctuation or
symbols.

## Keyboard interaction

```text
Type letters
  edit query when Command field is focused
  open Command from Start when type-to-command is active

Up / Down
  move result focus

Left / Right
  move between result groups or chips when group navigation is active

Enter
  open focused result or run safe primary action

Space
  preview / peek when supported and privacy allows

Tab / Shift+Tab
  move between query, group chips and results where available

Back / Esc
  close Command and restore previous focus

Menu / Fn+Enter
  show actions for focused result

Alt+Enter
  open result source/details where available

Backspace
  delete query text; close Command only when query is empty and not editing

Sym / Alt / Fn
  enter symbols and alternate characters in the query field
```

## Row actions

Every result row may expose actions, but only if the source can support them.

```text
App result
  Open
  App info
  Permissions
  Pin to Start
  Add to base bar
  Uninstall, if allowed

Settings result
  Open
  Open parent section
  Add shortcut, future optional

Contact result
  Call
  Message
  Mail, if account exists
  View contact

Hub result
  Open source
  Reply, only when provider supports it
  Mark read/unread, only when provider supports it
  Star/Add priority, Sable-owned metadata only where safe

Action result
  Run action
  Show action details
  Require confirmation for destructive or privacy-sensitive actions
```

Command must not fabricate capabilities. If provider support is absent, the
action must be hidden or disabled with a clear explanation.

## Privacy and lock state

Command can reveal sensitive information if implemented carelessly. It must
respect lock/private modes consistently with Hub and notifications.

```text
LOCKED_MODE=COUNTS_AND_GENERIC_LABELS_ONLY
PRIVATE_MODE=REDACT_CONTENT_PREVIEWS
CONTACT_NAMES=POLICY_DEPENDENT
MESSAGE_BODY_PREVIEW=NO_WHEN_LOCKED
INLINE_REPLY=NO_WHEN_LOCKED_UNLESS_SYSTEM_POLICY_ALLOWS
```

Examples:

```text
Unlocked
  Alex Chen — Are we still meeting today?

Locked/private
  Messages — 2 unread
```

## Mouse and pointer interaction

Pointer input complements keyboard focus. It must not create a separate command
state.

```text
Move pointer
  hover highlight only; do not steal keyboard focus

Click result
  focus and open result

Right click / long press
  open row actions menu

Scroll
  scroll result list

Click search field
  focus query text

Keyboard after pointer
  resumes keyboard focus from selected or last-focused row
```

## Visual and layout requirements

Titan 2 visual artifacts must use the Titan 2 hardware template, with square
screen above physical keyboard. Command is an overlay/surface that fits the
square display; it must not assume a slab phone screen.

```text
TITAN2_HARDWARE_TEMPLATE=REQUIRED
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=REQUIRED
NO_SLAB_PHONE_VISUAL_TARGET=REQUIRED
NO_STRETCHED_SCREEN_TARGET=REQUIRED
```

## Validation checklist

```text
SABLE_COMMAND_SEARCH_UX=PASS
GLOBAL_COMMAND_SEARCH=PASS
LOCAL_CONTEXT_COMMANDS=PASS
KEYBOARD_FIRST=PASS
TYPE_TO_COMMAND=PASS
RESULT_GROUPS=PASS
PROVIDER_SAFE_ACTIONS=PASS
NO_UNSUPPORTED_PROVIDER_ACTIONS=PASS
APP_ACTIONS_IN_COMMAND=PASS
SETTINGS_RESULTS_IN_COMMAND=PASS
HUB_RESULTS_IN_COMMAND=PASS
CONTACT_ACTIONS_IN_COMMAND=PASS
LOCKED_PRIVATE_MODE=PASS
ALT_SYM_FN_TEXT_ENTRY=PASS
MOUSE_POINTER_INTERACTION=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Non-goals

```text
NO_AI_REQUIRED_FOR_COMMAND
NO_PROVIDER_CREDENTIAL_OWNERSHIP
NO_PRIVATE_DATABASE_OWNERSHIP
NO_UNSUPPORTED_INLINE_REPLY
NO_DESTRUCTIVE_ONE_KEY_ACTIONS
NO_FLASH_ENABLEMENT
```
