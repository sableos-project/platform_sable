# Sable Phone / Dialer visual confirmation

Status: **visual confirmation checklist — Titan 2 target**

The Phone / Dialer visual target is source-controlled at:

```text
docs/design/artifacts/sable-phone-dialer-titan2.svg
```

The artifact must be reviewed against the active Titan 2 visual-template rule:

```text
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
```

## Visual panels

The source-controlled artifact should demonstrate these panels:

1. **Recents / missed calls**
   - square Titan 2 display;
   - focused missed-call row;
   - callback and details affordances;
   - Hub handoff indication;
   - privacy-aware row text.

2. **Physical keyboard dialing**
   - number field visible;
   - focus state visible;
   - hardware key mapping hint;
   - `0-9`, `*`, `#`, `+`, Backspace and Enter behavior represented.

3. **Onscreen dialpad**
   - full dialpad visible without depending on physical keys;
   - `0-9`, `*`, `#`, delete, call and plus entry represented;
   - large touch targets for square display.

4. **Contact search / handoff**
   - typed search / T9 contact match;
   - open contact provider action;
   - provider-safe call/message action.

5. **In-call controls**
   - active call surface;
   - mute, speaker/audio, keypad, hold/add if supported, end call;
   - DTMF only when keypad mode is active.

6. **Emergency and locked/private states**
   - emergency path preserved;
   - locked/private redaction;
   - no private contact leakage while locked.

## Required confirmation statements

```text
SABLE_PHONE_DIALER_UX=PASS
PHONE_VISUAL_ARTIFACT_SOURCE_CONTROLLED=PASS
TITAN2_PHONE_VISUAL_TARGET=PASS
ANDROID_PHONE_MENTAL_MODEL=PASS
PHYSICAL_KEYBOARD_DIALING=PASS
ONSCREEN_DIALPAD=PASS
SOFTWARE_KEYBOARD_FALLBACK=PASS
TOUCH_DIALPAD_TARGETS=PASS
PHYSICAL_AND_ONSCREEN_INPUT_SHARE_STATE=PASS
STAR_POUND_PLUS_ENTRY=PASS
DTMF_IN_CALL_KEYPAD=PASS
RECENTS_PRIVACY=PASS
MISSED_CALL_HANDOFF=PASS
CONTACTS_HANDOFF=PASS
HUB_CALL_HANDOFF=PASS
NOTIFICATION_SHADE_CALL_HANDOFF=PASS
EMERGENCY_CALL_PATH_PRESERVED=PASS
LOCKED_MODE_REDACTION=PASS
PRIVATE_MODE_REDACTION=PASS
NO_FOCUS_TRAPS=PASS
NO_CALL_DEAD_ENDS=PASS
NO_CUSTOM_TELEPHONY_STACK=PASS
NO_CUSTOM_IMS_STACK=PASS
NO_CUSTOM_EMERGENCY_STACK=PASS
TITAN2_HARDWARE_TEMPLATE=PASS
SQUARE_DISPLAY_PLUS_PHYSICAL_KEYBOARD=PASS
NO_SLAB_PHONE_VISUAL_TARGET=PASS
NO_STRETCHED_SCREEN_TARGET=PASS
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
```

## Physical keyboard review

Confirm:

- digits can be entered using hardware keys;
- `*`, `#` and `+` have documented paths;
- Backspace deletes one digit;
- Enter does not accidentally call unless a valid call target/action is active;
- arrows move focus without traps;
- Back/Esc exits transient panels;
- dedicated call/end keys are used only if present and supported by the device profile;
- in-call digit entry becomes DTMF only when keypad mode is active.

## Onscreen review

Confirm:

- Dialpad can be used entirely by touch;
- software keyboard appears for text/contact search where needed;
- onscreen dialpad and physical keyboard update the same number/search field;
- emergency number entry remains possible when physical input fails;
- touch controls fit the square display without appearing as a stretched slab-phone layout.

## Privacy review

Confirm:

- contact names/photos/numbers follow lockscreen/private-mode policy;
- missed-call details do not leak beyond platform policy;
- Hub and Notification Shade show only provider/platform-allowed actions;
- no reply/call/log mutation is invented by Sable when unsupported.
