# Setup Wizard / First Boot UX

Status: **Titan 2 keyboard-first design contract — 2026-09-26**

Setup Wizard / First Boot is the first user-facing system flow after a Sable image boots. It must prove that the device can be set up without touch-only assumptions, without unsafe text-entry traps, and without hiding privacy/security choices behind unsupported provider flows.

```text
SETUP_WIZARD_FIRST_BOOT_UX=YES
ANDROID_SETUP_MENTAL_MODEL=KEEP
KEYBOARD_FIRST_ONBOARDING=YES
CRITICAL_TEXT_ENTRY_GATE=YES
PHYSICAL_KEYBOARD_VERIFICATION=YES
SOFTWARE_KEYBOARD_FALLBACK=YES
DARK_THEME_SELECTION=YES
SABLE_APPS_FOLLOW_SYSTEM_THEME=YES
PRIVACY_POSTURE_EXPLICIT=YES
RESTORE_IMPORT_FAIL_CLOSED=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Scope

This contract covers the product and interaction behavior expected from Sable first boot on keyboard devices, starting with Titan 2.

It does not authorize build changes, flash enablement, account-provider changes, firmware changes, or production signing changes.

## First-boot principles

Setup Wizard must preserve the familiar Android setup mental model while making Sable-specific privacy and keyboard requirements explicit.

```text
ANDROID_SETUP_MENTAL_MODEL=KEEP
SABLE_BRANDING_ALLOWED=YES
GRAPHENEOS_BRANDING_IN_USER_UI=NO
TOUCH_ONLY_SETUP=NO
KEYBOARD_ONLY_SETUP=REQUIRED
SKIP_PATHS_VISIBLE=REQUIRED
```

The user must be able to complete setup with the Titan 2 physical keyboard and pointer/touch as secondary inputs. Every text entry field must also have a working software keyboard fallback.

## Required first-boot sequence

The default first-boot flow should be short and safe:

1. Welcome / language / accessibility entry
2. Keyboard verification
3. Wi-Fi selection and password entry
4. Privacy and telemetry posture
5. Theme choice
6. Time / date / network confirmation when needed
7. Restore / import choices
8. Account/provider choices
9. Lock method creation or skip where policy allows
10. Ready screen with Start/Home explanation

```text
WELCOME_STEP=REQUIRED
ACCESSIBILITY_ENTRY=REQUIRED
KEYBOARD_VERIFICATION_STEP=REQUIRED
WIFI_PASSWORD_TEXT_ENTRY=REQUIRED
PRIVACY_POSTURE_STEP=REQUIRED
THEME_CHOICE_STEP=REQUIRED
RESTORE_IMPORT_STEP=REQUIRED
ACCOUNT_PROVIDER_STEP=REQUIRED
LOCK_METHOD_STEP=REQUIRED
READY_TO_START_STEP=REQUIRED
```

## Keyboard verification

Titan 2 setup must include an explicit keyboard check before the user reaches password-heavy flows.

Required verification:

```text
LETTER_ENTRY=PASS_REQUIRED
NUMBER_ENTRY=PASS_REQUIRED
BACKSPACE=PASS_REQUIRED
ENTER_NEXT_OR_SUBMIT=PASS_REQUIRED
ALT_ENTRY=PASS_REQUIRED
SYM_ENTRY=PASS_REQUIRED
FN_ENTRY=PASS_REQUIRED
SHIFT_OR_CAPS_BEHAVIOR=PASS_REQUIRED
SOFTWARE_KEYBOARD_FALLBACK_VISIBLE=PASS_REQUIRED
```

The test must not be a hidden diagnostic. It is part of first-boot confidence. If the physical keyboard fails, the wizard must show a recovery path using software keyboard and accessibility options.

## Critical text-entry gates

Setup Wizard is a release-blocking critical text-entry gate for Titan 2.

```text
TITAN2_SETUP_WIZARD_TEXT_ENTRY=REQUIRED
TITAN2_WIFI_PASSWORD_ENTRY=REQUIRED
TITAN2_ACCOUNT_SIGN_IN_ENTRY=REQUIRED_IF_PROVIDER_ENABLED
TITAN2_LOCK_METHOD_TEXT_ENTRY=REQUIRED
TITAN2_ALT_ENTRY_SETUP_DIALOG=REQUIRED
TITAN2_SYM_ENTRY_SETUP_DIALOG=REQUIRED
TITAN2_SOFTWARE_KEYBOARD_FALLBACK_SETUP=REQUIRED
```

A user-facing Titan 2 image must not be accepted if Setup Wizard can trap the user in a text field, password field, account flow, Wi-Fi dialog, or lock-method dialog.

## Wi-Fi setup

Wi-Fi setup must work with hardware keyboard and software keyboard fallback.

```text
WIFI_NETWORK_LIST_KEYBOARD_NAVIGATION=YES
WIFI_PASSWORD_ENTRY=YES
SHOW_HIDE_PASSWORD_TOGGLE_KEYBOARD_ACCESSIBLE=YES
ADVANCED_NETWORK_FIELDS_KEYBOARD_ACCESSIBLE=YES
OFFLINE_SETUP_PATH_VISIBLE=YES
```

Offline setup may be allowed where system policy allows it, but it must not hide the consequences. If network is required for a provider/account step, that dependency must be shown before the user enters a blocked flow.

## Privacy posture

Sable first boot should make privacy posture explicit without requiring the user to read developer documentation.

Required items:

```text
LOCAL_FIRST_POSTURE_VISIBLE=YES
TELEMETRY_DEFAULT_OFF_OR_EXPLICIT=YES
PROVIDER_OWNERSHIP_EXPLAINED=YES
HUB_PROVIDER_BOUNDARY_EXPLAINED=YES
RESTORE_IMPORT_BOUNDARY_EXPLAINED=YES
LOCKSCREEN_NOTIFICATION_PRIVACY_VISIBLE=YES
```

Provider accounts, mail, messaging, contacts, calendar and restore flows remain provider-owned. Setup Wizard must not imply that Sable owns provider credentials or provider databases.

## Theme and appearance

Theme choice at first boot must become the same system theme state used by Quick Settings and followed by Sable apps.

```text
THEME_CHOICE_STEP=YES
DARK_THEME_AVAILABLE=YES
SYSTEM_THEME_SOURCE_OF_TRUTH=YES
SABLE_START_FOLLOWS_SYSTEM_THEME=YES
SABLE_SETTINGS_FOLLOWS_SYSTEM_THEME=YES
SABLE_HUB_FOLLOWS_SYSTEM_THEME=YES
SABLE_ALL_APPS_FOLLOWS_SYSTEM_THEME=YES
SABLE_COMMAND_FOLLOWS_SYSTEM_THEME=YES
```

This preserves the existing decision that Sable apps must follow system dark/light theme propagation.

## Restore and import

Restore/import must be fail-closed on Titan 2 until the implementation is validated.

```text
RESTORE_IMPORT_VISIBLE=YES
RESTORE_IMPORT_FAIL_CLOSED=YES
UNSUPPORTED_RESTORE_ACTIONS_DISABLED=YES
RESTORE_DOES_NOT_BYPASS_PRIVACY_POSTURE=YES
NO_PROVIDER_CREDENTIAL_OWNERSHIP_BY_SABLE=YES
```

If restore is not ready, the screen should say so and continue with a clean setup path rather than pretending restore exists.

## Account and provider choices

Setup may offer provider choices only when supported and safe.

```text
ACCOUNT_PROVIDER_STEP=YES
PROVIDER_SAFE_SIGN_IN_ONLY=YES
SKIP_ACCOUNT_VISIBLE_WHERE_ALLOWED=YES
NO_FAKE_ACCOUNT_PROVIDER=YES
NO_UNSUPPORTED_INLINE_AUTH=YES
```

A provider setup screen must have working physical-keyboard entry, Alt/Sym/Fn entry and software keyboard fallback before it is accepted.

## Lock method

Lock method setup must share the Lockscreen / Secure Entry rules.

```text
PIN_ENTRY=REQUIRED
PASSWORD_ENTRY=REQUIRED_WHERE_ALLOWED
CONFIRM_ENTRY=REQUIRED
MISMATCH_FEEDBACK=REQUIRED
EMERGENCY_PATH_NOT_BLOCKED=REQUIRED
NO_UNLOCK_BYPASS=REQUIRED
```

## Accessibility and fallback

Accessibility entry must be reachable from the first screen by keyboard and touch.

```text
ACCESSIBILITY_ENTRY_VISIBLE=YES
SCREEN_READER_PATH_VISIBLE=YES
TEXT_SIZE_PATH_VISIBLE=YES
HIGH_CONTRAST_PATH_VISIBLE=YES
SOFTWARE_KEYBOARD_FALLBACK_VISIBLE=YES
NO_FOCUS_TRAPS=YES
```

## Ready screen

The final first-boot screen should briefly explain Titan 2 controls:

```text
HOME_GOES_TO_SABLE_START=YES
TYPE_TO_SEARCH_OR_LAUNCH=YES
COMMAND_AVAILABLE=YES
LEFT_GLANCE_RIGHT_HUB_MODEL_VISIBLE=YES
ALL_APPS_ACCESS_VISIBLE=YES
QUICK_SETTINGS_ACCESS_VISIBLE=YES
```

## Visual and validation requirements

Every Setup Wizard visual artifact must use the Titan 2 hardware template when targeting Titan 2.

```text
TITAN2_HARDWARE_TEMPLATE=REQUIRED
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=REQUIRED
NO_SLAB_PHONE_VISUAL_TARGET=REQUIRED
NO_STRETCHED_SCREEN_TARGET=REQUIRED
```

## Acceptance checklist

```text
SETUP_WIZARD_FIRST_BOOT_UX=PASS
SETUP_WIZARD_VISUAL_ARTIFACT_SOURCE_CONTROLLED=PASS
TITAN2_SETUP_VISUAL_TARGET=PASS
ANDROID_SETUP_MENTAL_MODEL=PASS
KEYBOARD_FIRST_ONBOARDING=PASS
PHYSICAL_KEYBOARD_VERIFICATION=PASS
CRITICAL_TEXT_ENTRY_GATE=PASS
WIFI_PASSWORD_ENTRY=PASS
ALT_SYM_FN_TEXT_ENTRY=PASS
SOFTWARE_KEYBOARD_FALLBACK=PASS
DARK_THEME_SELECTION=PASS
SABLE_APPS_FOLLOW_SYSTEM_THEME=PASS
PRIVACY_POSTURE_EXPLICIT=PASS
RESTORE_IMPORT_FAIL_CLOSED=PASS
ACCOUNT_PROVIDER_SAFE=PASS
LOCK_METHOD_SETUP=PASS
ACCESSIBILITY_ENTRY_VISIBLE=PASS
NO_FOCUS_TRAPS=PASS
NO_SETUP_DEAD_ENDS=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
