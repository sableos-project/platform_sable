# Lockscreen / Secure Entry UX

Status: **design contract — keyboard-first secure entry**

This document defines the Sable lockscreen and secure-entry model for Titan 2 and
other keyboard-first devices. It is a UX and validation contract only.

```text
LOCKSCREEN_SECURE_ENTRY_UX=YES
ANDROID_LOCKSCREEN_MENTAL_MODEL=KEEP
KEYBOARD_FIRST_UNLOCK=YES
CRITICAL_TEXT_ENTRY_GATE=YES
PHYSICAL_KEYBOARD_PIN_PASSWORD_ENTRY=YES
ALT_SYM_FN_TEXT_ENTRY=YES
PRIVACY_REDACTION_BY_DEFAULT=YES
HUB_PRIVATE_MODE_COMPATIBLE=YES
EMERGENCY_CALL_PATH_REQUIRED=YES
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Goals

The lockscreen must remain familiar to Android users while making Titan 2's
physical keyboard safe and useful for unlock, emergency, notification and
private-mode flows.

Required goals:

```text
CLOCK_AND_STATUS_VISIBLE=YES
LOCK_STATE_OBVIOUS=YES
NOTIFICATION_PRIVACY_REDACTION=YES
SECURE_UNLOCK_PROMINENT=YES
PHYSICAL_KEYBOARD_UNLOCK=YES
SOFTWARE_KEYBOARD_FALLBACK=YES
EMERGENCY_CALL_ALWAYS_REACHABLE=YES
FAILED_UNLOCK_FEEDBACK=YES
NO_FOCUS_TRAPS=YES
```

Non-goals:

```text
NO_LOCKSCREEN_BASE_QUICK_BAR
NO_UNLOCK_BYPASS
NO_SECRET_SHORTCUTS_AROUND_KEYGUARD
NO_FULL_MESSAGE_BODY_BY_DEFAULT_WHEN_LOCKED
NO_DEVICE_FLASH_ENABLEMENT
```

## Screen states

### 1. Ambient / locked glance

The locked glance state shows time, date, battery, network state and privacy-safe
notification counts.

```text
STATE=LOCKED_GLANCE
TIME_VISIBLE=YES
DATE_VISIBLE=YES
BATTERY_VISIBLE=YES
NETWORK_VISIBLE=YES
NOTIFICATION_COUNTS_SAFE=YES
MESSAGE_BODIES_VISIBLE=NO_BY_DEFAULT
HUB_SUMMARY=COUNTS_ONLY
```

Recommended content:

```text
10:46
Friday, Sep 26
Locked
2 Messages · 1 Mail · 1 Missed call
Enter unlock · E Emergency · / Search disabled while locked
```

### 2. Unlock prompt

Unlock prompt supports PIN, password and pattern-equivalent fallback where
platform policy allows. Titan 2 must pass physical-keyboard and software-keyboard
entry gates.

```text
STATE=SECURE_ENTRY
PIN_ENTRY=YES
PASSWORD_ENTRY=YES
PHYSICAL_KEYBOARD_ENTRY=YES
SOFTWARE_KEYBOARD_FALLBACK=YES
ALT_SYM_FN_ENTRY=YES
VISIBLE_CURSOR_OR_DOTS=YES
ENTER_SUBMIT=YES
BACK_CANCEL_OR_DELETE_POLICY=PLATFORM_COMPATIBLE
```

Physical keyboard requirements:

```text
Digits 0-9             enter PIN digits
Letters                enter password letters when password mode active
Alt / Sym / Fn         must enter alternate and symbol characters when needed
Backspace              delete one character where allowed
Enter                  submit unlock attempt
Esc / Back             dismiss secure-entry prompt or return to locked glance
```

### 3. Notification preview while locked

Notifications remain visible only within privacy policy.

```text
LOCKED_NOTIFICATION_MODE=REDACTED_BY_DEFAULT
PRIVATE_MODE=COUNTS_ONLY
SENSITIVE_APP_CONTENT=HIDDEN_OR_REDACTED
HUB_PROVIDER_DETAILS=PROVIDER_POLICY_BOUND
INLINE_REPLY_WHILE_LOCKED=NO_BY_DEFAULT
AUTH_REQUIRED_FOR_PRIVATE_ACTIONS=YES
```

Permitted locked actions:

```text
Wake / focus notification row
Open requires unlock
Dismiss if platform/user policy allows
Expand redacted group summary where safe
Emergency call path remains available
```

### 4. Emergency and call path

Emergency call must be reachable with keyboard and touch.

```text
EMERGENCY_CALL_PATH_REQUIRED=YES
KEYBOARD_SHORTCUT_TO_EMERGENCY=E_WHEN_LOCKED_GLANCE_FOCUSED
TOUCH_EMERGENCY_ACTION=YES
ACCIDENTAL_EMERGENCY_GUARD=YES
NORMAL_DIALER_ACCESS_REQUIRES_UNLOCK=YES
```

Emergency should not expose non-emergency contacts, messages, mail or app data.

### 5. Failed unlock and lockout

Failed unlock feedback must be clear without leaking secrets.

```text
FAILED_ATTEMPT_FEEDBACK=YES
LOCKOUT_FEEDBACK=YES
COUNTDOWN_WHEN_PLATFORM_REQUIRES=YES
NO_PASSWORD_ECHO=YES
NO_RECENT_INPUT_LEAK=YES
ACCESSIBILITY_ANNOUNCEMENT_SAFE=YES
```

## Keyboard navigation

```text
Enter                  show secure-entry prompt / submit entry
Digits                 enter PIN when prompt is active
Letters                enter password when prompt is active
Backspace              delete active field character
Back                   dismiss prompt or return to locked glance
E                      focus emergency call from locked glance
Up / Down              move between notification rows or actions
Left / Right           move within action strip where visible
Space                  safe preview / focus action, not unlock bypass
Menu / Fn+Enter        row actions when allowed by locked policy
/                      disabled or opens unlock-first search prompt
Home                   no unlock; remains at lockscreen until authenticated
```

No keyboard path may unlock, open private content, launch apps or send replies
without successful authentication unless Android/platform policy explicitly
allows that action while locked.

## Touch and pointer model

Touch remains a secondary path. Mouse/pointer support must be deterministic on
Titan 2, but pointer clicks must follow the same lock policy as keyboard actions.

```text
TOUCH_UNLOCK_PROMPT=YES
TOUCH_EMERGENCY=YES
POINTER_FOCUS_VISIBLE=YES
POINTER_CLICK_POLICY_MATCHES_KEYBOARD=YES
NO_POINTER_ONLY_UNLOCK_BYPASS=YES
```

## Critical text-entry gates

Lockscreen is a critical text-entry surface. Titan 2 cannot be treated as passed
until these are explicitly validated on hardware/build evidence.

```text
TITAN2_LOCKSCREEN_PIN_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_PASSWORD_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_ALT_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_SYM_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_FN_ENTRY=REQUIRED
TITAN2_LOCKSCREEN_BACKSPACE_ENTER=REQUIRED
TITAN2_SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
TITAN2_FAILED_UNLOCK_FEEDBACK=REQUIRED
TITAN2_EMERGENCY_CALL_REACHABILITY=REQUIRED
```

## Visual target

The visual artifact for this UX is:

```text
docs/design/artifacts/sable-lockscreen-titan2.svg
```

It must follow the Titan 2 hardware-template rule:

```text
TITAN2_HARDWARE_TEMPLATE=YES
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=YES
NO_SLAB_PHONE_VISUAL_TARGET=YES
NO_STRETCHED_SCREEN_TARGET=YES
```

## Acceptance checklist

```text
LOCKSCREEN_SECURE_ENTRY_UX=PASS
LOCKSCREEN_VISUAL_ARTIFACT_SOURCE_CONTROLLED=PASS
TITAN2_LOCKSCREEN_VISUAL_TARGET=PASS
ANDROID_LOCKSCREEN_MENTAL_MODEL=PASS
KEYBOARD_FIRST_UNLOCK=PASS
PHYSICAL_KEYBOARD_PIN_PASSWORD_ENTRY=PASS
ALT_SYM_FN_TEXT_ENTRY=PASS
SOFTWARE_KEYBOARD_FALLBACK=PASS
NOTIFICATION_PRIVACY_REDACTION=PASS
HUB_PRIVATE_MODE_COMPATIBLE=PASS
EMERGENCY_CALL_PATH_REQUIRED=PASS
FAILED_UNLOCK_FEEDBACK=PASS
NO_FOCUS_TRAPS=PASS
NO_UNLOCK_BYPASS=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```
