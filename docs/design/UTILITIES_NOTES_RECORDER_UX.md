# Utilities / Notes / Recorder UX

## Status

```text
STATUS=DESIGN_CONTRACT
TARGET_DEVICE=Titan 2
SOURCE_PR=Utilities / Notes / Recorder UX
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

This document defines the merged daily-utility surface for SableOS on Titan 2.
It combines several small everyday apps into one design contract so the design
series does not fragment into separate PRs for every utility.

The merged surface covers:

```text
SURFACE_1=UTILITIES_HOME
SURFACE_2=NOTES_EDITOR_AND_LIST
SURFACE_3=VOICE_RECORDER_AND_TRANSCRIPT
SURFACE_4=CLOCK_ALARMS_TIMER_STOPWATCH
SURFACE_5=CALCULATOR_CONVERTER
SURFACE_6=TASKS_CHECKLISTS
SURFACE_7=OPTIONAL_WEATHER_PROVIDER_CARD
```

The goal is not to create a novelty utilities suite. The goal is to make the
small daily actions on a physical-keyboard square device fast, private,
keyboard-first and visually consistent with Start, Hub, Files, Media, Camera,
Mail, Calendar and Browser.

## Product decision

Utilities, Notes and Recorder should be treated as one daily-utility layer.

```text
UTILITIES_NOTES_RECORDER_UX=YES
MERGED_DAILY_UTILITY_SURFACE=YES
NOTES_INCLUDED=YES
RECORDER_INCLUDED=YES
CLOCK_INCLUDED=YES
CALCULATOR_CONVERTER_INCLUDED=YES
TASKS_CHECKLISTS_INCLUDED=YES
OPTIONAL_WEATHER_PROVIDER_CARD_INCLUDED=YES
SEPARATE_PR_PER_SMALL_UTILITY=NO
```

Rationale:

- Notes, Recorder, Tasks and Clock are quick-capture tools.
- Calculator and Converter are command-style utilities.
- Files owns long-term storage, export and attachment handoff.
- Hub owns notification resurfacing.
- Command/Search owns global discovery.
- Settings owns provider/account/privacy configuration.

This PR therefore defines a merged UX contract rather than six separate small
app designs.

## Global Titan 2 productivity rules

Utilities must follow the same Titan 2 rule established across Messages, Mail,
Calendar, Browser, Media, Camera and Files.

```text
TITAN2_PRODUCTIVITY_RULES_APPLY=YES
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
EXPLICIT_HISTORY_OR_PAGE_MODE=YES
RETURN_TO_CURRENT_OR_LATEST_KEY=YES
KEYBOARD_FIRST_UTILITIES=YES
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
TEXT_INPUT_ALWAYS_WINS=YES
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
```

The current note, active recording, current timer, active calculation or current
task list must be visible without requiring touchpad-style continuous scrolling.
Historical notes, older recordings, completed tasks and prior calculations may
use explicit history/page modes.

## Utilities Home

Utilities Home is a compact command dashboard, not a separate launcher.

Required content:

```text
UTILITIES_HOME=YES
CURRENT_ACTIVE_UTILITY_VISIBLE=YES
RECENT_NOTES_VISIBLE=YES
ACTIVE_RECORDING_VISIBLE=YES
NEXT_ALARM_OR_TIMER_VISIBLE=YES
RECENT_CALCULATION_VISIBLE=YES
OPEN_TASKS_VISIBLE=YES
OPTIONAL_WEATHER_PROVIDER_CARD_VISIBLE_WHEN_ENABLED=YES
```

Layout rules:

- top status strip remains consistent with other Sable surfaces;
- main canvas uses large Metro-influenced utility tiles and focused detail;
- active/urgent utility states sort above inactive history;
- bottom command dock exposes create, search and mode switches;
- keyboard focus is visible at all times.

Default utilities home keymap:

```text
N=New note
R=Recorder
C=Calculator
T=Tasks or Timer depending focused region
A=Alarm
S or /=Search
Enter=Open focused item
Space=Start/stop focused timer or recorder only when safe
Back=Return to previous surface
```

Single-key shortcuts must be disabled while typing in a note, task title,
calculator expression, search field or command dock prompt.

## Notes

Notes are quick local-first writing surfaces.

```text
NOTES_UX=YES
LOCAL_FIRST_NOTES=YES
CURRENT_NOTE_ALWAYS_VISIBLE=YES
RECENT_NOTES_LATEST_FIRST=YES
NOTE_LIST_AND_EDITOR_SPLIT=YES
FULL_SCREEN_EDITOR_MODE=YES
MARKDOWN_LIGHTWEIGHT_OPTIONAL=YES
CHECKLIST_LINES_SUPPORTED=YES
ATTACHMENT_HANDOFF_TO_FILES=YES
SHARE_EXPORT_EXPLICIT=YES
SILENT_REMOTE_SYNC_ENABLEMENT=NO
```

Notes should prioritize writing speed over rich-document complexity. The base
experience is plain text plus lightweight structure. A note may contain simple
headings, checklists and links, but complex document editing belongs to external
editors or future office/document work.

Required Notes states:

```text
NOTES_STATE_1=RECENT_NOTES_WITH_EDITOR_PREVIEW
NOTES_STATE_2=FOCUSED_EDITOR
NOTES_STATE_3=SEARCH_RESULTS_WITH_HIT_CONTEXT
NOTES_STATE_4=EXPORT_SHARE_CONFIRMATION
```

Notes keyboard rules:

```text
Ctrl+N=New note
Ctrl+S=Save checkpoint when applicable
Ctrl+F=Find in note
Ctrl+Enter=Complete current checklist item when applicable
Esc=Leave editor focus or close overlay
```

Text input owns all printable key events while the editor has focus.

Privacy rules:

```text
LOCKED_NOTE_REDACTION=YES
PRIVATE_MODE_NOTE_TITLES_REDACTED_WHEN_REQUIRED=YES
NOTES_SEARCH_RESPECTS_PRIVATE_MODE=YES
NOTES_PREVIEW_REDACTION_ON_LOCKSCREEN=YES
```

## Voice Recorder

Recorder is an explicit, privacy-forward capture tool.

```text
VOICE_RECORDER_UX=YES
EXPLICIT_RECORDING_START=YES
EXPLICIT_RECORDING_STOP=YES
ACTIVE_RECORDING_ALWAYS_VISIBLE=YES
RECORDING_DURATION_ALWAYS_VISIBLE=YES
AUDIO_PERMISSION_PROMPT_KEYBOARD_ACCESSIBLE=YES
FOREGROUND_RECORDING_INDICATOR_REQUIRED=YES
BACKGROUND_RECORDING_STATUS_REQUIRED=YES
SILENT_RECORDING_START=NO
HIDDEN_BACKGROUND_RECORDING=NO
```

Recorder should support quick capture, review, rename, trim handoff and export.
Automatic transcription may be a future provider-backed feature, but it must not
be silently enabled.

```text
TRANSCRIPTION_PROVIDER_OPTIONAL=YES
LOCAL_TRANSCRIPTION_PREFERRED_WHEN_AVAILABLE=YES
REMOTE_TRANSCRIPTION_SILENT_UPLOAD=NO
TRANSCRIPT_CONFIDENCE_OR_PROVIDER_VISIBLE=YES
TRANSCRIPT_EXPORT_EXPLICIT=YES
```

Required Recorder states:

```text
RECORDER_STATE_1=READY_TO_RECORD
RECORDER_STATE_2=ACTIVE_RECORDING
RECORDER_STATE_3=PLAYBACK_REVIEW
RECORDER_STATE_4=TRANSCRIPT_OR_MARKERS_REVIEW
```

Recorder keymap:

```text
R=Record when recorder surface is focused and no text input is active
Space=Pause/resume or play/pause depending state
Enter=Start/stop confirmation where required
M=Add marker
S=Save
E=Export
D=Delete with confirmation
```

Deletion must require confirmation when a recording has content.

## Clock, alarms, timer and stopwatch

Clock utilities are merged into the daily utilities layer.

```text
CLOCK_UX=YES
ALARMS_INCLUDED=YES
TIMER_INCLUDED=YES
STOPWATCH_INCLUDED=YES
WORLD_CLOCK_OPTIONAL=YES
NEXT_ALARM_ALWAYS_VISIBLE=YES
ACTIVE_TIMER_ALWAYS_VISIBLE=YES
ACTIVE_STOPWATCH_ALWAYS_VISIBLE=YES
CLOCK_HISTORY_OR_LAPS_PAGE_MODE=YES
```

Rules:

- active timer or stopwatch remains visible above history;
- alarm enable/disable is keyboard reachable;
- destructive alarm deletion requires confirmation;
- timers can be named;
- alarms and timers surface in Hub and lockscreen only through provider-safe
  system notification paths.

Clock keymap:

```text
A=New alarm
T=New timer
S=Stopwatch
Space=Start/pause active timer or stopwatch
R=Reset with confirmation where needed
L=Lap in stopwatch mode
D=Delete focused alarm/timer with confirmation
```

## Calculator and Converter

Calculator and Converter should be one command-style utility, not separate apps.

```text
CALCULATOR_UX=YES
CONVERTER_MERGED_IN_CALCULATOR=YES
STANDARD_CALCULATOR=YES
SCIENTIFIC_CALCULATOR=YES
UNIT_CONVERSION=YES
CURRENCY_CONVERSION_PROVIDER_OPTIONAL=YES
CURRENT_EXPRESSION_ALWAYS_VISIBLE=YES
RESULT_ALWAYS_VISIBLE=YES
HISTORY_EXPLICIT=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_RESULT=NO
```

Calculator must support keyboard entry first. The onscreen keypad is fallback
and touch convenience, not the primary Titan 2 input method.

Conversion scope:

```text
LENGTH_CONVERSION=YES
MASS_CONVERSION=YES
TEMPERATURE_CONVERSION=YES
AREA_VOLUME_CONVERSION=YES
TIME_CONVERSION=YES
DATA_SIZE_CONVERSION=YES
CURRENCY_CONVERSION_OPTIONAL_PROVIDER_BACKED=YES
SILENT_NETWORK_RATE_FETCH=NO
RATE_TIMESTAMP_VISIBLE=YES
```

Calculator key rules:

- digits and operators go to the expression field;
- Enter evaluates;
- Backspace edits;
- Esc clears current transient overlay;
- history requires explicit mode.

## Tasks and checklists

Tasks are intentionally simple. They are not a full project-management system.

```text
TASKS_CHECKLISTS_UX=YES
SIMPLE_TASKS_INCLUDED=YES
CHECKLISTS_INCLUDED=YES
CURRENT_OPEN_TASKS_VISIBLE=YES
COMPLETED_TASKS_EXPLICIT_HISTORY=YES
CALENDAR_HANDOFF_OPTIONAL=YES
NOTES_HANDOFF_SUPPORTED=YES
```

Tasks should be suitable for:

- short to-do lists;
- grocery/checklist style capture;
- note-linked tasks;
- quick reminders handed off to Calendar/Clock where appropriate.

Tasks must not silently create remote tasks or calendar items.

```text
SILENT_REMOTE_TASK_SYNC=NO
CALENDAR_EVENT_CREATION_USER_CONFIRMED=YES
REMINDER_CREATION_USER_CONFIRMED=YES
```

## Optional Weather/provider card

Weather is optional because it depends on provider/network policy.

```text
WEATHER_CARD_OPTIONAL=YES
WEATHER_PROVIDER_REQUIRED=YES
SILENT_LOCATION_ACCESS=NO
PRECISE_LOCATION_REQUIRED_BY_DEFAULT=NO
CITY_OR_MANUAL_LOCATION_SUPPORTED=YES
NETWORK_FETCH_USER_VISIBLE=YES
WEATHER_TIMESTAMP_VISIBLE=YES
```

If enabled, Weather appears as a utility card. It must not become an implicit
location tracker or hidden background network surface.

## Storage and handoff

Files / Documents owns long-term file organization. Utilities use explicit
handoffs.

```text
FILES_HANDOFF_REQUIRED=YES
NOTE_EXPORT_TO_FILES=YES
RECORDING_EXPORT_TO_FILES=YES
CALCULATION_COPY_OR_NOTE_HANDOFF=YES
TASK_EXPORT_TO_NOTE_OR_FILES=YES
SHARE_SHEET_USER_INITIATED=YES
SILENT_UPLOAD_OR_EXPORT=NO
```

Recorder output should appear in Files/Media only after explicit save/export or
provider-safe media insertion. Notes should store local data in an app-owned or
provider-owned location and expose export/import through Files/Documents.

## Security and privacy posture

```text
LOCAL_FIRST_DEFAULT=YES
REMOTE_SYNC_DEFAULT_OFF=YES
PROVIDER_BOUNDARY_EXPLICIT=YES
MICROPHONE_PERMISSION_EXPLICIT=YES
LOCATION_PERMISSION_EXPLICIT=YES
NOTIFICATION_PERMISSION_EXPLICIT=YES
LOCKSCREEN_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
APP_PRIVATE_DATA_NOT_BROWSED_BY_DEFAULT=YES
```

Recorder has the strictest privacy posture: no hidden background recording, no
silent transcription upload, no silent share/export and no lockscreen content
leakage.

## Visual target

The source-controlled visual artifact is:

```text
docs/design/artifacts/sable-utilities-notes-recorder-titan2.svg
```

It represents four Titan 2 states:

```text
VISUAL_STATE_1=UTILITIES_HOME
VISUAL_STATE_2=NOTES_EDITOR
VISUAL_STATE_3=RECORDER_ACTIVE_REVIEW
VISUAL_STATE_4=CLOCK_CALCULATOR_TASKS
```

## Acceptance checklist

```text
UTILITIES_NOTES_RECORDER_UX=PASS
MERGED_DAILY_UTILITY_SURFACE=PASS
NOTES_INCLUDED=PASS
RECORDER_INCLUDED=PASS
CLOCK_INCLUDED=PASS
CALCULATOR_CONVERTER_INCLUDED=PASS
TASKS_CHECKLISTS_INCLUDED=PASS
OPTIONAL_WEATHER_PROVIDER_CARD_INCLUDED=PASS
TITAN2_PRODUCTIVITY_RULES_APPLY=PASS
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=PASS
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
KEYBOARD_FIRST_UTILITIES=PASS
TEXT_INPUT_ALWAYS_WINS=PASS
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=PASS
LOCAL_FIRST_DEFAULT=PASS
REMOTE_SYNC_DEFAULT_OFF=PASS
SILENT_RECORDING_START=NO
HIDDEN_BACKGROUND_RECORDING=NO
SILENT_REMOTE_SYNC_ENABLEMENT=NO
SILENT_UPLOAD_OR_EXPORT=NO
SILENT_LOCATION_ACCESS=NO
CUSTOM_CLOUD_SYNC_STACK=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
