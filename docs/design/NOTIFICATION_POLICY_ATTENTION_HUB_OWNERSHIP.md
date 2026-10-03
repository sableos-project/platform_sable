# DESIGN-KF-A — Notification Policy, Attention and Hub Ownership

Status: **accepted implementation-ready design contract**  
Date: **2026-10-03**

This contract closes ADR-0010 KF7/KF8 product design. It defines ownership,
inheritance, privacy and keyboard behavior for notification delivery, attention
outputs and Sable Hub integration.

## Core ownership

SableOS must preserve one authoritative notification-delivery model:

```text
Android NotificationManager / NotificationChannel
  owns delivery importance, sound, vibration, heads-up, badge,
  lockscreen visibility, snooze and Android-supported conversation policy

SystemUI / Settings
  presents and edits Android-owned notification policy

Sable Attention
  maps effective Android policy to profile-supported hardware attention outputs

Sable Hub
  aggregates permitted communication/notification information and provider-safe
  actions; it does not become the notification policy database
```

Required:

```text
DUPLICATE_NOTIFICATION_POLICY_STORE=NO
HUB_OWNS_ANDROID_NOTIFICATION_DELIVERY=NO
HUB_BYPASSES_DND=NO
HUB_INVENTS_PROVIDER_ACTIONS=NO
ANDROID_NOTIFICATION_STATE_SOURCE_OF_TRUTH=YES
```

## Policy hierarchy

Effective notification state follows Android ownership and inheritance:

```text
application
  -> channel
      -> conversation, where Android exposes conversation-specific policy
```

Sable UI may summarize the effective result but must not imply a control exists
when Android does not expose one.

User-facing policy is grouped into three clearly separated concepts:

```text
Delivery
  Alerting / Quiet / Off
  sound
  vibration
  pop-on-screen / heads-up where supported
  badge
  lockscreen visibility

Hub
  Include in Hub
  Hub priority
  Hub preview policy

Attention
  supported device outputs such as keyboard backlight, secondary glance display,
  haptic, status LED or AOD
```

Changing Hub inclusion must not silently change Android delivery policy.
Changing Attention output selection must not silently change notification
importance.

## Hub configuration ownership

Hub-specific state may be stored separately because it is not Android delivery
state.

Allowed Hub state:

```text
per Android user/profile + package
  include/exclude from Hub
  Hub priority
  privacy-safe preview preference
  optional bounded local history preference
```

Conversation-level Hub overrides are allowed only if the underlying Android
conversation identity is stable and source-owned.

Hub configuration must never store provider credentials or private provider
database identifiers as product truth.

## Attention capability model

Attention outputs are device-profile capabilities:

```text
OUTPUT_AUDIO
OUTPUT_HAPTIC
OUTPUT_STATUS_LED
OUTPUT_KEYBOARD_BACKLIGHT
OUTPUT_SECONDARY_DISPLAY
OUTPUT_ALWAYS_ON_DISPLAY
```

Rules:

- unsupported outputs are hidden, not shown as disabled "coming soon" controls;
- device families do not inherit PASS from each other;
- Titan 2 rear SubScreen is a curated glance/attention output, not arbitrary app
  mirroring;
- Titan 2 AOD is not exposed as a working output;
- Titan 2 Elite/Q27 AOD remains gated until independently validated;
- hardware-attention output must obey lockscreen/private/work-profile redaction;
- hardware output may indicate category/count/state but must not leak protected
  message bodies while locked.

## Default attention posture

A notification channel keeps Android defaults unless the user changes them.

Sable hardware attention is conservative by default:

```text
KEYBOARD_BACKLIGHT_NOTIFICATION_DEFAULT=OFF
SUBSCREEN_NOTIFICATION_DEFAULT=PROFILE_AND_PRIVACY_GATED
STATUS_LED_DEFAULT=PLATFORM_DEFAULT_IF_PRESENT
AOD_NOTIFICATION_DEFAULT=PLATFORM_DEFAULT_IF_SUPPORTED
```

No hardware-attention feature may flash, pulse or wake continuously without an
explicit bounded policy.

## Do Not Disturb

Android DND remains authoritative.

```text
DND_SOURCE_OF_TRUTH=ANDROID
HUB_CAN_BYPASS_DND=NO
ATTENTION_OUTPUT_CAN_BYPASS_DND=NO_UNLESS_ANDROID_POLICY_ALLOWS
SABLE_PRIORITY_IS_NOT_DND_BYPASS=YES
```

"Hub priority" is an ordering/aggregation concept, not permission to bypass DND.

## Privacy states

The effective policy must survive these states:

| State | Delivery | Hub | Attention |
| --- | --- | --- | --- |
| Unlocked | Android policy | allowed previews/actions | profile-supported |
| Locked | Android policy | redacted by default | category/count only unless policy permits more |
| Private mode | Android policy | body/sender redaction by default | non-content indicator only |
| Work profile locked | Android policy | work content hidden | generic work indicator only |
| Source restricted | Android policy | source/count/action only | generic indicator |

Reply from a locked surface is allowed only when Android/source policy explicitly
supports that action without unlocking. Otherwise authentication is required.

## Notification center ownership

The Android notification shade is the notification center. Sable does not create
a second general notification-center database or timeline.

Sable Hub may expose a Notifications filter, but that view is an aggregation
surface with its own bounded cache/history semantics and provider-safe handoff.

```text
SYSTEM_NOTIFICATION_CENTER=SYSTEMUI_SHADE
HUB_NOTIFICATIONS_FILTER=AGGREGATION_VIEW
HUB_NOTIFICATION_HISTORY=BOUNDED_DERIVED_CACHE_ONLY
SYSTEM_NOTIFICATION_HISTORY_OWNERSHIP=ANDROID
```

## Keyboard-first notification interaction

Single-letter commands are contextual to the focused shade/notification surface
and never global.

```text
Up / Down
  previous / next notification

Left / Right
  action buttons or collapsed/expanded action region

Enter
  open focused notification

Space
  expand/collapse or safe peek

R
  reply only when RemoteInput/source policy allows

D
  dismiss

Z
  snooze

M
  mute / delivery options

C
  notification/channel/conversation settings

H
  open in Hub when eligible

Back / Esc
  close transient UI or return one level
```

Text entry always wins. Once an inline-reply editor owns focus, printable keys
edit text and do not trigger single-key notification commands.

## Settings information architecture

Keep Android's familiar structure:

```text
Settings > Notifications
  App notifications
  Notification history
  Conversations, where Android exposes it
  Lock screen notifications
  Do Not Disturb
  Sable Attention
```

Per-app detail:

```text
App notification settings
  Android-owned channels / conversations
  Hub
    Include in Hub
    Hub priority
    Preview policy
  Attention
    only profile-supported outputs
```

Sable Attention must deep-link to the owning Android setting when the requested
change is Android-owned rather than maintaining a duplicate toggle.

## Hub handoff

Hub keeps the Panther V1 portability contract.

Allowed sources/actions remain public Android/provider-owned interfaces:

```text
NotificationListener
MessagingStyle / Person / conversation metadata
RemoteInput
PendingIntent
published shortcuts / public supported APIs
```

Provider-specific private protocols, credentials, database scraping and provider
WebViews remain prohibited.

Sable Messages and Hub remain separate products.

## Multi-user and work profile

Policy and Hub state are scoped by Android user/profile.

```text
PACKAGE_ONLY_KEY=FORBIDDEN
POLICY_KEY=android_user_or_profile + package + android_channel_or_conversation
HUB_KEY=android_user_or_profile + package + stable_conversation_when_available
WORK_PROFILE_BADGING=REQUIRED
CROSS_PROFILE_PREVIEW_LEAK=NO
```

## Implementation gates

Implementation must prove:

```text
NOTIFICATION_POLICY_ANDROID_OWNED=PASS
DUPLICATE_NOTIFICATION_POLICY_STORE=PASS_ABSENT
HUB_DELIVERY_POLICY_OWNERSHIP=PASS_ABSENT
HUB_CONNECTED_APPS_PARITY=PASS
HUB_REMOTEINPUT_PROVIDER_SAFE=PASS
HUB_DND_BYPASS=PASS_ABSENT
ATTENTION_CAPABILITY_GATING=PASS
ATTENTION_LOCKSCREEN_REDACTION=PASS
WORK_PROFILE_SEPARATION=PASS
SHADE_IS_NOTIFICATION_CENTER=PASS
HUB_HISTORY_BOUNDED_DERIVED=PASS
KEYBOARD_NOTIFICATION_COMMANDS_CONTEXTUAL=PASS
TEXT_INPUT_ALWAYS_WINS=PASS
```

## Non-goals

```text
CUSTOM_NOTIFICATION_TRANSPORT=NO
CUSTOM_DND_ENGINE=NO
PROVIDER_PRIVATE_DATABASE_ACCESS=NO
PROVIDER_SPECIFIC_ALLOWLIST=NO
SECOND_GENERAL_NOTIFICATION_CENTER=NO
DEVICE_MODEL_BRANCHING_IN_COMMON_UI=NO
```
