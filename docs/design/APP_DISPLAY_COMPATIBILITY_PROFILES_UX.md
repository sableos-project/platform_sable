# Sable App Display Compatibility Profiles UX

## Purpose

Define the SableOS per-app display compatibility surface for Titan-family devices, especially Titan 2 and Titan 2 Elite. This fills the gap between the existing Mini Mode / Rotation Control design and the fuller product requirement: per-app custom display profiles including 16:9, 4:3, square-safe, fit/fill scaling, keyboard-safe layout, and reversible app-specific overrides.

```text
APP_DISPLAY_COMPATIBILITY_PROFILES_UX=YES
PER_APP_DISPLAY_PROFILE=YES
PER_APP_ASPECT_RATIO=YES
CUSTOM_16_9_PROFILE=YES
CUSTOM_4_3_PROFILE=YES
CUSTOM_SQUARE_SAFE_PROFILE=YES
CUSTOM_PROFILE_ADVANCED=YES_GATED
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Source basis

Titan-family manuals describe two related compatibility features:

```text
MINI_MODE=manual_documented_display_compatibility_mode
ROTATION_CONTROL=manual_documented_per_app_orientation_control
```

SableOS treats those as the starting point, not the full design. The product feature is a first-class per-app display compatibility profile system.

```text
MINI_MODE_IS_PROFILE=YES
ROTATION_CONTROL_IS_PROFILE_DIMENSION=YES
APP_DISPLAY_COMPATIBILITY_IS_PARENT_FEATURE=YES
```

## Product model

```text
FEATURE_NAME=Sable App Display Compatibility
SETTINGS_PRIMARY_PATH=Settings > Display > App display compatibility
APP_INFO_PATH=Settings > Apps > <App> > Display compatibility
LAUNCHER_CONTEXT_PATH=Launcher > App info > Display compatibility
PROFILE_SCOPE=per_user_per_package
DEFAULT_PROFILE=Native
RESET_VISIBLE=YES
SAFE_MODE_RESET=YES
```

The feature must feel like a compatibility control panel, not a hidden developer option.

```text
USER_VISIBLE=YES
SECURITY_AND_PRIVACY_SAFE=YES
NO_VENDOR_HACK_REQUIRED_FOR_UI=YES
IMPLEMENTATION_BACKEND_GATED=YES
```

## Profiles

```text
PROFILE_NATIVE=Use app and platform defaults
PROFILE_MINI_MODE=Use Titan manual-style mini display size
PROFILE_16_9_LETTERBOX=Run app inside a 16:9 safe canvas with bars
PROFILE_16_9_FILL=Run app in a 16:9 canvas and scale/fill where safe
PROFILE_4_3_LETTERBOX=Run app inside a 4:3 safe canvas
PROFILE_SQUARE_SAFE=Optimize app for square-ish Titan-family layout
PROFILE_FULLSCREEN_MEDIA=Prefer media/fullscreen behavior and avoid forced letterboxing
PROFILE_KEYBOARD_SAFE=Reserve keyboard/text-entry safe area when needed
PROFILE_CUSTOM=Advanced custom aspect ratio and sizing, gated
```

## Per-app controls

```text
CONTROL_ENABLE_PROFILE=YES
CONTROL_ASPECT_RATIO=default,16:9,4:3,square,custom
CONTROL_ORIENTATION=default,portrait,landscape,auto
CONTROL_SCALING=default,fit,fill
CONTROL_STRETCH=NO_BY_DEFAULT
CONTROL_LETTERBOX_BACKGROUND=system,dark,light,accent
CONTROL_KEYBOARD_SAFE_AREA=default,on,off
CONTROL_STATUS_NAV_BEHAVIOR=default,show,hide
CONTROL_FULLSCREEN_MEDIA_EXCEPTION=on,off
CONTROL_RESET=YES
```

Stretching is not the default because it can distort text, maps, camera previews, games, and authentication screens.

```text
STRETCH_DEFAULT=NO
STRETCH_REQUIRES_WARNING=YES
```

## 16:9 profile

The 16:9 profile is first-class because many Android apps are designed around widescreen assumptions and can behave poorly on square-ish or keyboard-first layouts.

```text
CUSTOM_16_9_PROFILE=YES
CUSTOM_16_9_CAN_BE_PER_APP=YES
CUSTOM_16_9_SAFE_CANVAS=YES
CUSTOM_16_9_LETTERBOX=YES
CUSTOM_16_9_FILL=YES_GATED
CUSTOM_16_9_DEFAULT=letterbox
```

User-facing copy:

```text
PROFILE_COPY_16_9=Run this app in a 16:9 compatibility window. Useful for video, games, maps, camera companion apps, and apps that assume a wide screen.
WARNING_COPY_16_9_FILL=Fill can crop edges. Use letterbox if buttons or text are missing.
```

## Mini Mode integration

Mini Mode remains a recognizable Titan-family compatibility preset, but it should be modeled as one profile inside the larger system.

```text
MINI_MODE_PROFILE=YES
MINI_MODE_GLOBAL_SHORTCUT=YES
MINI_MODE_PER_APP_PROFILE=YES
MINI_MODE_CAN_BE_DEFAULT_FOR_SELECTED_APPS=YES
MINI_MODE_REVERT_VISIBLE=YES
```

## Rotation Control integration

Rotation Control should not live separately from display compatibility. It is a per-app dimension in the profile.

```text
ROTATION_CONTROL_INCLUDED=YES
PER_APP_ORIENTATION=YES
ORIENTATION_PROFILE_DIMENSION=YES
ORIENTATION_REVERT_VISIBLE=YES
```

## Keyboard-first behavior

Titan-family devices are keyboard-first. Display profiles must not break hardware keyboard workflows.

```text
KEYBOARD_SAFE_PROFILE=YES
TEXT_INPUT_ALWAYS_WINS=YES
SHORTCUTS_DISABLED_WHILE_TYPING=YES
CURSOR_ASSISTANT_COMPATIBILITY_REQUIRED=YES
SCROLL_ASSISTANT_COMPATIBILITY_REQUIRED=YES
SPACE_KEY_ACTIONS_RESPECT_TEXT_FIELDS=YES
```

## Safety rules

```text
DO_NOT_BREAK_TEXT_INPUT=YES
DO_NOT_BREAK_ACCESSIBILITY=YES
DO_NOT_BREAK_LOCKSCREEN=YES
DO_NOT_BREAK_AUTHENTICATOR_APPS=YES
DO_NOT_BREAK_CAMERA_PREVIEW=YES
DO_NOT_BREAK_FULLSCREEN_MEDIA=YES
DO_NOT_BREAK_PAYMENT_OR_WALLET_APPS=YES
DO_NOT_BREAK_WORK_PROFILE=YES
PROFILE_REVERTIBLE_WITHOUT_OPENING_APP=YES
```

If an app becomes unusable after a compatibility profile is applied, the user must be able to reset the profile from Settings without launching the app.

```text
RESET_OUTSIDE_APP=YES
SAFE_MODE_RESET_ALL_DISPLAY_PROFILES=YES
```

## Implementation gates

This design is product-level. N0 may include UI, storage, stubs, and validation hooks, but backend behavior must be proven against Android 16 WindowManager / package compatibility behavior and Titan vendor display behavior.

```text
WINDOW_MANAGER_BACKEND_VALIDATION_REQUIRED=YES
PER_APP_BOUNDS_VALIDATION_REQUIRED=YES
DENSITY_OVERRIDE_VALIDATION_REQUIRED=YES
DISPLAY_CUTOUT_VALIDATION_REQUIRED=YES
INPUT_METHOD_VALIDATION_REQUIRED=YES
NAV_BAR_STATUS_BAR_VALIDATION_REQUIRED=YES
OEM_VENDOR_COMPATIBILITY_VALIDATION_REQUIRED=YES
```

## N0 interpretation

```text
N0_CAN_INCLUDE_SETTINGS_UI=YES
N0_CAN_INCLUDE_PER_APP_PROFILE_STORAGE=YES
N0_CAN_INCLUDE_NOOP_PROFILE_STATE=YES
N0_CAN_INCLUDE_VALIDATION_HOOKS=YES
N0_CAN_ASSUME_BACKEND_WORKS=NO
N0_CAN_FORCE_ASPECT_RATIO_WITHOUT_VALIDATION=NO
N0_CAN_FLASH=NO
```

## Visual states required

```text
VISUAL_STATE_1=App display compatibility list
VISUAL_STATE_2=Per-app profile editor
VISUAL_STATE_3=16:9 profile preview and warnings
VISUAL_STATE_4=Reset and safe-mode recovery
```

## Acceptance checklist

```text
APP_DISPLAY_COMPATIBILITY_PROFILES_UX=PASS
PER_APP_DISPLAY_PROFILE=PASS
CUSTOM_16_9_PROFILE=PASS
MINI_MODE_AS_PROFILE=PASS
ROTATION_CONTROL_AS_DIMENSION=PASS
KEYBOARD_SAFE_PROFILE=PASS
RESET_OUTSIDE_APP=PASS
IMPLEMENTATION_BACKEND_GATED=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
