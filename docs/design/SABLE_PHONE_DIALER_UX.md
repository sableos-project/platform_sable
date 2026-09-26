# Sable Phone / Dialer UX

Status: **design contract — Titan 2 keyboard-first profile**

This document defines the common Sable Phone / Dialer UX used by the keyboard-first Sable profile family, with Titan 2 as the active visual target. It is a UX contract only; it does not enable a build target, device image, telephony stack, IMS stack, emergency behavior change, or flash path.

```text
SABLE_PHONE_DIALER_UX=YES
ANDROID_PHONE_MENTAL_MODEL=KEEP
PHYSICAL_KEYBOARD_DIALING=YES
ONSCREEN_DIALPAD=YES
SOFTWARE_KEYBOARD_FALLBACK=YES
HUB_CALL_HANDOFF=YES
CONTACTS_HANDOFF=YES
EMERGENCY_CALL_PATH_PRESERVED=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Product role

Sable Phone is the daily calling surface for calls, recents, dialpad, contacts handoff, voicemail handoff where supported, missed-call handling and Hub integration.

Sable Phone must preserve Android user expectations:

- a numeric dialpad is always reachable;
- emergency calling remains reachable through platform policy;
- call log / recents are understandable without exposing private content unnecessarily;
- Contacts remains the contact data owner unless an explicit Sable contacts provider exists;
- telephony, SIM, carrier and IMS behavior remain platform/device-adapter responsibilities;
- the UI must work without assuming a fully functional physical keyboard.

## Primary surfaces

```text
PHONE_HOME
  tabs: Recents | Dialpad | Contacts | Voicemail/provider-supported

DIALPAD
  number entry, T9/contact matching, call action, delete, paste, SIM choice if available

RECENTS
  missed/incoming/outgoing calls, callback, details, block/report where platform-supported

CONTACT_HANDOFF
  contact search/open, call/SMS/email actions through provider-safe paths

IN_CALL
  mute, speaker, keypad, hold/add call where platform-supported, end call, route audio

MISSED_CALL_HANDOFF
  notification -> Phone recents and Hub calls filter
```

## Required mental model

```text
ANDROID_PHONE_MENTAL_MODEL=KEEP
CUSTOM_PHONE_STACK=NO_BY_DEFAULT
CUSTOM_EMERGENCY_BEHAVIOR=NO
CUSTOM_CARRIER_OR_IMS_POLICY=NO
```

Sable may improve layout, focus, keyboard access, privacy summaries and Hub handoff, but must not invent carrier, SIM, IMS, emergency or voicemail behaviors not exposed by the platform.

## Physical keyboard dialing

Titan 2 has a physical keyboard, so dialing must be efficient from hardware keys.

```text
PHYSICAL_KEYBOARD_DIALING=YES
TYPE_DIGITS_TO_DIAL=YES
TYPE_NAME_TO_SEARCH_CONTACTS=YES
STAR_POUND_PLUS_ENTRY=YES
BACKSPACE_DELETE_DIGIT=YES
ENTER_CALL_FOCUSED_ACTION=YES
ESC_BACK_CANCEL=YES
```

Required physical-key behavior:

| Key / input | Behavior |
| --- | --- |
| `0-9` | append digit in Dialpad; type-to-filter in Recents/Contacts when search focus is active |
| `*` | append `*` in Dialpad where the keyboard can emit it |
| `#` | append `#` in Dialpad where the keyboard can emit it |
| long-press / alternate `0` | append `+` when supported by input method/profile |
| `Backspace` | delete one digit or character |
| `Enter` | call if the call button is focused or the number field has a valid dial target; otherwise open focused row |
| `Space` | preview/details for focused recent/contact row where safe |
| `Up/Down` | move focus through recents, contacts or dialpad rows |
| `Left/Right` | move between tab/filter/action areas |
| `Back/Esc` | close details/search, clear transient entry, or return one layer |
| `/` or Search key | focus Phone search where available |
| `Menu` / `Fn+Enter` | open focused row actions |
| dedicated call key if present | call current valid dial target or callback focused recent row |
| dedicated end key if present | end active call or leave the call UI according to platform behavior |

Physical keyboard dialing must support both raw numeric dialing and contact search/T9-style contact narrowing where implementation supports it.

## Onscreen dialpad and software keyboard

The onscreen path is required, not optional. The user explicitly must not be blocked if physical keys fail, a modifier mapping is broken, accessibility mode changes input behavior, or the device is operated by touch.

```text
ONSCREEN_DIALPAD=YES
SOFTWARE_KEYBOARD_FALLBACK=YES
TOUCH_DIALPAD_TARGETS=YES
DIALPAD_VISIBLE_WITHOUT_HARDWARE_KEYS=YES
```

Required onscreen behavior:

- Dialpad tab shows a full numeric dialpad with `0-9`, `*`, `#`, delete and call.
- Long-press or alternate action provides `+` entry for international dialing.
- Onscreen dialpad remains usable when the physical keyboard is closed, unavailable, unmapped or failing.
- Contact search fields must allow the software keyboard to appear when text input is requested.
- Physical-key entry and onscreen entry must update the same number/search field, not two separate states.
- Touch targets must be large enough for Titan 2 square display operation.

## Dialpad layout guidance

Titan 2 square display space is limited. Dialpad should prioritize clarity over a dense phone-slab layout.

```text
DIALPAD_LAYOUT=COMPACT_SQUARE
NUMBER_FIELD_VISIBLE=YES
DELETE_VISIBLE=YES
CALL_ACTION_VISIBLE=YES
SIM_PICKER_VISIBLE_WHEN_APPLICABLE=YES
```

Recommended order:

```text
Top:     status / Phone title / active SIM if relevant
Middle:  number field + contact match preview
Grid:    1 2 3
         4 5 6
         7 8 9
         * 0 #
Bottom:  delete | call | more/SIM where applicable
```

## Recents and missed calls

```text
CALL_LOG_PRIVACY_AWARE=YES
MISSED_CALLS_VISIBLE=YES
CALLBACK_KEYBOARD_ACCESSIBLE=YES
HUB_CALL_HANDOFF=YES
```

Recents rows should show:

- contact name or number according to contact/provider policy;
- call direction and missed status;
- time/date;
- SIM indicator where dual-SIM state is exposed;
- callback action;
- details action;
- block/report/spam action only when platform-supported.

Locked/private modes must not expose more call-log detail than platform policy allows.

## Contacts handoff

```text
CONTACTS_HANDOFF=YES
CONTACT_PROVIDER_OWNERSHIP_RETAINED=YES
CONTACT_ACTIONS_PROVIDER_SAFE=YES
```

Sable Phone may search and route to Contacts. It must not silently own contacts storage unless the platform/product explicitly defines a Sable contacts provider.

Allowed actions:

- call selected number;
- open contact details;
- message selected number through provider-safe route;
- add unknown number to contacts through provider-owned flow;
- edit contact through Contacts provider UI;
- block/report where platform-supported.

## In-call surface

```text
IN_CALL_UI=YES
IN_CALL_KEYBOARD_NAVIGATION=YES
ONSCREEN_IN_CALL_CONTROLS=YES
AUDIO_ROUTE_PROVIDER_SAFE=YES
```

In-call controls must be available by focus and touch:

- mute;
- speaker/audio route;
- keypad/DTMF;
- hold/add call where platform-supported;
- end call;
- contact/call details where safe;
- return to active call from shade/Hub/Phone.

Physical keyboard in call:

| Key / input | Behavior |
| --- | --- |
| digits | DTMF when keypad mode is active; otherwise no accidental DTMF |
| Enter | activate focused in-call control |
| Back/Esc | leave transient panel or return to active call surface |
| dedicated end key if present | end call according to platform policy |
| arrows | move focus between controls |

## Emergency and emergency-adjacent behavior

```text
EMERGENCY_CALL_PATH_PRESERVED=YES
CUSTOM_EMERGENCY_ROUTING=NO
NO_EMERGENCY_DEAD_ENDS=YES
```

Sable Phone must not override Android/platform emergency call routing. The UX must ensure:

- emergency number entry works using both physical and onscreen input;
- emergency call affordance remains reachable from lockscreen per platform policy;
- emergency action cannot be hidden behind search, Hub, or custom Sable navigation;
- failed keyboard mappings cannot prevent onscreen emergency number entry;
- no Sable-only confirmation step blocks platform emergency behavior.

## Hub integration

```text
HUB_CALL_FILTER=YES
MISSED_CALL_HANDOFF_TO_HUB=YES
CALLBACK_FROM_HUB=PROVIDER_SAFE
PHONE_REMAINS_CALL_OWNER=YES
```

Hub may show missed calls and route callback/details actions. Phone/Dialer remains the call-owner surface.

Hub actions must follow provider/platform support:

- callback;
- open call details;
- open contact;
- clear/mute missed-call notification where allowed;
- no invented call-log mutation if unsupported.

## Notification shade integration

Phone notifications must follow the Quick Settings / Notification Shade contract:

```text
NOTIFICATION_SHADE_CALL_HANDOFF=YES
MISSED_CALL_NOTIFICATION=YES
ACTIVE_CALL_NOTIFICATION=YES
LOCKED_MODE_REDACTION=YES
```

Active call controls exposed from notifications must match platform-supported actions and must not expose private contact data beyond lockscreen policy.

## Privacy and security

```text
CALL_LOG_PRIVACY_AWARE=YES
LOCKED_MODE_COUNTS_ONLY_WHEN_REQUIRED=YES
PRIVATE_MODE_REDACTION=YES
NO_CONTACT_LEAKAGE_WHEN_LOCKED=YES
```

Sensitive states:

- locked device;
- private mode enabled;
- work profile / managed contacts;
- unknown caller / spam label;
- emergency call path;
- dual-SIM identity;
- contact photo/name policy.

Sable Phone must respect Android/profile policies before showing contact names, call log details, phone numbers, voicemail metadata or source account labels.

## Search model

```text
PHONE_SEARCH=YES
COMMAND_SEARCH_INTEGRATION=YES
LOCAL_FIRST_RESULTS=YES
NO_WEB_SEARCH_FROM_DIALER=YES
```

Phone search covers local/provider-allowed data only:

- recents;
- contacts;
- typed phone number;
- voicemail metadata if supported;
- settings aliases such as call settings / blocked numbers where platform-exposed.

Command/Search may route to Phone results, but Phone should not become a web search surface.

## Accessibility

```text
ACCESSIBILITY_ENTRY_VISIBLE=YES
FOCUS_ORDER_DETERMINISTIC=YES
SCREEN_READER_LABELS_REQUIRED=YES
NO_FOCUS_TRAPS=YES
```

Every Phone action must be reachable using:

- physical keyboard;
- onscreen touch controls;
- software keyboard where text entry is needed;
- screen reader / accessibility focus;
- Android Back navigation.

## Non-goals

```text
CUSTOM_TELEPHONY_STACK=NO
CUSTOM_IMS_STACK=NO
CUSTOM_EMERGENCY_STACK=NO
CUSTOM_CONTACTS_PROVIDER=NO_BY_DEFAULT
CALL_RECORDING_DECISION=OUT_OF_SCOPE
SPAM_CLASSIFIER_DECISION=OUT_OF_SCOPE
```

Those items require separate legal, privacy, carrier, platform and implementation decisions.

## Acceptance gates

```text
SABLE_PHONE_DIALER_UX=PASS
PHONE_VISUAL_ARTIFACT_SOURCE_CONTROLLED=PASS
TITAN2_PHONE_VISUAL_TARGET=PASS
ANDROID_PHONE_MENTAL_MODEL=PASS
PHYSICAL_KEYBOARD_DIALING=PASS
ONSCREEN_DIALPAD=PASS
SOFTWARE_KEYBOARD_FALLBACK=PASS
STAR_POUND_PLUS_ENTRY=PASS
DTMF_IN_CALL_KEYPAD=PASS
RECENTS_PRIVACY=PASS
CONTACTS_HANDOFF=PASS
HUB_CALL_HANDOFF=PASS
EMERGENCY_CALL_PATH_PRESERVED=PASS
NO_FOCUS_TRAPS=PASS
NO_CALL_DEAD_ENDS=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
