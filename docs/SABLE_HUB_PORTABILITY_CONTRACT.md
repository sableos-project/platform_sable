# Sable Hub portability contract

Status: **current common product contract — 2026-10-03**

Sable Hub is common product code and semantics. Panther is the physical reference;
Titan 2 and Titan 2 Elite may use a keyboard-first presentation but must not lose
the generic Connected Apps behavior.

```text
SABLE_HUB_PANTHER_V1_SEMANTICS_REQUIRED=YES
SABLE_HUB_CONNECTED_APPS=REQUIRED
SABLE_HUB_GENERIC_NOTIFICATION_ADAPTER=REQUIRED
SABLE_HUB_REMOTEINPUT_REPLY=REQUIRED
SABLE_HUB_OPEN_APP_FALLBACK=REQUIRED
SABLE_HUB_PACKAGE_USER_POLICY=REQUIRED
SABLE_HUB_LOCAL_BOUNDED_HISTORY=REQUIRED
SABLE_HUB_DYNAMIC_PROVIDER_DISCOVERY=REQUIRED
SABLE_HUB_PROVIDER_SPECIFIC_ALLOWLIST=NO
SABLE_HUB_PRIVATE_PROVIDER_PROTOCOLS=NO
SABLE_HUB_PROVIDER_CREDENTIALS=NO
SABLE_HUB_PRIVATE_DATABASE_SCRAPING=NO
SABLE_HUB_PROVIDER_WEBVIEWS=NO
SABLE_MESSAGES_AND_SABLE_HUB_ARE_SEPARATE_PRODUCT_SURFACES=YES
D4_MESSAGES_REPLACES_SABLE_HUB=NO
D4_MESSAGES_WEAKENS_CONNECTED_APPS=NO
```

The provider adapter is keyed by package plus Android user/profile and consumes
standard Android notification/conversation metadata and source-authored actions.
Provider names are compatibility/evidence targets, not a source allowlist.

Panther evidence levels must be carried accurately:

- Signal: notification ingestion, conversation rendering and source-authorized
  RemoteInput reply physically proven.
- WhatsApp: notification ingestion/rendering physically proven; generic
  reply-handle retention required remediation/final qualification.
- LinkedIn: notification ingestion plus Open-app fallback physically proven;
  quick reply depends on what the source app exposes.
- Telegram: installation proven; account/runtime conversation qualification was
  not completed in the Panther test environment.

Sable Messages and Sable Hub are distinct surfaces. Messages integration must not
replace, subsume, disable or weaken Sable Hub Connected Apps semantics.

Device profiles may change layout, focus and keyboard shortcuts, but they may not
create provider-specific forks or remove common Hub behavior.
