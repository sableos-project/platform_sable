# Keyboard-device Sable Tools

Status: **common architecture direction for Titan 2 / future Q27; not an R8 Panther build blocker**.

## Product rule

Physical-keyboard SableOS devices should provide a compact Sable-native device toolbox with keyboard-first navigation, read-only diagnostics by default and bounded maintenance actions routed through the owning Android/Sable service.

Common semantic sections:

```text
DEVICE
KEYBOARD
RADIO_CONNECTIVITY
STORAGE
APPS_PROCESSES
LOGS_REPORT
FILE_TRANSFER
RECOVERY_MAINTENANCE
```

Device repositories may provide adapters for hardware-specific keyboard, radio, display, partition or firmware evidence. They must not fork the common Tools application merely because hardware differs.

## External reference

Behavior/tooling reference:

```text
repository: bb10root/bb10tools
revision: 68d2e42a471b46b53ff5567edac4293ef0f3d5d6
license: GPL-3.0
```

Useful lessons include on-device radio/device inspection, compact service utilities, report/file workflows and low-level observability.

The repository also contains BB10/QNX kernel/process patching, NVRAM/MMC mutation, physical-memory work, trust/security bypass experiments and unauthenticated transfer tooling. These are not Sable production architecture.

Direct code reuse requires separate licensing/provenance review. Default policy is behavioral/reference use.

## Security boundary

```text
READ_ONLY_BY_DEFAULT=YES
NO_NEW_SHARED_UID=YES
NO_AMBIENT_ROOT_DAEMON=YES
NO_RAW_DEVICE_NODE_ACCESS_FROM_UI=YES
NO_ACCESSIBILITY_AUTOMATION_FOR_PLATFORM_OWNED_ACTIONS=YES
```

Do not expose generic arbitrary memory, NVRAM, raw-storage or kernel mutation from the production Tools UI.

## Keyboard-first interaction

Titan 2 and Q27 Tools presentation must support deterministic visible focus, type-to-filter/search, Enter activation, Back/Escape, keyboard-only report/export flows and touch as a secondary input path.

## Relationship to existing surfaces

- Settings owns configuration/policy.
- Application Security & Privacy owns per-app security evidence/control routes.
- Sable Tools owns device diagnostics, input inspection, reports and bounded utility workflows.

Reuse/deep-link rather than duplicating authority.

## Portability

Titan 2 is the first near-term qualification target. Q27 should consume the same common semantics if it becomes an active supported development target.