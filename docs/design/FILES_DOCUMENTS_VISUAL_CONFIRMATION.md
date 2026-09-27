# Files / Documents Visual Confirmation

Status: design validation checklist  
Artifact: `docs/design/artifacts/sable-files-documents-titan2.svg`

## Visual goal

The Files / Documents visual target must prove that Files is not a slab-phone directory clone. It must show a Titan 2 square display with physical keyboard, current/latest content visible by default, and merged productivity surfaces.

```text
FILES_DOCUMENTS_VISUAL_CONFIRMATION=YES
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
```

## Required visual states

```text
VISUAL_STATE_1=FILES_HOME_RECENTS
VISUAL_STATE_2=DOWNLOADS_ATTACHMENTS
VISUAL_STATE_3=DOCUMENT_READER_PREVIEW
VISUAL_STATE_4=FILE_PICKER_SHARE_EXPORT
```

These four states cover the merged surfaces requested in the UX contract. Camera/Gallery captured-media handoff is represented inside state 1 and state 2 rather than as a separate PR.

## State 1: Files home / recents

Must show:

```text
RECENTS_FIRST=YES
LOCATIONS_PANEL=YES
PREVIEW_PANEL=YES
ACTIONS_PANEL=YES
SOURCE_LABELS_VISIBLE=YES
CAPTURED_MEDIA_HANDOFF_VISIBLE=YES
```

Expected content:

```text
Recent
Downloads
Attachments
Camera / Gallery
Documents
Local storage
Provider locations
```

Validation:

```text
LATEST_RELEVANT_FILES_VISIBLE=PASS
CURRENT_SELECTION_PREVIEW_VISIBLE=PASS
RAW_PATH_DEFAULT_VISIBLE=NO
PROVIDER_LABELS_VISIBLE=PASS
```

## State 2: Downloads / attachments

Must show:

```text
DOWNLOADS_LATEST_FIRST=YES
ATTACHMENTS_LATEST_FIRST=YES
SOURCE_APP_VISIBLE=YES
SAVE_AS_EXPLICIT=YES
SHARE_USER_INITIATED=YES
```

Expected sources:

```text
Browser download
Mail attachment
Messages attachment
Camera capture
Gallery export
```

Validation:

```text
DOWNLOADS_ATTACHMENTS_MERGED=PASS
MAIL_MESSAGES_BROWSER_HANDOFF=PASS
SILENT_ATTACHMENT_EXPORT=NO
SILENT_CLOUD_UPLOAD=NO
```

## State 3: Document reader / preview

Must show:

```text
CURRENT_PAGE_VISIBLE=YES
PAGE_BLOCK_NAVIGATION=YES
FIND_IN_DOCUMENT=YES
OUTLINE_OR_METADATA_PANEL=YES
FULL_EDITING_HANDOFF_VISIBLE=YES
```

Validation:

```text
DOCUMENT_CONTINUOUS_SCROLL_REQUIRED=NO
DOCUMENT_FIND_REQUIRED=PASS
DOCUMENT_CURRENT_CONTEXT_VISIBLE=PASS
```

## State 4: File picker / share / export

Must show:

```text
PICKER_RECENTS_FIRST=YES
SEARCH_REQUIRED=YES
DESTINATION_VISIBLE=YES
OPEN_WITH_VISIBLE=YES
EXPORT_TARGET_VISIBLE=YES
CONFIRMATION_FOR_DESTRUCTIVE_ACTIONS=YES
```

Validation:

```text
FILE_PICKER_KEYBOARD_ACCESSIBLE=PASS
SAVE_AS_KEYBOARD_ACCESSIBLE=PASS
SHARE_EXPORT_KEYBOARD_ACCESSIBLE=PASS
DELETE_REQUIRES_CONFIRMATION=PASS
EXTERNAL_PROVIDER_UPLOAD_EXPLICIT=PASS
```

## Shared keyboard confirmation

The visual artifact must include command strips or hints for:

```text
Enter Open
/ Search
J/K Move
I Info
E Export
O Open with
D Delete
Back Cancel
```

Input guard:

```text
TEXT_INPUT_ALWAYS_WINS=PASS
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=PASS
SEARCH_FIELD_TEXT_NOT_HIJACKED=PASS
FILENAME_TEXT_NOT_HIJACKED=PASS
```

## Privacy and source-state confirmation

The visual must make it clear that Files is privacy-sensitive.

```text
LOCKED_MODE_REDACTION=PASS
PRIVATE_MODE_REDACTION=PASS
SOURCE_RESTRICTED_MODE=PASS
LOCAL_REMOTE_PROVIDER_LABELS=PASS
EXIF_LOCATION_VISIBILITY_EXPLICIT=PASS
```

## Build posture

```text
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
