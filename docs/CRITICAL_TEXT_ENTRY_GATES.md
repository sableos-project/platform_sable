# Critical text-entry gates

Status: **architecture contract — launch blocker for keyboard-first devices**

Keyboard-first SableOS devices must never strand the user in setup, pairing, lockscreen, recovery or system dialogs because hardware modifiers fail and the software keyboard is unavailable.

Titan 2 stock research exposed this failure mode during Bluetooth keyboard pairing: the pairing flow required a key/passcode, regular Alt/SYM behavior was unavailable, and the stock software keyboard path was not usable as fallback. SableOS must treat this class of failure as a launch blocker.

## Critical flows

```text
Bluetooth pairing
Wi-Fi password entry
Setup Wizard
Lockscreen PIN/password
Account sign-in
Emergency text entry
Recovery/restore prompts
Input-method switching
ADB/USB authorization naming prompts when applicable
```

## Required behavior

```text
PAIRING_SAFE_TEXT_ENTRY=REQUIRED
HARDWARE_KEYBOARD_TEXT_ENTRY=REQUIRED
SOFTWARE_KEYBOARD_FALLBACK=REQUIRED
ALT_SYM_FALLBACK=REQUIRED
SETUP_WIZARD_KEYBOARD_SAFE=REQUIRED
BLUETOOTH_PAIRING_KEYBOARD_SAFE=REQUIRED
LOCKSCREEN_KEYBOARD_SAFE=REQUIRED
```

A third-party IME, accessibility service, root module or user-installed keyboard app must not be required to complete these flows.

## Fallback hierarchy

Critical fields must have at least two usable input paths:

```text
1. device physical keyboard with letters/numbers/symbols
2. Sable/software keyboard fallback
3. touch-accessible input-method switch or software keyboard toggle
```

If hardware keyboard presence causes Android to suppress the software keyboard, SableOS must provide an explicit override for critical flows.

## Modifier and symbol requirements

Each device must prove:

```text
letters
numbers
space
backspace/delete
enter/confirm
escape/back/cancel
shift upper-case
alt symbols
sym/fn symbols
punctuation required for pairing and passwords
input-method/software-keyboard toggle
```

The test must run in system dialogs, not only in normal apps.

## Per-device gates

Titan 2:

```text
TITAN2_BT_PAIRING_TEXT_ENTRY=PASS
TITAN2_ALT_ENTRY_SYSTEM_DIALOG=PASS
TITAN2_SYM_ENTRY_SYSTEM_DIALOG=PASS
TITAN2_SOFTWARE_KEYBOARD_FALLBACK=PASS
TITAN2_SETUP_WIZARD_TEXT_ENTRY=PASS
TITAN2_LOCKSCREEN_TEXT_ENTRY=PASS
```

Titan 2 Elite:

```text
TITAN2_ELITE_BT_PAIRING_TEXT_ENTRY=PASS
TITAN2_ELITE_ALT_ENTRY_SYSTEM_DIALOG=PASS
TITAN2_ELITE_SYM_ENTRY_SYSTEM_DIALOG=PASS
TITAN2_ELITE_SOFTWARE_KEYBOARD_FALLBACK=PASS
TITAN2_ELITE_SETUP_WIZARD_TEXT_ENTRY=PASS
TITAN2_ELITE_LOCKSCREEN_TEXT_ENTRY=PASS
```

Zinwa Q27:

```text
Q27_BT_PAIRING_TEXT_ENTRY=PASS
Q27_ALT_ENTRY_SYSTEM_DIALOG=PASS
Q27_SYM_ENTRY_SYSTEM_DIALOG=PASS
Q27_SOFTWARE_KEYBOARD_FALLBACK=PASS
Q27_SETUP_WIZARD_TEXT_ENTRY=PASS
Q27_LOCKSCREEN_TEXT_ENTRY=PASS
```

No device inherits another device's PASS.

## Test method

For each target, record:

```text
stock_build_or_sable_build
locked_or_unlocked_state
active_input_method
hardware_keyboard_profile
software_keyboard_available
flow_under_test
characters_required
characters_entered
fallback_used
result
```

The result must include success and negative/failure notes, not only screenshots.

## Release rule

```text
CRITICAL_TEXT_ENTRY_GATES_PASS=REQUIRED_FOR_USER_FACING_IMAGE
CRITICAL_TEXT_ENTRY_GATES_FAIL=BLOCK_RELEASE
CRITICAL_TEXT_ENTRY_GATES_UNTESTED=BLOCK_RELEASE
```

A first engineering boot may proceed with gates open only if the release plan labels the artifact as engineering-only and does not present it as user-facing.

## Related device issue

Titan 2 public adapter tracking issue:

```text
sableos-project/device_sable_titan2#3
N0_A16: require pairing-safe keyboard fallback and per-device keyboard profiles
```
