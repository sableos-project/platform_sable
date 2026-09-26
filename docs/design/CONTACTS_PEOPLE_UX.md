# Contacts / People UX

Status: **design contract**  
Scope: **common Sable UX; Titan 2 visual target; no build/device/flash enablement**

## Purpose

Contacts / People is the common Sable surface for finding people, understanding
which source owns their contact data and starting provider-safe communication
actions. It supports Phone, Hub, Messages, Mail, Command/Search and notification
privacy without becoming a custom contacts provider or private identity store.

```text
CONTACTS_PEOPLE_UX=YES
ANDROID_CONTACTS_MENTAL_MODEL=KEEP
PEOPLE_MODEL_SHARED=YES
PROVIDER_OWNERSHIP_RETAINED=YES
CUSTOM_CONTACTS_PROVIDER=NO
CUSTOM_SYNC_PROVIDER=NO
```

## Primary jobs

Contacts / People must let the user:

- find a person quickly from a square keyboard-device display;
- use physical-keyboard alphabet shortcuts for fast list navigation;
- search by name, phone number, email, organization or alias where provider policy allows;
- call, message, email or open Hub history using provider-safe routes;
- inspect which app/account/source owns the displayed contact data;
- edit only through a supported provider or Android Contacts path;
- hide or redact sensitive people data when locked or private mode is active.

## Surface model

Contacts / People keeps the familiar Android Contacts structure while adapting it
for keyboard-first devices:

```text
Favorites
Frequent / Recent people
All contacts
Groups / labels when provider exposes them
Search
Person detail
Action sheet
Edit / source handoff
```

The default list is name-first. Raw contact IDs, provider row IDs and account
internals are not displayed in the default user view.

## Physical keyboard alphabet shortcuts

Alphabet shortcuts are required on Titan 2 and other keyboard-first profiles.

```text
ALPHABET_SHORTCUTS=YES
PHYSICAL_KEYBOARD_A_TO_Z_JUMP=YES
REPEATED_LETTER_CYCLES_MATCHES=YES
ALPHABET_INDEX_VISIBLE=YES
SEARCH_FIELD_EXPLICIT=YES
LETTER_KEYS_DO_NOT_OPEN_RANDOM_CONTACTS=YES
```

Rules:

- When focus is on the contact list and search is not active, pressing `A`-`Z`
  jumps focus to the first visible contact whose sort/display bucket begins with
  that letter.
- Repeating the same letter cycles through additional visible contacts in that
  bucket.
- The alphabet index is visible or discoverable on the right side of the list on
  compact square layouts.
- If no visible contact matches the pressed letter, focus does not move and a
  small `No contacts under X` status is shown.
- Letter shortcuts must never dial, message, email, delete, merge, edit or open a
  contact by themselves.
- `/`, Search key or Command opens full search. Once search is focused, physical
  letters enter query text instead of alphabet-jump commands.
- `Back`/`Esc` exits search and restores the previous list focus.
- `Alt`, `Sym`, `Fn` and long-press character entry are reserved for text-entry
  fields such as search, edit, account sign-in and provider forms.

## Keyboard navigation

```text
Up/Down              move list focus
Left/Right           move between list, alphabet index and detail/action panes
A-Z                  jump to visible alphabet bucket when search inactive
Repeated A-Z         cycle through contacts in bucket
Enter                open focused contact detail
Space                preview allowed safe actions where policy allows
/ or Search          focus search
Back/Esc             close search/detail/action or return one layer
Menu/Fn+Enter        open contact actions
Alt+Enter            open source/provider app or Android contact details
Home                 return to Sable Start/Home
Tab/Shift+Tab        move between list/search/actions/detail areas
```

Focus must be deterministic. Returning from Phone, Hub, Messages, Mail or source
app handoff restores the prior focused person when possible.

## Onscreen keyboard and touch behavior

Contacts / People must also work without relying only on the hardware keyboard.

```text
ONSCREEN_KEYBOARD_SEARCH=YES
TOUCH_TARGETS=YES
POINTER_INTERACTION=YES
PHYSICAL_AND_ONSCREEN_SEARCH_SHARE_STATE=YES
```

Touching the search field opens the onscreen keyboard when required. Physical
keyboard and onscreen keyboard search edit the same query state. Pointer hover
highlights rows without stealing focus; click selects/opens; long-press or
right-click opens actions.

## Person row model

Each row should fit the square display without becoming a dense address-book
spreadsheet.

```text
ROW_PRIMARY=DISPLAY_NAME
ROW_SECONDARY=SOURCE_OR_CONTEXT
ROW_BADGES=SAFE_ACTIONS_AND_PRIVACY
PACKAGE_NAMES_DEFAULT_VISIBLE=NO
RAW_PROVIDER_IDS_DEFAULT_VISIBLE=NO
```

Row content:

- avatar or initials;
- display name;
- source/app/account label where useful;
- safe action hints such as Call, Message, Mail or Hub;
- favorite/star state;
- privacy/source warning badge when policy limits details.

## Person detail model

Person detail is source-aware and action-first:

```text
PERSON_DETAIL=YES
SOURCE_AWARE_FIELDS=YES
CONTACT_ACTIONS_PROVIDER_SAFE=YES
UNSUPPORTED_ACTIONS_DISABLED=YES
```

Detail sections:

- primary identity: name, avatar/initials, favorite state;
- primary actions: Call, Message, Mail, Hub history;
- phone numbers and labels;
- email addresses and labels;
- linked source/provider labels;
- recent interactions when provider or Hub policy allows;
- edit/open-in-source affordance;
- privacy/source notes.

Actions must route through Android/provider-owned paths. Contacts / People does
not implement its own telephony, messaging, mail, sync or account stack.

## Provider/source boundaries

```text
PROVIDER_OWNERSHIP_RETAINED=YES
SOURCE_LABEL_VISIBLE=YES
EDIT_THROUGH_PROVIDER_OR_ANDROID_CONTACTS=YES
UNIFIED_PRIVATE_CONTACT_DB=NO
SILENT_CONTACT_MERGE=NO
```

Contacts may show a unified people view, but it must not silently merge or alter
provider-owned records. When multiple sources appear to represent the same
person, the UI may present a clear linked-source view and route edits through the
owning source.

## Privacy states

Contacts / People participates in the same privacy model used by Hub,
Notification Shade, Lockscreen, Command and Phone.

```text
LOCKED_MODE_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
CONTACT_DETAILS_HIDDEN_WHEN_LOCKED=YES
```

States:

| State | Behavior |
| --- | --- |
| Unlocked | Show allowed names, source labels, contact details and actions. |
| Locked | Show counts or generic labels only unless platform policy allows more. |
| Private mode | Hide sensitive contact details and previews by default. |
| Source restricted | Show source and allowed action only; route to source app for detail. |

## Handoff model

Contacts / People is the shared people surface for these integrations:

```text
PHONE_HANDOFF=YES
HUB_HANDOFF=YES
MESSAGES_HANDOFF=YES
MAIL_HANDOFF=YES
COMMAND_SEARCH_HANDOFF=YES
NOTIFICATION_SHADE_HANDOFF=YES
```

Required flows:

- Phone: open person detail from Recents or a call row.
- Hub: open People filter or a person thread context.
- Messages: start/open a provider-safe message path.
- Mail: start/open a provider-safe mail path.
- Command/Search: show people results and safe actions without leaking locked/private details.
- Notification Shade: route missed-call or message notification people context into Contacts/People or Hub as allowed.

## Search model

Search is explicit and source-aware.

```text
CONTACTS_SEARCH=YES
LOCAL_FIRST_SEARCH=YES
PROVIDER_ALLOWED_FIELDS_ONLY=YES
```

Search may include display names, phone numbers, email addresses, organization,
notes or aliases only when platform/provider policy allows. Search results should
show why a person matched without exposing hidden details in locked/private modes.

## Accessibility

```text
VISIBLE_FOCUS=YES
KEYBOARD_ONLY_ACCESSIBLE=YES
SCREEN_READER_LABELS=YES
LARGE_TOUCH_TARGETS=YES
NO_FOCUS_TRAPS=YES
```

Every person row, alphabet shortcut region, search field and action must have a
clear accessible name. The alphabet index must not become a focus trap.

## Non-goals

```text
CUSTOM_CONTACTS_PROVIDER=NO
CUSTOM_SYNC_PROVIDER=NO
CUSTOM_TELEPHONY_STACK=NO
CUSTOM_MESSAGING_STACK=NO
CUSTOM_MAIL_STACK=NO
SILENT_CONTACT_MERGE=NO
UNSUPPORTED_PROVIDER_ACTIONS=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

This contract defines UX and integration rules only. It does not enable a new
contacts provider, sync engine, telephony stack, mail stack, messaging stack or
Titan 2 flash/build path.
