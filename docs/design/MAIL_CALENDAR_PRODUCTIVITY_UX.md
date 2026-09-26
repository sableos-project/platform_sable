# Mail / Calendar Productivity UX

Status: design contract for Titan 2 keyboard-first productivity surfaces.

This document defines the common Sable productivity rule used by Mail, Calendar
and future dense information surfaces on Titan 2-class keyboard devices.

## Scope

Mail and Calendar are productivity surfaces, not launcher extensions. They must
feel consistent with Sable Start, Hub, Command/Search, Messages and Contacts, but
they keep provider/source ownership.

```text
MAIL_CALENDAR_PRODUCTIVITY_UX=YES
TITAN2_PRODUCTIVITY_RULES=YES
ANDROID_MAIL_CALENDAR_MENTAL_MODEL=KEEP
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
EXPLICIT_HISTORY_OR_PAGE_MODE=YES
RETURN_TO_CURRENT_OR_LATEST_KEY=YES
KEYBOARD_FIRST_PRODUCTIVITY=YES
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
```

## Product rule: current/latest content must be visible

Titan 2 has a square display and hardware keyboard, but no touchpad dependency.
Dense productivity screens must therefore open on the current or latest relevant
state, not on an arbitrary scroll position.

```text
LATEST_OR_CURRENT_CONTENT_PRIMARY=YES
MAIL_INBOX_LATEST_FIRST=YES
MAIL_OPEN_FOLDER_AT_NEWEST=YES
MAIL_OPEN_THREAD_AT_NEWEST_RELEVANT_MESSAGE=YES
CALENDAR_OPEN_AT_TODAY_CURRENT_TIME=YES
CALENDAR_NEXT_EVENT_VISIBLE=YES
CALENDAR_NOW_LINE_VISIBLE=YES
SCROLLING_REQUIRED_FOR_CURRENT_CONTENT=NO
```

Older mail, long quoted history, previous calendar days, future blocks and dense
agenda history remain accessible, but only through explicit commands such as
search, jump, page mode, agenda mode, date mode, or history mode.

```text
MAIL_HISTORY_ACCESS_EXPLICIT=YES
MAIL_LONG_BODY_PAGE_MODE=YES
MAIL_QUOTED_TEXT_COLLAPSED_BY_DEFAULT=YES
CALENDAR_PAST_EVENTS_EXPLICIT=YES
CALENDAR_FUTURE_BLOCK_JUMP=YES
CALENDAR_TIME_BLOCK_PAGE_ONLY=YES
CONTINUOUS_TOUCHPAD_SCROLLING_REQUIRED=NO
```

## Shared keyboard contract

Mail and Calendar share a mnemonic keymap. Single-key shortcuts must never fire
while the user is typing in a compose body, reply dock, search field, event title,
location, attendee field, note field, or Bottom Command Dock prompt.

```text
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
TEXT_INPUT_ALWAYS_WINS=YES
INPUT_GUARD_REQUIRED=YES
```

Default shared key behavior:

| Key | Mail action | Calendar action |
| --- | --- | --- |
| `C` | Compose mail | Create event |
| `T` | Top/current latest point | Today/current time |
| `J` / `N` | Next thread/message/action | Next event/day/block |
| `K` / `P` | Previous thread/message/action | Previous event/day/block |
| `R` | Reply | RSVP / respond |
| `D` / `Backspace` | Delete with confirmation | Delete with confirmation |
| `/` / `S` | Search/filter | Search/filter |
| `V` | View/filter mode | View mode |
| `Enter` | Open/focus detail | Open/edit detail |
| `Esc` / `Back` | Back/cancel | Back/cancel |
| `Space` | Explicit page mode only | Explicit time/page block only |
| `Shift+Space` | Explicit previous page only | Explicit previous time/page block only |

`Space` and `Shift+Space` are allowed only as explicit paging controls. They must
not be required to see the newest mail, the current thread state, today's current
time window, or the next event.

## Mail model

Mail uses a square-display split-pane layout on Titan 2: thread list on the left,
reader/detail pane on the right, and a Bottom Command Dock for quick reply,
search, compose and commands.

```text
MAIL_SPLIT_PANE=YES
MAIL_THREAD_LIST_LEFT=YES
MAIL_READER_RIGHT=YES
MAIL_BOTTOM_COMMAND_DOCK=YES
MAIL_QUICK_REPLY_DOCK=YES
MAIL_ATTACHMENTS_AND_ACTIONS_VISIBLE=YES
```

Default Mail state:

```text
MAIL_FOLDER_LATEST_FIRST=YES
MAIL_NEWEST_THREAD_FOCUSED_BY_DEFAULT=YES
MAIL_READER_SHOWS_LATEST_RELEVANT_MESSAGE=YES
MAIL_NEW_MESSAGE_AUTO_ADVANCE=YES
MAIL_RETURN_TO_LATEST_KEY=YES
MAIL_UNREAD_JUMP=YES
MAIL_SEARCH=YES
MAIL_PROVIDER_SAFE_ACTIONS=YES
```

Reader and reply behavior:

```text
MAIL_INLINE_REPLY_FROM_READER=YES
MAIL_CTRL_ENTER_SEND=YES
MAIL_ESC_CANCEL_REPLY=YES
MAIL_ALT_SYM_FN_TEXT_ENTRY=YES
MAIL_ONSCREEN_KEYBOARD_FALLBACK=YES
MAIL_LONG_BODY_REQUIRES_EXPLICIT_PAGE_MODE=YES
MAIL_FULL_THREAD_HISTORY_SEARCHABLE=YES
MAIL_REMOTE_CONTENT_SAFE_BY_DEFAULT=YES
MAIL_ATTACHMENT_ACTIONS_PROVIDER_SAFE=YES
```

Mail must not take ownership of provider accounts, sync engines, message stores,
protocol stacks, signing/encryption stacks or server-side labels beyond what the
selected source app/provider exposes.

```text
CUSTOM_MAIL_PROVIDER=NO
CUSTOM_MAIL_SYNC_PROVIDER=NO
CUSTOM_IMAP_SMTP_STACK=NO
CUSTOM_EAS_STACK=NO
PROVIDER_OWNERSHIP_RETAINED=YES
UNSUPPORTED_PROVIDER_ACTIONS=NO
```

## Calendar model

Calendar uses a 5-column current-work-window view and an agenda/detail view.
The 5-column view is the dense visual surface; the agenda/detail view is the
no-scroll-cost fallback for event-heavy days.

```text
CALENDAR_5_COLUMN_VIEW=YES
CALENDAR_AGENDA_SPLIT_VIEW=YES
CALENDAR_OPEN_AT_TODAY_CURRENT_TIME=YES
CALENDAR_NEXT_EVENT_VISIBLE=YES
CALENDAR_SELECTED_EVENT_DETAIL_VISIBLE=YES
CALENDAR_NOW_LINE_VISIBLE=YES
CALENDAR_RETURN_TO_NOW_KEY=YES
```

The 5-column view should expose today/current time, adjacent work days, the next
upcoming event, and a selected-slot inspector. Event cards may be compact, but
the selected event detail must show title, time, location, attendees or meeting
link when available.

```text
CALENDAR_SELECTED_SLOT_INSPECTOR=YES
CALENDAR_EVENT_DETAIL_CARD=YES
CALENDAR_LOCATION_AND_MEETING_LINK_VISIBLE=YES
CALENDAR_RSVP_PROVIDER_SAFE=YES
CALENDAR_NATURAL_LANGUAGE_QUICK_ENTRY=CANDIDATE
```

Dense time navigation is explicit:

```text
CALENDAR_PAGE_TIME_BLOCK_ONLY=YES
CALENDAR_JUMP_TO_TODAY=YES
CALENDAR_JUMP_TO_NEXT_EVENT=YES
CALENDAR_SEARCH=YES
CALENDAR_DATE_PICKER=YES
CALENDAR_PAST_EVENTS_EXPLICIT=YES
CALENDAR_FUTURE_BLOCK_JUMP=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_NEXT_EVENT=NO
```

Calendar must not take ownership of sync providers, remote calendars, event
stores, account credentials, conferencing providers or RSVP backends beyond what
Android/provider APIs expose.

```text
CUSTOM_CALENDAR_PROVIDER=NO
CUSTOM_CALENDAR_SYNC_PROVIDER=NO
CUSTOM_CONFERENCE_PROVIDER=NO
PROVIDER_OWNERSHIP_RETAINED=YES
UNSUPPORTED_PROVIDER_ACTIONS=NO
```

## Bottom Command Dock

The Bottom Command Dock is a Sable interaction model shared with Start, Command,
Messages and productivity apps. It may expose search, compose, quick reply and
quick event entry, but it must not capture normal text unexpectedly.

```text
BOTTOM_COMMAND_DOCK=YES
COMMAND_DOCK_TEXT_INPUT_GUARDED=YES
MAIL_REPLY_DOCK=YES
CALENDAR_QUICK_ENTRY_DOCK=CANDIDATE
COMMAND_DOCK_VISIBLE_SHORTCUTS=YES
COMMAND_DOCK_NO_FOCUS_TRAPS=YES
```

Natural-language entry for `mail` or `cal` is a candidate feature. This contract
allows it as a future implementation path, but does not require parser code in
this PR.

## Visual states required

The source-controlled visual artifact must show four Titan 2 states:

```text
VISUAL_STATE_1=MAIL_INBOX_SPLIT_PANE
VISUAL_STATE_2=MAIL_READER_QUICK_REPLY
VISUAL_STATE_3=CALENDAR_5_COLUMN_NOW_VIEW
VISUAL_STATE_4=CALENDAR_AGENDA_EVENT_DETAIL
```

Each state must use the Titan 2 template rule.

```text
TITAN2_HARDWARE_TEMPLATE=YES
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=YES
NO_SLAB_PHONE_VISUAL_TARGET=YES
NO_STRETCHED_SCREEN_TARGET=YES
```

## Privacy and locked/private behavior

Mail and Calendar must preserve the privacy model already defined for Hub,
Notifications, Command/Search, Lockscreen and Messages.

```text
LOCKED_MODE_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
NOTIFICATION_SHADE_HANDOFF=YES
HUB_HANDOFF=YES
COMMAND_SEARCH_HANDOFF=YES
CONTACTS_PEOPLE_HANDOFF=YES
```

Locked/private contexts may show counts, sender/source class, event count, or
calendar busy state only when allowed by platform and source policy.

## Implementation boundary

This is a design and product contract only. K-9 Mail / Thunderbird for Android,
Etar Calendar, AOSP Calendar providers, and other implementations may be used as
future references or integration candidates, but this PR does not import code,
patch application source, add protocol stacks, change provider ownership, enable
build targets, or enable flashing.

```text
SOURCE_PATCHES_REFERENCE_ONLY=YES
CODE_IMPORT=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
