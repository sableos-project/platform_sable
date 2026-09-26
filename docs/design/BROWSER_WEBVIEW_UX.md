# Browser / WebView UX

Status: design contract for Titan 2 keyboard-first SableOS planning.

This document defines the Sable Browser / WebView user experience. It is a UX and validation contract only. It does not import browser code, fork a WebView provider, change target images, enable flashing, or change production signing.

```text
BROWSER_WEBVIEW_UX=YES
ANDROID_BROWSER_MENTAL_MODEL=KEEP
TITAN2_PRODUCTIVITY_RULES_APPLY=YES
KEYBOARD_FIRST_BROWSER=YES
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Product identity and labeling

SableOS must not present a Vanadium-backed or Sable-default browser as Chrome. Browser identity must be accurate, user-facing, and consistent across Start, All Apps, Settings, Command/Search, default-app selection, permission prompts, app info and WebView diagnostics.

```text
BROWSER_LABEL_ACCURATE=YES
VANADIUM_LABEL_IF_VANADIUM_BACKED=YES
CHROME_LABEL_FOR_VANADIUM=NO
RAW_PACKAGE_NAME_DEFAULT_VISIBLE=NO
WEBVIEW_PROVIDER_LABEL_VISIBLE_IN_DIAGNOSTICS=YES
BROWSER_PROVIDER_BOUNDARY_EXPLICIT=YES
```

If the installed browser is Vanadium, user-facing surfaces should say `Vanadium` or `Sable Browser (Vanadium)` according to product branding. They must not say `Chrome` unless Chrome is actually installed and selected. Developer details and package names remain available through App info / developer views, not as the default user-facing label.

## Titan 2 productivity rule

Browser and WebView surfaces inherit the Titan 2 productivity rule introduced for Mail / Calendar and Messages.

```text
CURRENT_OR_RELEVANT_CONTENT_ALWAYS_VISIBLE=YES
CONTINUOUS_TOUCHPAD_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
EXPLICIT_PAGE_FIND_LINK_OR_READER_MODE=YES
RETURN_TO_CURRENT_OR_TOP_KEY=YES
BROWSER_CHROME_ALWAYS_KEYBOARD_REACHABLE=YES
```

Web pages can be longer than the square display, so the rule does not mean every page fits at once. It means users must not need a touchpad-style continuous scroll path to reach primary controls, current focus, find results, form fields, or reading progress. Long content must be handled through explicit page blocks, find-in-page, link/headline navigation, reader mode, top/bottom jumps and keyboard focus traversal.

## Browser structure

The default Browser surface uses Android-compatible browser expectations: address/search field, tabs, back/forward/reload, share, downloads, site controls, permissions and private browsing.

```text
ADDRESS_SEARCH_FIELD=YES
TAB_MODEL=YES
BACK_FORWARD_RELOAD=YES
PRIVATE_TAB_MODE=YES
DOWNLOADS_SURFACE=YES
SITE_INFO_PRIVACY_SURFACE=YES
FIND_IN_PAGE=YES
READER_MODE_CANDIDATE=YES
```

On Titan 2, the address/search field is reachable by keyboard from anywhere in the Browser. Tab and page state must remain understandable without relying on touch gestures.

## Keyboard model

Shortcuts must be deterministic and must never steal text from active web inputs, the address field, search fields, login forms, payment forms, text areas, editors, or the Sable command dock.

```text
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
TEXT_INPUT_ALWAYS_WINS=YES
WEB_FORM_INPUT_NOT_HIJACKED=YES
INPUT_GUARD_REQUIRED=YES
```

Recommended Browser key model:

```text
Ctrl+L = focus address/search
Ctrl+T = new tab
Ctrl+W = close tab, with undo affordance where available
Ctrl+Tab = next tab
Ctrl+Shift+Tab = previous tab
Alt+Left = back
Alt+Right = forward
Ctrl+R = reload
/ = find in page when no text field is active
Esc = close find/dialog/menu or return focus to page
Enter = open focused link/result/control
Space = explicit page block down, not touchpad-style scroll
Shift+Space = explicit page block up
T = top/current page start in reader/page mode only
B = bottom/end in reader/page mode only
R = reader mode toggle where available and no text field is active
L = link navigation mode where available and no text field is active
```

Single-letter shortcuts are allowed only in Browser-owned chrome, reader mode, or an explicit keyboard-navigation mode. They must not fire while a web page or form owns text input focus.

## No-scroll-cost reading model

Browser reading should be keyboard-first without forcing touchpad scrolling. The page viewport may move in bounded blocks, but current controls and reading state must remain predictable.

```text
PAGE_BLOCK_NAVIGATION=YES
CONTINUOUS_SCROLL_REQUIRED_TO_READ=NO
FIND_IN_PAGE_REQUIRED=YES
FOCUSED_LINK_VISIBLE=YES
FOCUSED_FORM_FIELD_VISIBLE=YES
RETURN_TO_TOP_KEY=YES
RETURN_TO_CURRENT_FOCUS_KEY=YES
READER_MODE_REDUCES_LAYOUT_COMPLEXITY=YES
```

Long articles should offer a reader-mode path when supported. Complex sites that cannot be simplified still require keyboard focus traversal, find-in-page, page-block navigation and visible focus.

## Tabs and private browsing

Tab overview must be usable with only the physical keyboard and must not expose private tab content in locked/private surfaces.

```text
TAB_OVERVIEW_KEYBOARD_ACCESSIBLE=YES
ACTIVE_TAB_VISIBLE=YES
TAB_TITLES_READABLE=YES
PRIVATE_TAB_VISUALLY_DISTINCT=YES
LOCKED_MODE_TAB_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
```

Tab previews on lockscreen, Hub, notification shade or Command/Search must be redacted according to privacy state. Private tabs should never surface page titles or previews outside the private browsing surface.

## WebView model

Embedded WebView surfaces keep app ownership. Browser UX may offer consistent controls, but the embedding app owns its account, content, session, private storage and navigation intent.

```text
WEBVIEW_PROVIDER_BOUNDARY=YES
EMBEDDING_APP_OWNS_SESSION=YES
NO_CROSS_APP_HISTORY_LEAKAGE=YES
OPEN_IN_BROWSER_ACTION=YES
COPY_LINK_ACTION=YES
SHARE_ACTION_PROVIDER_SAFE=YES
WEBVIEW_PERMISSION_PROMPTS_KEYBOARD_ACCESSIBLE=YES
```

WebView prompts must make the source app and web origin visible. Permission prompts for camera, microphone, location, notifications, downloads and external app launches must be keyboard accessible and must not default to allow without user confirmation.

## Site permissions and privacy

Browser and WebView must expose site identity, permissions and storage in language users can understand.

```text
SITE_IDENTITY_VISIBLE=YES
TLS_WARNING_NOT_BYPASSED_BY_KEYBOARD=YES
PERMISSION_PROMPTS_KEYBOARD_ACCESSIBLE=YES
CAMERA_MIC_LOCATION_PROMPTS_EXPLICIT=YES
DOWNLOAD_PROMPTS_EXPLICIT=YES
CLEAR_SITE_DATA_VISIBLE=YES
DEFAULT_PRIVACY_CONTROLS_VISIBLE=YES
```

Keyboard shortcuts must not bypass TLS warnings, mixed-content warnings, dangerous download prompts, external-app launch prompts or permission confirmations.

## Command/Search and All Apps integration

Browser integrates with Sable Command/Search and All Apps without exposing private browsing state or raw package names by default.

```text
COMMAND_SEARCH_BROWSER_RESULTS=YES
COMMAND_SEARCH_PRIVATE_TAB_REDACTION=YES
ALL_APPS_BROWSER_LABEL_ACCURATE=YES
DEFAULT_BROWSER_SETTINGS_DEEP_LINK=YES
WEBVIEW_DIAGNOSTICS_DEEP_LINK=YES
```

Command/Search can surface open normal tabs, bookmarks and browser actions only when the relevant privacy state allows it. It must not expose private tab titles, private URLs or app-owned WebView contents.

## Visual states

The source-controlled Titan 2 visual artifact for this contract must show four states:

```text
VISUAL_STATE_1=BROWSER_START_SEARCH
VISUAL_STATE_2=BROWSER_READER_PAGE_MODE
VISUAL_STATE_3=TAB_OVERVIEW_PRIVACY
VISUAL_STATE_4=WEBVIEW_PERMISSION_EXTERNAL_OPEN
```

The artifact must use the Titan 2 hardware template: square display plus physical keyboard, no stretched slab-phone screen.

## Validation

```text
BROWSER_WEBVIEW_UX=PASS
BROWSER_LABEL_ACCURATE=PASS
VANADIUM_LABEL_IF_VANADIUM_BACKED=PASS
CHROME_LABEL_FOR_VANADIUM=NO
ANDROID_BROWSER_MENTAL_MODEL=PASS
TITAN2_PRODUCTIVITY_RULES_APPLY=PASS
CONTINUOUS_TOUCHPAD_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
KEYBOARD_FIRST_BROWSER=PASS
PHYSICAL_KEYBOARD_FIRST=PASS
ONSCREEN_KEYBOARD_FALLBACK=PASS
TEXT_INPUT_ALWAYS_WINS=PASS
WEB_FORM_INPUT_NOT_HIJACKED=PASS
FIND_IN_PAGE_REQUIRED=PASS
TAB_OVERVIEW_KEYBOARD_ACCESSIBLE=PASS
WEBVIEW_PROVIDER_BOUNDARY=PASS
NO_CROSS_APP_HISTORY_LEAKAGE=PASS
PERMISSION_PROMPTS_KEYBOARD_ACCESSIBLE=PASS
TLS_WARNING_NOT_BYPASSED_BY_KEYBOARD=PASS
COMMAND_SEARCH_PRIVATE_TAB_REDACTION=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
