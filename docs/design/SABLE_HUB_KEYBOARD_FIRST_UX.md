# Sable Hub keyboard-first UX

Status: **design contract — Sable Hub / communications**

This document defines the keyboard-first Sable Hub UX for Titan 2, Titan 2 Elite,
Q27 and future keyboard-first devices. It follows the Sable Start visual-template
rule: visual design artifacts for Titan 2 must use the Titan 2 hardware shape,
with a square display above the physical keyboard, not slab-phone proportions.

Sable Hub is the communications and attention surface. It aggregates and routes
communication, but provider/source applications remain the owner of accounts,
credentials, private stores and protocol stacks.

```text
SABLE_HUB_SURFACE=YES
COMMUNICATIONS_FIRST=YES
PROVIDER_OWNERSHIP_RETAINED=YES
KEYBOARD_FIRST=YES
TOUCH_SECONDARY=YES
VISIBLE_FOCUS_REQUIRED=YES
TITAN2_HARDWARE_TEMPLATE=REQUIRED_FOR_VISUALS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Design goals

Sable Hub should be fast, readable and safe on a small square screen.

Goals:

```text
one-action access to priority communications
clear filters: Priority, Messages, Calls, Mail, Notifications, People
keyboard navigation without focus traps
privacy-safe lockscreen and private-mode behavior
provider-safe reply/open flows
search/filter within Hub
missed-call and quick-call affordances
message preview only when allowed by user privacy policy
clear app/source ownership for every row
Titan 2 square display visual target
```

Non-goals:

```text
no provider database ownership inside Hub
no silent credential ownership
no raw notification scraping as product truth
no hidden keyboard-only actions without visible hints
no launcher base quick bar inside Hub content
no generic notification shade clone
```

## Primary surfaces

Hub has five core surfaces. They share the same list, focus and action model.

```text
Priority
  user-relevant communication requiring attention

Messages
  SMS/MMS/RCS/provider-backed messages where supported

Calls
  missed calls, recent calls and contact actions

Mail
  email summaries from provider-owned mail sources

Notifications
  notification summaries and provider-safe actions

People
  contact/person-centric view across supported communication sources
```

The default Titan 2 entry should be **Priority** unless the user chooses a
specific default filter.

```text
DEFAULT_FILTER=Priority
USER_DEFAULT_FILTER=ALLOWED
FILTER_BAR_VISIBLE=YES
```

## Layout model

Titan 2 uses a square display, so Hub should prefer a compact vertical list with
visible filter chips rather than a tall phone timeline.

Required structure:

```text
status/title row
Hub title + privacy state where relevant
filter chips
priority summary row where space allows
communication list
optional action/detail pane only on larger profiles
shortcut hint footer where space allows
```

Hub must not use Sable Start's base quick bar as an internal dock. Access back to
Start is via system Home/back behavior, not a duplicated launcher row.

```text
START_BASE_QUICK_BAR_IN_HUB=NO
SYSTEM_BACK_HOME_NAVIGATION=YES
OPTIONAL_SHORTCUT_HINT_FOOTER=YES
```

## Row model

Each row must expose source ownership and privacy state.

```text
row icon
source/app label
sender/contact or channel
short preview if privacy allows
time / unread count / missed status
action chevron or context affordance
```

Examples:

```text
Messages
  Alex · 2 unread · provider: Messages

Mail
  Inbox · 1 unread · provider: Sable Mail or configured mail app

Missed call
  +1 contact/number · provider: Phone

Notifications
  3 new · provider/source app preserved
```

## Privacy model

Hub must degrade safely when locked, private mode is enabled, or a source app does
not allow previews.

```text
UNLOCKED_ALLOWED
  show sender, source and preview where user/source policy allows

LOCKED_OR_PRIVATE
  show count/source category only by default
  hide message text and sensitive content
  require unlock for reply/open sensitive content

SOURCE_RESTRICTED
  show app/source and count only
  route to source app for details
```

Hub should never infer or display protected content beyond what user policy and
source permissions allow.

## Keyboard interactions

Baseline keys:

```text
Up / Down
  move row focus

Left / Right
  move filter focus when filter bar is active
  otherwise collapse/expand row details where supported

Enter
  open selected row or source-owned conversation/detail

Space
  safe peek / expand row preview when privacy allows

Back / Esc
  close detail/search/context menu or return one layer

Home
  return to Sable Start / Home

Slash or Search key
  focus Hub search/filter field

Tab / Shift+Tab
  move between filter bar, list and action areas where present

Menu / Fn+Enter
  open row actions

Alt+Enter
  open source app / app info for the focused provider row where applicable
```

No key path may trap the user in a filter, search result, row detail, reply box or
context menu.

## Type input and search

Hub search is not a general web search. It filters local/provider-allowed Hub
content and actions.

```text
Typing when search field is not active
  type-ahead filters visible Hub rows or opens Hub search according to user mode

/ or Search key
  open full Hub search

Typing when search field is active
  edit search text

Backspace
  delete query text

Long Backspace
  clear query

Alt / Sym / Fn
  required for symbols and alternate characters in search/reply fields
```

Search aliases:

```text
messages, sms, unread, missed call, calls, voicemail
mail, inbox, people, contacts, priority, notifications
from:<person>, app:<source>, unread, today, missed
```

## Row actions

Row actions depend on source/provider support, user policy and privacy state.

Common actions:

```text
Open
Reply where provider exposes a safe reply action
Call back for missed calls
Mark read where supported
Mute / quiet source where supported
Open in source app
Show contact/person
Notification settings for source
Privacy options
```

Hub must not synthesize unsupported provider actions.

```text
PROVIDER_ACTION_REQUIRED=YES
INVENT_UNSUPPORTED_ACTIONS=NO
```

## Reply model

Reply is allowed only when the source provides a safe reply path.

```text
Inline reply
  allowed only for provider-supported safe reply
  Alt/Sym/Fn text entry required
  software keyboard fallback required where physical text entry fails

Unsupported reply
  route to source app / conversation
```

Critical text-entry gates still apply to reply and search fields.

## Mouse / pointer interactions

Pointer mode must complement keyboard navigation.

```text
Move pointer
  hover highlight only; do not steal keyboard focus

Click row
  focus and open/select row

Right click / long press
  open row actions where available

Scroll wheel
  scroll Hub list

Keyboard after pointer use
  resumes focus from clicked/selected row
```

## Visual language

Hub should feel like Sable Start's communication-right-page expanded into a full
surface, not like an unrelated app.

```text
Titan 2 hardware template
square display + physical keyboard
Sable dark visual language
small but readable filter chips
compact rows
strong blue focus ring
privacy indicators where needed
no slab-phone mockup
no stretched screen target
```

## Acceptance

```text
SABLE_HUB_KEYBOARD_FIRST_UX=PASS
COMMUNICATIONS_FIRST=PASS
PROVIDER_OWNERSHIP_RETAINED=PASS
PRIORITY_FILTER_DEFAULT=PASS
FILTER_BAR_VISIBLE=PASS
KEYBOARD_NAVIGATION=PASS
HUB_SEARCH=PASS
ROW_ACTIONS_PROVIDER_SAFE=PASS
INLINE_REPLY_PROVIDER_SAFE=PASS
PRIVACY_LOCKED_MODE=PASS
MOUSE_POINTER_INTERACTION=PASS
START_BASE_QUICK_BAR_IN_HUB=NO
TITAN2_HARDWARE_TEMPLATE=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
NO_DEVICE_CODE_CHANGED=PASS
NO_BUILD_CODE_CHANGED=PASS
FLASH_ENABLEMENT=NO
```
