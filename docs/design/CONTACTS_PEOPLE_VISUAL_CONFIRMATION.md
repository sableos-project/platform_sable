# Contacts / People visual confirmation

Status: **visual and interaction checklist**  
Artifact: `docs/design/artifacts/sable-contacts-people-titan2.svg`

## Required visual target

```text
CONTACTS_PEOPLE_VISUAL_ARTIFACT_SOURCE_CONTROLLED=PASS
TITAN2_CONTACTS_VISUAL_TARGET=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
```

The visual artifact must show Contacts / People on a Titan 2-style square display
with a physical keyboard below the display. It must not use a generic slab-phone
mockup or a stretched rectangular display.

## Screens that must be visible

```text
CONTACT_LIST=PASS
ALPHABET_SHORTCUTS=PASS
PERSON_DETAIL=PASS
CONTACT_SEARCH=PASS
ACTIONS_SHEET=PASS
LOCKED_PRIVATE_REDACTION=PASS
SOURCE_PROVIDER_VIEW=PASS
```

The artifact should cover:

1. **Contacts list** — name-first rows, source/context labels and action hints.
2. **Alphabet shortcuts** — A-Z jump behavior from the physical keyboard, visible alphabet index and repeated-letter cycling.
3. **Person detail** — primary actions, source-aware fields and provider labels.
4. **Search** — explicit search field, onscreen keyboard fallback and physical-keyboard text entry.
5. **Actions sheet** — Call, Message, Mail, Hub, favorite, open in source and edit-through-provider.
6. **Locked/private state** — contact details redacted where required.
7. **Source/provider boundary** — source ownership visible; unsupported edits/actions disabled.

## Physical keyboard checks

```text
PHYSICAL_KEYBOARD_A_TO_Z_JUMP=PASS
REPEATED_LETTER_CYCLES_MATCHES=PASS
SEARCH_FIELD_EXPLICIT=PASS
LETTER_KEYS_DO_NOT_OPEN_RANDOM_CONTACTS=PASS
ALPHABET_INDEX_VISIBLE=PASS
NO_FOCUS_TRAPS=PASS
```

Validation expectations:

- Pressing `A`-`Z` from the list moves focus to the matching section or contact.
- Repeating the same letter cycles through visible matches.
- `/` or Search opens contact search.
- Once search is focused, letters enter query text rather than triggering alphabet jumps.
- Back/Esc exits search or detail and restores the previous list focus.
- Letter shortcuts never dial, message, email, delete, merge, edit or open a person by themselves.

## Onscreen keyboard and touch checks

```text
ONSCREEN_KEYBOARD_SEARCH=PASS
TOUCH_TARGETS=PASS
POINTER_INTERACTION=PASS
PHYSICAL_AND_ONSCREEN_SEARCH_SHARE_STATE=PASS
```

Search must work with the hardware keyboard and onscreen keyboard. Touch and
pointer paths must be available without creating a separate state from physical
keyboard entry.

## People/action checks

```text
DISPLAY_NAME_PRIMARY=PASS
SOURCE_LABEL_VISIBLE=PASS
PROVIDER_OWNERSHIP_RETAINED=PASS
CONTACT_ACTIONS_PROVIDER_SAFE=PASS
PHONE_HANDOFF=PASS
HUB_HANDOFF=PASS
MESSAGES_HANDOFF=PASS
MAIL_HANDOFF=PASS
COMMAND_SEARCH_HANDOFF=PASS
UNSUPPORTED_ACTIONS_DISABLED=PASS
```

Actions must route through Android/provider-owned paths. Contacts / People must
not invent unsupported call, message, mail, sync, merge or edit actions.

## Privacy checks

```text
LOCKED_MODE_REDACTION=PASS
PRIVATE_MODE_REDACTION=PASS
SOURCE_RESTRICTED_MODE=PASS
CONTACT_DETAILS_HIDDEN_WHEN_LOCKED=PASS
```

Locked and private modes must not leak hidden phone numbers, email addresses,
message context, mail context or contact notes through the list, search results,
action sheets or Command/Search results.

## Non-regression checks

```text
CUSTOM_CONTACTS_PROVIDER=NO
CUSTOM_SYNC_PROVIDER=NO
CUSTOM_TELEPHONY_STACK=NO
CUSTOM_MESSAGING_STACK=NO
CUSTOM_MAIL_STACK=NO
SILENT_CONTACT_MERGE=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
