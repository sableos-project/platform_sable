# Sable Files / Documents UX

Status: design contract  
Target hardware: Titan 2 and keyboard-first Sable devices  
Scope: Files, Documents, Downloads, Attachments, File Picker, Share/Export, Recent/Captured Media handoff

## Decision

Files and Documents should be a single integrated productivity surface rather than separate, loosely related app designs.

```text
FILES_DOCUMENTS_UX=YES
FILES_AND_DOCUMENTS_MERGED_SURFACE=YES
DOWNLOADS_ATTACHMENTS_PICKER_SHARE_MERGED=YES
DOCUMENT_VIEWER_INCLUDED=YES
RECENT_CAPTURED_MEDIA_HANDOFF_INCLUDED=YES
ANDROID_STORAGE_ACCESS_MODEL=KEEP
SABLE_FILE_MANAGER_CLONE=NO
```

The user model is Android-compatible: files remain provider-owned, apps open documents through normal Android storage, picker and share flows, and Sable does not invent a custom filesystem or bypass scoped storage. Sable adds a keyboard-first productivity shell around those flows.

```text
ANDROID_DOCUMENTSUI_MENTAL_MODEL=KEEP
STORAGE_ACCESS_FRAMEWORK_COMPATIBLE=YES
MEDIASTORE_BOUNDARY_RESPECTED=YES
DOWNLOAD_PROVIDER_BOUNDARY_RESPECTED=YES
APP_PRIVATE_DATA_NOT_BROWSED_BY_DEFAULT=YES
ROOT_FILE_MANAGER_DEFAULT=NO
CUSTOM_FILESYSTEM_STACK=NO
CUSTOM_CLOUD_SYNC_STACK=NO
```

## Merged screens

This PR intentionally combines surfaces that would otherwise become separate PRs:

```text
SURFACE_1=FILES_HOME_RECENTS
SURFACE_2=DOWNLOADS_ATTACHMENTS
SURFACE_3=DOCUMENT_READER_PREVIEW
SURFACE_4=FILE_PICKER_SHARE_EXPORT
SURFACE_5=CAPTURED_MEDIA_HANDOFF
```

The combined contract is justified because the same core actions appear across all of them: open, search, filter, preview, share, move/copy, rename, delete, save-as, attach, export, and reveal source.

## Titan 2 productivity rule

Files follows the broader Titan 2 productivity rule landed in Mail/Calendar, Browser/WebView, Media/Gallery and Camera/Capture.

```text
TITAN2_PRODUCTIVITY_RULES_APPLY=YES
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
EXPLICIT_HISTORY_OR_PAGE_MODE=YES
RETURN_TO_CURRENT_OR_LATEST_KEY=YES
KEYBOARD_FIRST_FILES=YES
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
TEXT_INPUT_ALWAYS_WINS=YES
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
```

On open, Files must show current work and recent material first, not an empty directory tree.

```text
FILES_HOME_RECENTS_FIRST=YES
DOWNLOADS_LATEST_FIRST=YES
ATTACHMENTS_LATEST_FIRST=YES
CAPTURED_MEDIA_LATEST_FIRST=YES
LAST_OPENED_DOCUMENTS_VISIBLE=YES
CURRENT_FOLDER_OR_DOCUMENT_CONTEXT_VISIBLE=YES
```

## Files home

The home surface is a keyboard-first command center for local, downloaded, attached and captured files.

```text
FILES_HOME=YES
RECENTS_PANEL=YES
LOCATIONS_PANEL=YES
PREVIEW_PANEL=YES
ACTIONS_PANEL=YES
SOURCE_LABELS_VISIBLE=YES
PROVIDER_LABELS_VISIBLE=YES
```

Default group order:

```text
GROUP_1=Recent
GROUP_2=Downloads
GROUP_3=Attachments
GROUP_4=Camera and Gallery
GROUP_5=Documents
GROUP_6=Local storage
GROUP_7=Provider locations
```

Each row should show a human name first, then compact source and privacy hints. Raw paths and package/provider names are detail-mode information, not the default primary label.

```text
DISPLAY_NAME_PRIMARY=YES
RAW_PATH_DEFAULT_VISIBLE=NO
RAW_PROVIDER_URI_DEFAULT_VISIBLE=NO
SOURCE_PRIVACY_SUMMARY_VISIBLE=YES
DETAILS_REVEAL_RAW_PATH_AND_URI=YES
```

## Downloads and attachments

Downloads and attachments should not require a separate app model. They are filters inside Files with strong source labels.

```text
DOWNLOADS_AS_FILES_FILTER=YES
ATTACHMENTS_AS_FILES_FILTER=YES
MAIL_ATTACHMENT_HANDOFF=YES
MESSAGES_ATTACHMENT_HANDOFF=YES
BROWSER_DOWNLOAD_HANDOFF=YES
CAMERA_CAPTURE_HANDOFF=YES
GALLERY_HANDOFF=YES
```

Attachment handling must preserve provider ownership and user intent.

```text
ATTACHMENT_OPEN_SOURCE_VISIBLE=YES
ATTACHMENT_SAVE_AS_EXPLICIT=YES
ATTACHMENT_SHARE_USER_INITIATED=YES
SILENT_ATTACHMENT_EXPORT=NO
SILENT_CLOUD_UPLOAD=NO
```

## Document reader and preview

Files owns lightweight preview and reader shells, not every editor stack.

```text
DOCUMENT_PREVIEW=YES
DOCUMENT_READER=YES
PDF_PREVIEW=YES
TEXT_PREVIEW=YES
IMAGE_PREVIEW_HANDOFF=YES
MEDIA_PREVIEW_HANDOFF=YES
FULL_EDITING_HANDOFF_TO_PROVIDER_APP=YES
CUSTOM_OFFICE_SUITE=NO
```

The document reader is current-page/current-section first, not endless-scroll first.

```text
DOCUMENT_CURRENT_PAGE_VISIBLE=YES
DOCUMENT_PAGE_BLOCK_NAVIGATION=YES
DOCUMENT_FIND_REQUIRED=YES
DOCUMENT_OUTLINE_OPTIONAL=YES
DOCUMENT_CONTINUOUS_SCROLL_REQUIRED=NO
RETURN_TO_CURRENT_PAGE_KEY=YES
```

## File picker, save, share and export

The Android file picker and share flows are part of the same UX family. Sable must make them keyboard-first and source-safe.

```text
FILE_PICKER_INCLUDED=YES
SAVE_AS_INCLUDED=YES
SHARE_EXPORT_INCLUDED=YES
OPEN_WITH_INCLUDED=YES
PICKER_SOURCE_LABELS_VISIBLE=YES
PICKER_RECENTS_FIRST=YES
PICKER_SEARCH_REQUIRED=YES
PICKER_KEYBOARD_SELECT_REQUIRED=YES
```

Destructive and externalizing actions require explicit confirmation or user-initiated platform UI.

```text
DELETE_REQUIRES_CONFIRMATION=YES
MOVE_REQUIRES_VISIBLE_DESTINATION=YES
COPY_REQUIRES_VISIBLE_DESTINATION=YES
EXPORT_REQUIRES_VISIBLE_TARGET=YES
SHARE_SHEET_USER_INITIATED=YES
EXTERNAL_PROVIDER_UPLOAD_EXPLICIT=YES
```

## Captured media handoff

Camera, Gallery and Files must share a clean post-capture path.

```text
CAMERA_TO_FILES_HANDOFF=YES
GALLERY_TO_FILES_HANDOFF=YES
LATEST_CAPTURE_VISIBLE_IN_FILES=YES
CAPTURED_MEDIA_SOURCE_LABEL_VISIBLE=YES
CAPTURED_MEDIA_EXIF_LOCATION_VISIBILITY_EXPLICIT=YES
CAPTURED_MEDIA_DELETE_REQUIRES_CONFIRMATION=YES
```

## Keyboard model

Suggested shortcuts:

```text
/ or S = Search
Enter = Open / preview / choose
Space = Quick preview / play-pause where applicable
Back or Esc = Back / cancel
J or Down = Next item
K or Up = Previous item
Left or H = Parent / previous pane
Right or L = Detail / next pane
T = Today / latest / current group
D or Backspace = Delete with confirmation
R = Rename
M = Move
C = Copy
A = Attach / add / select all depending context
E = Export
I = Info / metadata
O = Open with
V = View mode
F = Favorite / pin
```

Input guard:

```text
TEXT_INPUT_ALWAYS_WINS=YES
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
SEARCH_FIELD_TEXT_NOT_HIJACKED=YES
FILENAME_TEXT_NOT_HIJACKED=YES
DOCUMENT_TEXT_SELECTION_NOT_HIJACKED=YES
```

## Privacy and security

Files is a high-risk privacy surface. It must be explicit about what is local, downloaded, attached, captured, private, locked, remote or provider-owned.

```text
PRIVATE_OR_LOCKED_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
LOCKSCREEN_FILE_CONTENT_REDACTED=YES
RECENTS_REDACT_IN_LOCKED_MODE=YES
EXIF_LOCATION_VISIBILITY_EXPLICIT=YES
HIDDEN_FILE_DISCOVERY_DEFAULT=NO
APP_PRIVATE_STORAGE_DEFAULT_HIDDEN=YES
```

No cloud provider should be silently treated as local storage.

```text
LOCAL_STORAGE_LABEL_VISIBLE=YES
REMOTE_PROVIDER_LABEL_VISIBLE=YES
SYNC_STATUS_VISIBLE_WHEN_PROVIDER_EXPOSES_IT=YES
OFFLINE_AVAILABILITY_VISIBLE_WHEN_PROVIDER_EXPOSES_IT=YES
SILENT_REMOTE_SYNC_ENABLEMENT=NO
```

## Integration points

```text
START_HANDOFF=YES
HUB_HANDOFF=YES
COMMAND_SEARCH_HANDOFF=YES
BROWSER_DOWNLOAD_HANDOFF=YES
MAIL_ATTACHMENT_HANDOFF=YES
MESSAGES_ATTACHMENT_HANDOFF=YES
CALENDAR_ATTACHMENT_HANDOFF=YES
CAMERA_HANDOFF=YES
GALLERY_HANDOFF=YES
MEDIA_HANDOFF=YES
NOTIFICATION_SHADE_DOWNLOAD_HANDOFF=YES
```

Command/Search should be able to find files by name and source summary, but private/locked state controls visibility.

```text
COMMAND_SEARCH_FILE_RESULTS=YES
COMMAND_SEARCH_PRIVATE_FILE_REDACTION=YES
COMMAND_SEARCH_REMOTE_PROVIDER_LABEL=YES
```

## Non-goals

```text
ROOT_FILE_MANAGER=NO
CUSTOM_FILESYSTEM=NO
CUSTOM_CLOUD_SYNC=NO
CUSTOM_MEDIASTORE_PROVIDER=NO
CUSTOM_DOWNLOAD_PROVIDER=NO
BYPASS_SCOPED_STORAGE=NO
SILENT_UPLOAD_OR_EXPORT=NO
```

## Acceptance markers

```text
FILES_DOCUMENTS_UX=PASS
FILES_AND_DOCUMENTS_MERGED_SURFACE=PASS
DOWNLOADS_ATTACHMENTS_PICKER_SHARE_MERGED=PASS
DOCUMENT_VIEWER_INCLUDED=PASS
RECENT_CAPTURED_MEDIA_HANDOFF_INCLUDED=PASS
ANDROID_STORAGE_ACCESS_MODEL=PASS
TITAN2_PRODUCTIVITY_RULES_APPLY=PASS
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=PASS
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
KEYBOARD_FIRST_FILES=PASS
TEXT_INPUT_ALWAYS_WINS=PASS
PRIVATE_OR_LOCKED_MODE_REDACTION=PASS
SOURCE_RESTRICTED_MODE=PASS
SILENT_REMOTE_SYNC_ENABLEMENT=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
