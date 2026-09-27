# Keyboard-First System Image Coverage Audit

STATUS=DESIGN_COVERAGE_AUDIT
TARGET=Titan-family N0 system.img planning
SOURCE_REPOS=platform_sable,aimindseye/sableos,device_sable_titan2
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO

## Purpose

This audit verifies that the keyboard-first SableOS design surface is represented as build-facing system image expectations before the first Titan-family N0 build.

It exists because design coverage alone is not enough: a feature can be discussed and documented, yet still be absent from the product/package/system image plan. App Display Compatibility was the example that forced this audit.

```text
KEYBOARD_FIRST_SYSTEM_IMG_COVERAGE_AUDIT=YES
DESIGN_TO_SYSTEM_IMG_TRACEABILITY=YES
MISSING_BIG_ITEM_PREVENTION=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Coverage rules

Every major feature must answer:

```text
WHAT_IS_THE_USER_SURFACE
WHAT_PACKAGE_OR_SYSTEM_SURFACE_OWNS_IT
WHAT_SYSTEM_IMG_ARTIFACT_SHOULD_EXIST
WHAT_IS_GATED
WHAT_MUST_NOT_BE_CLAIMED_WITHOUT_RUNTIME_PROOF
```

N0 may carry UI, profile state and validation hooks, but must not claim hardware/backend success without evidence.

## Keyboard-first principles that must survive system image planning

```text
PHYSICAL_KEYBOARD_FIRST=YES
ONSCREEN_KEYBOARD_FALLBACK=YES
TOUCH_OPTIONAL=YES
VISIBLE_FOCUS_REQUIRED=YES
TEXT_INPUT_ALWAYS_WINS=YES
SHORTCUTS_DISABLED_WHILE_TYPING=YES
CURRENT_OR_LATEST_RELEVANT_CONTENT_VISIBLE=YES
CONTINUOUS_TOUCHPAD_SCROLLING_REQUIRED=NO
NO_FOCUS_TRAPS=YES
PROVIDER_OWNERSHIP_RETAINED=YES
ANDROID_MENTAL_MODELS_KEPT=YES
```

## Design-to-system coverage matrix

| Design surface | Existing design coverage | System image coverage expectation | Audit result |
| --- | --- | --- | --- |
| Sable Start / Launcher | Sable Start keyboard-first UX, three-page model, base quick bar, type-to-launch, pointer behavior | SableLauncher HOME, Launcher3 not HOME, Quickstep retained for Recents | COVERED_REQUIRES_BUILD_CHECK |
| Command / Search | Global command/search local-first model | Command entry reachable from Start, Settings, Hub and supported surfaces | COVERED_REQUIRES_BUILD_CHECK |
| Hub | Communications-first Hub, provider ownership retained, local priority | `org.sableos.hub` present and provider-bounded | COVERED_REQUIRES_BUILD_CHECK |
| All Apps | privacy/security summaries, app actions, package names not primary | launcher/app list and Settings application detail surfaces | COVERED_REQUIRES_BUILD_CHECK |
| Quick Settings / Shade | keyboard shade, compact/expanded QS, profile-gated hardware controls | SystemUI/QS profile overlay and redaction behavior | COVERED_REQUIRES_SYSTEMUI_CHECK |
| Lockscreen | keyboard secure entry, redaction, emergency path | Keyguard input/fallback/redaction behavior | COVERED_REQUIRES_SYSTEMUI_CHECK |
| Setup Wizard | keyboard verification, Wi-Fi password, privacy/theme | first boot/provisioning flow | COVERED_REQUIRES_SETUP_CHECK |
| Phone | physical keyboard dialing, onscreen dialpad, emergency path | phone/dialer package and telephony boundary | COVERED_REQUIRES_RUNTIME_CHECK |
| Contacts | A-Z rail, search, provider-safe people actions | Contacts/People package and provider labels | COVERED_REQUIRES_RUNTIME_CHECK |
| Messages | latest-anchored conversations, keyboard composer, no custom SMS/RCS/MMS stack | Sable Messages UI plus existing transport boundary | COVERED_REQUIRES_RUNTIME_CHECK |
| Mail / Calendar | current/latest visible productivity UX | Sable Mail and Calendar surfaces with Hub snapshot boundary | COVERED_REQUIRES_PACKAGE_CHECK |
| Browser / WebView | Vanadium identity, WebView provider boundary, keyboard find/page mode | browser/WebView provider labels and diagnostics | COVERED_REQUIRES_STRING_CHECK |
| Media / Gallery | media now-playing, gallery contact sheet, MediaStore boundary | Sable Media/Gallery packages and MediaStore boundary | COVERED_REQUIRES_PACKAGE_CHECK |
| Camera | camera quality matrix, telephoto/pro/raw gates | Sable Camera profile and HAL validation gates | COVERED_HARDWARE_GATED |
| Files / Documents | SAF/DocumentsUI-compatible files/docs/picker/share | files/docs surface without custom filesystem stack | COVERED_REQUIRES_PACKAGE_CHECK |
| Utilities | Notes, Recorder, Clock, Calculator/Converter, Tasks | utility packages/surfaces and local-first defaults | COVERED_REQUIRES_PACKAGE_CHECK |
| Toolbox | sensors, IR Remote, FM gate | Toolbox package with sensor/IR/FM gates | COVERED_HARDWARE_GATED |
| Mobile Manager / App Controls | network/app/background/freezer/student controls | Settings/Mobile Manager deep links; no duplicate network policy | COVERED_REQUIRES_AUTHORITY_CHECK |
| Private Space / Dual Apps | vault/profile gates, app lock, freezer | Settings/privacy/profile surfaces and no fake isolation | COVERED_PROFILE_GATED |
| Connectivity | OTG, NFC, Cast, tethering, Bluetooth transfer, Mini Mode/rotation | Connected Devices surfaces and hardware gates | COVERED_HARDWARE_GATED |
| App Display Compatibility | per-app display profiles, 16:9, Mini Mode as profile, rotation as dimension | Settings > Display/App display compatibility; app-info handoff; backend gated | COVERED_NEWLY_ADDED |
| Titan-family base matrix | shared/gated features across Titan 2 and Elite | main/device repo matrices and runtime gates | COVERED_REQUIRES_BUILD_PREFLIGHT |

## Newly closed gap: App Display Compatibility

The earlier Mini Mode / Rotation Control coverage was incomplete. The corrected coverage treats App Display Compatibility as a parent feature.

```text
APP_DISPLAY_COMPATIBILITY_PROFILES=YES
PER_APP_DISPLAY_PROFILE=YES
PER_APP_ASPECT_RATIO=YES
CUSTOM_16_9_PROFILE=YES
MINI_MODE_AS_PROFILE=YES
ROTATION_CONTROL_AS_PROFILE_DIMENSION=YES
KEYBOARD_SAFE_PROFILE=YES
RESET_OUTSIDE_APP=YES
WINDOW_MANAGER_BACKEND_GATED=YES
```

System image expectation:

```text
SETTINGS_DISPLAY_COMPATIBILITY_SURFACE=YES
APP_INFO_DISPLAY_COMPATIBILITY_ENTRY=YES
LAUNCHER_APP_INFO_HANDOFF=YES
PROFILE_STORAGE_ALLOWED=YES
NOOP_STATE_ALLOWED=YES
FORCE_ASPECT_RATIO_CLAIM_WITHOUT_VALIDATION=NO
```

## Potential remaining gap classes

No new missing major feature was found in this pass, but these classes remain high risk until preflight maps packages/modules:

```text
DESIGN_DOC_EXISTS_BUT_PACKAGE_ABSENT=HIGH_RISK
FEATURE_SURFACE_EXISTS_BUT_STRING_BRAND_WRONG=HIGH_RISK
UI_EXISTS_BUT_AUTHORITY_WRONG=HIGH_RISK
PROFILE_GATED_FEATURE_EXPOSED_AS_WORKING=HIGH_RISK
HARDWARE_FEATURE_CLAIM_WITHOUT_HAL_PROOF=HIGH_RISK
SETTINGS_SURFACE_EXISTS_BUT_RESET_PATH_MISSING=HIGH_RISK
KEYBOARD_SHORTCUT_EXISTS_BUT_TEXT_INPUT_BROKEN=HIGH_RISK
```

## System image preflight checklist

Before a full N0 build or artifact qualification, check:

```text
PRODUCT_MAKEFILES_AUDITED=YES
PRODUCT_PACKAGES_AUDITED=YES
SYSTEM_EXT_PACKAGES_AUDITED=YES
PRIV_APP_PACKAGES_AUDITED=YES
DEFAULT_HOME_AUDITED=YES
INPUT_KEYLAYOUTS_AUDITED=YES
SETTINGS_SURFACES_AUDITED=YES
SYSTEMUI_SURFACES_AUDITED=YES
APP_INFO_SURFACES_AUDITED=YES
PERMISSION_CONTROLLER_BOUNDARY_AUDITED=YES
BRAND_STRINGS_AUDITED=YES
VENDOR_HARDWARE_ASSUMPTIONS_BLOCKED=YES
FLASH_AUTHORIZED=NO
```

## Must-not-regress list

```text
SABLE_START_HOME_NOT_LAUNCHER3_HOME
QUICKSTEP_RETAINED_FOR_RECENTS_ONLY
SETTINGS_APPEARANCE_AUTHORITY_RETAINED
SABLE_APPS_FOLLOW_SYSTEM_THEME
HUB_DOES_NOT_OWN_MAIL_DATABASE
MAIL_OWNS_MAIL_ACCOUNTS_AND_SYNC
MESSAGES_DOES_NOT_CLAIM_CUSTOM_SMS_RCS_MMS_STACK
NETWORK_MANAGER_AUTHORITY_STAYS_SETTINGS_OWNED
MOBILE_MANAGER_DOES_NOT_OWN_DUPLICATE_NETWORK_POLICY
VANADIUM_NOT_LABELED_CHROME
PACKAGE_NAMES_NOT_PRIMARY_APP_LABELS
SUBSCREEN_NOT_PROMOTED_TO_ELITE_BASE
FM_RADIO_NOT_ENABLED_WITHOUT_PROOF
APP_DISPLAY_BACKEND_NOT_CLAIMED_WITHOUT_WINDOWMANAGER_PROOF
```

## N0 build interpretation

```text
N0_BUILD_CAN_INCLUDE_SURFACES=YES
N0_BUILD_CAN_INCLUDE_STUBS=YES
N0_BUILD_CAN_INCLUDE_VALIDATION_HOOKS=YES
N0_BUILD_CAN_INCLUDE_NOOP_PROFILE_STATE=YES
N0_BUILD_CAN_ASSUME_HARDWARE_WORKS=NO
N0_BUILD_CAN_ASSUME_VENDOR_SERVICE_WORKS=NO
N0_BUILD_CAN_FLASH=NO
```

## Acceptance criteria

```text
KEYBOARD_FIRST_SYSTEM_IMG_COVERAGE_AUDIT=PASS
DESIGN_TO_SYSTEM_IMG_TRACEABILITY=PASS
APP_DISPLAY_COMPATIBILITY_CLOSED=PASS
NO_NEW_MAJOR_FEATURE_GAP_FOUND=PASS_WITH_PREFLIGHT_REQUIRED
SYSTEM_IMG_MATRIX_REQUIRED=PASS
DEVICE_GATE_MATRIX_REQUIRED=PASS
NO_FLASH_ENABLEMENT=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
