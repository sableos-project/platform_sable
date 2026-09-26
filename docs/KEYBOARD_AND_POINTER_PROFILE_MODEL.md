# Keyboard and pointer profile model

Status: **architecture contract — keyboard families are independent**

SableOS must treat physical keyboards and keyboard-adjacent pointer surfaces as device-profiled hardware. Titan 2, Titan 2 Elite and Zinwa Q27 may all be keyboard-first devices, but their keyboard geometry, scan codes, modifier behavior, touch surfaces and mouse modes are not interchangeable.

## Core rule

```text
KEYBOARD_FIRST_COMMON_UI=YES
ONE_HARDCODED_KEYBOARD_PROFILE=NO
PER_DEVICE_KEYBOARD_PROFILE=REQUIRED
PER_DEVICE_POINTER_PROFILE=REQUIRED_WHEN_SURFACE_EXISTS
```

## Keyboard profile fields

```text
keyboard_profile:
  device_family
  input_device_names
  bus_vendor_product_ids
  scan_code_map
  keylayout_files
  key_character_maps
  printed_legend_map
  android_keycode_map
  modifier_model
  symbol_model
  numeric_entry_model
  shortcut_model
  wake_model
  repeat_model
  lockscreen_model
  setup_model
```

Do not infer Android behavior from keycap legends. Profile facts must come from observed Linux input, Android input, keylayout/kcm files and user-facing behavior.

## Modifier model

Each device must define:

```text
Shift:
  held
  one_shot
  lock
  clear_on_space

Alt:
  held
  one_shot
  lock
  clear_on_space
  symbol_dependency

Sym/Fn:
  held
  timeout
  page
  one_shot
  lock
  produces_software_surface

Ctrl:
  held
  one_shot
  lock
  nav_mode_trigger
```

The model must explicitly cover text fields and non-text contexts.

## Pointer/touch-surface profile

Some keyboard devices expose touch or mouse capability through a touchpad, keyboard surface, side surface or vendor mouse mode. SableOS must model this separately from the key matrix.

```text
pointer_profile:
  surface_present
  surface_location
  input_device_name
  relative_pointer
  absolute_touch
  multi_touch
  scroll_x
  scroll_y
  tap
  click
  long_press
  gesture_zones
  mouse_mode
  cursor_visibility_policy
  text_cursor_policy
  app_compatibility_notes
```

Surface location values:

```text
separate_trackpad
keyboard_matrix_surface
side_touch_surface
toolbelt_surface
unknown
none
```

Titan 2 evidence starts with a separate keyboard touchpad/mouse path. Titan 2 Elite and Q27 require independent capture.

## Common semantic actions

Device adapters map raw keys to Sable semantic actions. Apps consume actions rather than scan codes.

```text
SABLE_ACTION_COMMAND
SABLE_ACTION_HUB
SABLE_ACTION_SEARCH
SABLE_ACTION_BACK
SABLE_ACTION_HOME
SABLE_ACTION_RECENTS
SABLE_ACTION_CONTEXT_MENU
SABLE_ACTION_COMPOSE
SABLE_ACTION_PEEK
SABLE_ACTION_FOCUS_NEXT
SABLE_ACTION_FOCUS_PREVIOUS
SABLE_ACTION_SCROLL_LINE_UP
SABLE_ACTION_SCROLL_LINE_DOWN
SABLE_ACTION_SCROLL_PAGE_UP
SABLE_ACTION_SCROLL_PAGE_DOWN
SABLE_ACTION_CAMERA_SHUTTER
SABLE_ACTION_MEDIA_PLAY_PAUSE
SABLE_ACTION_INPUT_METHOD_SWITCH
SABLE_ACTION_SOFTWARE_KEYBOARD_TOGGLE
```

A missing or ambiguous action must not be guessed from another device family.

## Per-app input policy

SableOS may expose profile-driven app behavior:

```text
enter_to_send
enter_to_newline
auto_focus_first_editable
force_nav_mode
block_keyboard_for_game
keyboard_mouse_mode
camera_shutter_binding
call_screen_shortcuts
media_shortcuts
software_keyboard_preferred
```

Per-app behavior must be transparent to users and reversible. It must not be hidden root/Accessibility magic.

## Reference intake

Pastiera/Plektra inform modifier state, layout schemas, Nav Mode, SYM pages and regression tests.

q25toolbox informs user stories for per-app keyboard behavior, auto-focus, in-call shortcuts and device workarounds, but its root/LSPosed/bind-mount model is not Sable's base architecture.

Commander and OpenMiniLaunch/Mink inform command/hub keyboard workflows but do not define hardware policy.

## Acceptance gates

For each keyboard-first device:

```text
SCAN_CODE_MAP_CAPTURED=YES
KEYLAYOUT_KCM_CAPTURED=YES
MODIFIER_MODEL_DEFINED=YES
SYMBOL_ENTRY_DEFINED=YES
NUMERIC_ENTRY_DEFINED=YES
POINTER_SURFACE_DEFINED=YES_OR_NOT_PRESENT
LOCKSCREEN_KEYBOARD_DEFINED=YES
SETUP_KEYBOARD_DEFINED=YES
SEMANTIC_ACTION_MAP_DEFINED=YES
```

Titan 2, Titan 2 Elite and Q27 each require independent PASS evidence.
