# Sable Media / Gallery UX

Status: design contract
Scope: SableOS common UX with Titan 2 profile requirements
Target hardware template: square display plus physical keyboard

## Purpose

Sable Media / Gallery defines the user experience for local and provider-backed audio,
video, photos, screenshots and downloaded media on keyboard-first Sable devices.

The design uses Zune / Windows-inspired typography, dark immersive media surfaces,
large current-item presentation and clear command labels. It does not clone Zune,
Windows Phone, Windows Media Player or any third-party product. Gallery uses a
more practical keyboard-first contact sheet and focused viewer model because photo
and video management requires predictable item selection, metadata, sharing and
confirmation flows.

```text
MEDIA_GALLERY_UX=YES
ZUNE_WINDOWS_STYLE_FOR_MEDIA_PLAYER=YES
ZUNE_WINDOWS_STYLE_FOR_GALLERY=PARTIAL_ONLY
METRO_CLARITY_NOT_METRO_CLONE=YES
GALLERY_KEYBOARD_FIRST_CONTACT_SHEET=YES
TITAN2_PRODUCTIVITY_RULES_APPLY=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Global Titan 2 media rules

Media and Gallery inherit the Titan 2 productivity rule established for Messages,
Mail, Calendar and Browser: current or latest relevant content must be visible
without requiring touchpad-style continuous scrolling.

```text
CURRENT_OR_LATEST_RELEVANT_CONTENT_ALWAYS_VISIBLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
EXPLICIT_HISTORY_OR_PAGE_MODE=YES
RETURN_TO_CURRENT_OR_LATEST_KEY=YES
KEYBOARD_FIRST_MEDIA=YES
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
TEXT_INPUT_ALWAYS_WINS=YES
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
```

## Surface model

Sable Media / Gallery has four primary visual states:

```text
VISUAL_STATE_1=MEDIA_NOW_PLAYING
VISUAL_STATE_2=MEDIA_QUEUE_LIBRARY
VISUAL_STATE_3=GALLERY_LATEST_CONTACT_SHEET
VISUAL_STATE_4=GALLERY_VIEWER
```

The states may live in one app shell or closely-linked app surfaces. The UX contract
is shared either way.

## Media player visual direction

Media playback should use the strongest Zune / Windows influence:

- large type and strong hierarchy;
- dark immersive canvas;
- album art or video frame as the dominant visual;
- current track or video metadata always visible;
- minimal chrome;
- keyboard shortcut labels shown as direct affordances;
- queue/library as explicit modes, not hidden scroll-only regions.

```text
MEDIA_NOW_PLAYING_PRIMARY=YES
CURRENT_TRACK_ALWAYS_VISIBLE=YES
CURRENT_VIDEO_ALWAYS_VISIBLE=YES
PLAYBACK_CONTROLS_ALWAYS_KEYBOARD_REACHABLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_NOW_PLAYING=NO
QUEUE_ACCESS_EXPLICIT=YES
LIBRARY_ACCESS_EXPLICIT=YES
LYRICS_METADATA_OR_CHAPTERS_PAGE_MODE=YES
```

### Now playing state

The default playback state centers the current audio track or video. Controls must
be visible and keyboard reachable at all times. Long lyrics, metadata, chapters,
comments or descriptions move into explicit page/detail modes rather than pushing
current playback state off screen.

Required visible regions:

```text
TOP_STATUS_STRIP=YES
CURRENT_ART_OR_VIDEO_FRAME=YES
TITLE_ARTIST_ALBUM_OR_VIDEO_TITLE=YES
PLAYBACK_POSITION=YES
PRIMARY_CONTROLS=YES
SECONDARY_ACTIONS=YES
BOTTOM_COMMAND_DOCK=YES
```

### Queue and library state

Queue/library uses page or list-focus navigation. The active queue item remains
visibly connected to the currently playing item.

```text
QUEUE_VISIBLE_ON_REQUEST=YES
QUEUE_CURRENT_ITEM_MARKED=YES
QUEUE_NEXT_PREVIOUS_KEYBOARD_FIRST=YES
LIBRARY_SEARCH_EXPLICIT=YES
ALBUM_ARTIST_PLAYLIST_FILTERS_EXPLICIT=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_QUEUE=NO
```

## Gallery visual direction

Gallery must not be treated as a music-player clone. It uses a keyboard-first
contact sheet with the newest media visible first and a persistent focused-item
preview.

```text
GALLERY_LATEST_FIRST=YES
LATEST_MEDIA_VISIBLE_ON_OPEN=YES
FOCUSED_ITEM_PREVIEW_VISIBLE=YES
CONTACT_SHEET_GRID=YES
PAGE_GRID_NAVIGATION=YES
ALBUMS_DATES_AND_TYPES_EXPLICIT=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_LATEST=NO
```

### Contact sheet state

The contact sheet opens to the latest local/camera/screenshot/download media that
the user is allowed to see. The focused thumbnail exposes enough context to act
without opening every item.

Required visible regions:

```text
TOP_STATUS_STRIP=YES
LATEST_GROUP_HEADER=YES
BOUNDED_THUMBNAIL_GRID=YES
FOCUSED_ITEM_PREVIEW=YES
FOCUSED_ITEM_ACTIONS=YES
BOTTOM_COMMAND_DOCK=YES
```

### Viewer state

The viewer keeps the current photo or video centered. Zoom, pan, metadata, share,
edit and delete are explicit modes or actions. Pan and zoom must not become hidden
requirements for normal viewing.

```text
VIEWER_CURRENT_ITEM_ALWAYS_VISIBLE=YES
NEXT_PREVIOUS_KEYBOARD_FIRST=YES
ZOOM_MODE_EXPLICIT=YES
PAN_MODE_EXPLICIT=YES
METADATA_PANEL_EXPLICIT=YES
SHARE_ACTION_EXPLICIT=YES
DELETE_REQUIRES_CONFIRMATION=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_ITEM=NO
```

## Input contract

Shared keys should feel mnemonic and consistent with Phone, Messages, Mail,
Calendar and Browser. Single-key shortcuts are disabled while a text input,
search prompt, rename field, caption field or command dock prompt is active.

```text
SPACE=PLAY_PAUSE_FOR_MEDIA_OR_VIDEO
J_OR_LEFT=PREVIOUS_ITEM
K_OR_RIGHT=NEXT_ITEM
ENTER=OPEN_OR_ACTIVATE
BACK_OR_ESC=BACK_OR_CANCEL
SLASH_OR_S=SEARCH
V=VIEW_MODE
F=FAVORITE
D_OR_BACKSPACE=DELETE_WITH_CONFIRMATION
Q=QUEUE
I=INFO_METADATA
T=TODAY_OR_LATEST
A=ALBUMS
Z=ZOOM_MODE
TEXT_INPUT_ALWAYS_WINS=YES
```

## Privacy and source boundaries

Media/Gallery uses Android media ownership boundaries. It must not silently ingest,
merge, index or expose provider media beyond platform and user-granted access.
Metadata and sharing surfaces must be explicit.

```text
ANDROID_MEDIASTORE_MENTAL_MODEL=KEEP
PROVIDER_OWNERSHIP_RETAINED=YES
CUSTOM_MEDIASTORE_PROVIDER=NO
CUSTOM_CODEC_STACK=NO
CUSTOM_DRM_STACK=NO
SILENT_MEDIA_IMPORT=NO
EXIF_LOCATION_VISIBILITY_EXPLICIT=YES
SHARE_SHEET_USER_INITIATED=YES
DELETE_REQUIRES_CONFIRMATION=YES
PRIVATE_OR_LOCKED_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
```

## Hub, Command and notification handoff

Media controls may appear in Hub, Lockscreen and Notification Shade only through
provider-safe media-session surfaces. Gallery actions are user-initiated and should
not leak private thumbnails into locked/private modes.

```text
MEDIA_SESSION_HANDOFF=YES
LOCKSCREEN_MEDIA_CONTROLS_PROVIDER_SAFE=YES
NOTIFICATION_SHADE_MEDIA_HANDOFF=YES
HUB_NOW_PLAYING_HANDOFF=YES
COMMAND_SEARCH_MEDIA_RESULTS=YES
COMMAND_SEARCH_PRIVATE_MEDIA_REDACTION=YES
GALLERY_THUMBNAILS_REDACTED_WHEN_LOCKED=YES
```

## Non-goals

```text
ZUNE_CLONE=NO
WINDOWS_MEDIA_PLAYER_CLONE=NO
CUSTOM_MEDIASTORE_PROVIDER=NO
CUSTOM_CODEC_STACK=NO
CUSTOM_DRM_STACK=NO
SILENT_CLOUD_SYNC=NO
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
