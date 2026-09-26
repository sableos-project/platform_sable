# Messages / Conversation UX

Status: design contract for keyboard-first Sable Messages and conversation screens.

This document defines the Sable Messages / Conversation UX for Titan 2 and other
keyboard-first profiles. It complements Sable Hub, Contacts / People, Phone,
Command/Search, Notification Shade, Lockscreen and Setup Wizard contracts.

The primary design rule is simple:

```text
LATEST_MESSAGE_ALWAYS_VISIBLE=YES
SCROLLING_REQUIRED_TO_SEE_LATEST_MESSAGE=NO
```

A user must never need a touchpad, swipe, drag gesture or manual scroll to return
to the current conversation state. Conversation screens open at the newest
message, stay anchored to the newest message, and automatically advance when new
messages arrive or are sent.

## Scope

```text
MESSAGES_CONVERSATION_UX=YES
ANDROID_MESSAGES_MENTAL_MODEL=KEEP
KEYBOARD_FIRST_COMPOSITION=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
LATEST_ANCHORED_CONVERSATION=YES
LATEST_MESSAGE_ALWAYS_VISIBLE=YES
AUTO_ADVANCE_TO_LATEST_ON_INCOMING=YES
AUTO_ADVANCE_TO_LATEST_ON_SEND=YES
SCROLLING_REQUIRED_TO_SEE_LATEST_MESSAGE=NO
CONTINUOUS_TOUCHPAD_SCROLLING_REQUIRED=NO
HISTORY_ACCESS_EXPLICIT=YES
PROVIDER_OWNERSHIP_RETAINED=YES
```

Sable Messages must preserve familiar Android Messages expectations while making
Titan 2 physical keyboard use safe and primary.

## Non-goals

```text
CUSTOM_SMS_STACK=NO
CUSTOM_RCS_STACK=NO
CUSTOM_MMS_STACK=NO
CUSTOM_CARRIER_STACK=NO
UNSUPPORTED_PROVIDER_ACTIONS=NO
SILENT_MESSAGE_IMPORT=NO
```

Sable Messages is a user-facing messaging surface and handoff surface. It must
not pretend to provide transport, carrier, RCS, MMS, encryption, backup, import,
read receipt or attachment behavior that the underlying provider does not expose.

## Conversation layout

A conversation uses a latest-anchored layout:

```text
Top:      contact/source/privacy header
Middle:   latest message window
Bottom:   composer and action/status row
```

The latest message window is not a free-scroll transcript. It is a bounded
current-context window showing the newest message plus a small number of recent
messages for context. The newest received or sent message remains visible.

```text
LATEST_MESSAGE_POSITION=PINNED_IN_CURRENT_VIEW
RECENT_CONTEXT_WINDOW=YES
OLD_HISTORY_FREE_SCROLL=NO_BY_DEFAULT
COMPOSER_VISIBLE_WITH_LATEST=YES
```

On square Titan 2 displays, message bubbles should be compact, single-column and
high contrast. Long message text may wrap inside the visible message area, but it
must not push the newest message or composer off screen without a visible
focused affordance to continue.

## Automatic latest behavior

Opening a thread must land at latest:

```text
OPEN_THREAD_AT_LATEST=YES
RESTORE_THREAD_AT_LATEST=YES
RETURN_FROM_HISTORY_TO_LATEST=REQUIRED
```

Sending a message must keep the conversation at latest:

```text
SEND_MESSAGE_AUTO_ADVANCE=YES
SENT_MESSAGE_VISIBLE_IMMEDIATELY=YES
DELIVERY_STATUS_VISIBLE_WHEN_PROVIDER_EXPOSES=YES
```

Receiving a message while in the conversation must keep the conversation at
latest unless the user is in an explicit history/search mode:

```text
INCOMING_MESSAGE_AUTO_ADVANCE=YES
LATEST_UNLESS_HISTORY_MODE=YES
HISTORY_MODE_SHOWS_NEW_MESSAGE_INDICATOR=YES
```

If the user is deliberately viewing older history, the screen may avoid yanking
focus, but it must show a clear keyboard-focusable indicator such as `New message
— Enter to latest`.

## History access without touchpad-style scrolling

Older history is accessed explicitly. It is not a required continuous scroll path.

Allowed history patterns:

```text
SEARCH_IN_CONVERSATION=YES
JUMP_TO_DATE=YES
JUMP_TO_UNREAD=YES
JUMP_TO_ATTACHMENT=YES
PAGE_UP_HISTORY=OPTIONAL
PAGE_DOWN_TOWARD_LATEST=OPTIONAL
LATEST_KEY=REQUIRED
```

Recommended keyboard model:

```text
/                 Search in conversation
Ctrl+L or End      Return to latest
PageUp             Older page, if supported
PageDown           Newer page / latest direction, if supported
N                 Next unread/new item when in history/search mode
Back/Esc           Leave history/search mode and return to latest
```

History pages are discrete slices. They must keep visible focus and must always
provide a direct return-to-latest action. A conversation should not depend on
precision pointer scrolling, touchpad panning or finger scrolling as the only
way to read or leave history.

## Conversation list

The conversation list is also latest-oriented:

```text
LATEST_CONVERSATION_FIRST=YES
UNREAD_AND_PRIORITY_AWARE=YES
OPEN_SELECTED_THREAD_AT_LATEST=YES
NO_TOUCHPAD_SCROLL_DEPENDENCY=YES
```

Keyboard behavior:

```text
Up/Down            Move visible focus one row
Home               First/latest row
End                Last visible row or latest anchor depending context
Letters            Search/type-to-filter, not random open
/                  Focus search/filter
Enter              Open focused conversation at latest
Menu/Fn+Enter      Conversation actions
Back/Esc           Clear search/filter or leave Messages
```

Conversation list paging may exist for very large inboxes, but it should be
page-based, search-first and keyboard-addressable rather than touchpad-scroll
first.

## Composer and text entry

Physical keyboard text entry is primary on Titan 2. Onscreen keyboard fallback is
required.

```text
PHYSICAL_KEYBOARD_COMPOSITION=YES
ALT_SYM_FN_TEXT_ENTRY=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
PHYSICAL_AND_ONSCREEN_DRAFT_SHARE_STATE=YES
DRAFT_PERSISTENCE=YES
```

Recommended composer behavior:

```text
Typing             Enters message text when composer is focused
Enter              Send when single-line send mode is enabled
Shift+Enter        Insert newline
Alt+Enter          Provider-safe send/secondary send option when used
Backspace/Delete   Edit draft text
Esc                Collapse actions or leave composer focus safely
Menu/Fn+Enter      Open message actions/attachments if available
```

If a physical keyboard mapping fails for a critical character, the onscreen
keyboard must allow entry without leaving the conversation or losing the draft.

## Attachments and media

Attachments are provider-safe and permission-aware.

```text
ATTACHMENT_ACTIONS_PROVIDER_SAFE=YES
CAMERA_HANDOFF_PROVIDER_SAFE=YES
MEDIA_PICKER_PERMISSION_AWARE=YES
UNSUPPORTED_ATTACHMENT_ACTIONS=NO
```

On Titan 2, attachment actions should appear as a compact action sheet rather
than requiring horizontal carousels or drag gestures.

## Row and bubble actions

Message bubble actions must be keyboard reachable and provider-safe.

```text
MESSAGE_ACTIONS_KEYBOARD_ACCESSIBLE=YES
COPY_TEXT=YES
REPLY_IF_PROVIDER_SAFE=YES
FORWARD_IF_PROVIDER_SAFE=YES
DELETE_IF_PROVIDER_SAFE=YES
DETAILS_IF_PROVIDER_SAFE=YES
UNSUPPORTED_PROVIDER_ACTIONS=NO
```

Recommended message action model:

```text
Up/Down            Move focus through visible recent messages only when not composing
Enter              Open safe details/action if focused message supports it
Space              Safe preview/select if policy allows
Menu/Fn+Enter      Open focused message actions
Back/Esc           Return focus to composer/latest
```

Message actions must not make the user scroll away from latest as a side effect.

## Privacy and locked state

Messages must respect lockscreen, private mode and provider/source restrictions.

```text
LOCKED_MODE_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
MESSAGE_BODY_HIDDEN_WHEN_LOCKED=YES
CONTACT_NAME_POLICY_AWARE=YES
HUB_PRIVACY_COMPATIBLE=YES
NOTIFICATION_SHADE_PRIVACY_COMPATIBLE=YES
```

Locked/private views may show counts, source labels, unread status and safe
metadata. They must not show message bodies, sensitive contact details,
attachments or inline replies unless platform and provider policy allow it.

## Handoffs

Messages integrates with existing Sable surfaces without taking over their data.

```text
CONTACTS_HANDOFF=YES
PHONE_HANDOFF=YES
HUB_HANDOFF=YES
COMMAND_SEARCH_HANDOFF=YES
NOTIFICATION_SHADE_HANDOFF=YES
LOCKSCREEN_HANDOFF=YES
```

Handoff rules:

- Contacts / People owns person identity display and source labels.
- Phone owns call handoff.
- Hub may show conversation rows and provider-safe reply paths.
- Notification Shade may surface safe inline reply only when provider/platform
  exposes it.
- Command/Search can open conversations and provider-safe actions, but must not
  expose private bodies when policy forbids it.
- Lockscreen can show redacted conversation state only.

## Focus restoration

Focus restoration must prefer latest and composer safety.

```text
RETURN_FROM_ACTIONS_TO_COMPOSER=YES
RETURN_FROM_HISTORY_TO_LATEST=YES
RETURN_FROM_CONTACT_TO_THREAD_LATEST=YES
RETURN_FROM_ATTACHMENT_TO_COMPOSER=YES
NO_FOCUS_TRAPS=YES
NO_MESSAGE_DEAD_ENDS=YES
```

The user should always be one obvious keypress away from the current/latest
conversation state.

## Titan 2 visual requirements

Titan 2 visual artifacts must use the square display plus physical keyboard
hardware template.

```text
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
```

The visual target must demonstrate:

- latest-anchored conversation;
- newest message visible above composer;
- physical keyboard composition;
- onscreen keyboard fallback;
- explicit History/Search mode;
- `New message — Enter to latest` indicator while in history;
- privacy-redacted locked/private state;
- provider-safe actions.

## Validation

```text
MESSAGES_CONVERSATION_UX=PASS
ANDROID_MESSAGES_MENTAL_MODEL=PASS
KEYBOARD_FIRST_COMPOSITION=PASS
PHYSICAL_KEYBOARD_COMPOSITION=PASS
ONSCREEN_KEYBOARD_FALLBACK=PASS
ALT_SYM_FN_TEXT_ENTRY=PASS
LATEST_ANCHORED_CONVERSATION=PASS
LATEST_MESSAGE_ALWAYS_VISIBLE=PASS
OPEN_THREAD_AT_LATEST=PASS
RESTORE_THREAD_AT_LATEST=PASS
AUTO_ADVANCE_TO_LATEST_ON_INCOMING=PASS
AUTO_ADVANCE_TO_LATEST_ON_SEND=PASS
SCROLLING_REQUIRED_TO_SEE_LATEST_MESSAGE=NO
CONTINUOUS_TOUCHPAD_SCROLLING_REQUIRED=NO
HISTORY_ACCESS_EXPLICIT=PASS
SEARCH_IN_CONVERSATION=PASS
JUMP_TO_UNREAD=PASS
RETURN_TO_LATEST_KEY=PASS
CONVERSATION_LIST_LATEST_FIRST=PASS
MESSAGE_ACTIONS_KEYBOARD_ACCESSIBLE=PASS
CONTACTS_HANDOFF=PASS
PHONE_HANDOFF=PASS
HUB_HANDOFF=PASS
COMMAND_SEARCH_HANDOFF=PASS
NOTIFICATION_SHADE_HANDOFF=PASS
LOCKSCREEN_PRIVACY_COMPATIBLE=PASS
LOCKED_MODE_REDACTION=PASS
PRIVATE_MODE_REDACTION=PASS
SOURCE_RESTRICTED_MODE=PASS
NO_FOCUS_TRAPS=PASS
NO_MESSAGE_DEAD_ENDS=PASS
CUSTOM_SMS_STACK=NO
CUSTOM_RCS_STACK=NO
CUSTOM_MMS_STACK=NO
UNSUPPORTED_PROVIDER_ACTIONS=NO
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
