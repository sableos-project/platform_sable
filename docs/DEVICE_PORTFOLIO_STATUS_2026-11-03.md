# SableOS device portfolio — current decision (2026-11-03)

**Status: FINAL SCOPE DECISION / SUPERSEDES EARLIER TITAN ACTIVE ROADMAPS**

## Decision

Effective 2026-11-03, **all SableOS work to create or deliver GSIs or custom ROMs for the Unihertz Titan 2 and Unihertz Titan 2 Elite is closed until further notice**. This includes device-specific Android image development, OEM IMS/VoLTE integration, flashing/qualification campaigns, new port or release claims, and Titan-specific implementation tickets. This is an explicit product-priority decision, not a claim that a complete technical root cause was proven.

**Active SableOS device focus is now Zinwa Q25 and Zinwa Q27.** Work on reusable, device-agnostic keyboard-first applications, security architecture, build tooling and platform components can continue to the extent it supports Q25/Q27. Prior Pixel 7 / Panther reference evidence is preserved; no new Panther release is implied. Being a current focus does **not** assert that a Q25 or Q27 image has passed device testing or is release-ready.

## Titan 2 closing physical result (November 1, 2026)

The last recorded Titan 2 experimental Android 16 `system.img` was 3,584,798,720 bytes, SHA-256 `f1a9869ecd1bbbd9237651cfb86ac292629daaaff7e102a5ff9aea6b54663d1a`. The `system_a` flash completed (14/14 sparse chunks, approximately 86 seconds) with matching postwrite logical size and slot A unchanged, **but usable Android boot did not complete**. Initial reboot entered Android Recovery. After an operator-authorized factory data reset the RestlessOS animation moved continuously without reaching setup or home; ADB remained unauthorized. The same animation continued without SIM cards. **The cause is unproven; neither IMS crashes nor userdata corruption was established as the cause.**

The operator then restored matched Unihertz stock images and performed a factory data reset; **stock Android boot was reported successful**. The full final per-partition transport transcript and final runtime fingerprint were not captured in the chat. The earlier historical N1C recovery showed that matched stock `super` plus boot/AVB components, not a stock `system_a`-only flash, were required for reliable restoration.

Full operator-grounded engineering closure and evidence pointers are retained at [aimindseye/sableos PR #347](https://github.com/aimindseye/sableos/pull/347), closed without merge.

## Titan 2 Elite

Titan 2 Elite was a separate, research-only candidate. **No Elite GSI/custom-ROM qualification or device-flash success is claimed.** Its porting effort is likewise closed until further notice. Do not transfer Titan 2 firmware, partition or boot assumptions to the Elite.

## Repository governance and preserved history

- The 2026-11-03 decision overrides previous README/roadmap statements labeling Titan 2 `ACTIVE`, `NEXT`, `N1D/C3B`, `N0`, `PORTABILITY_TARGET`, or similar. Such text remains dated historical context, **not instructions to restart work**.
- Close Titan 2 / Titan 2 Elite-specific open issues and PRs as **not planned**, without merging, rewriting evidence, deleting branches or removing existing research.
- **Do not** close unrelated device-independent architecture, common applications, security, CI, Panther history, or Q25/Q27 tasks just because they cite Titan 2 as a former reference.
- No additional Titan 2/Elite GSI or custom-ROM build, flash, runtime gate, issue or PR should be initiated without a subsequent explicit project-owner decision reversing this suspension.
- Stock firmware research, physical restoration findings, and historical failure reports remain available strictly as reference material.
- The active workqueue and future planning should identify **Zinwa Q25** and **Zinwa Q27** as the only current device-development priorities. Their status remains subject to independent evidence; this document is not build/flash authorization.

```text
DECISION_DATE=2026-11-03
TITAN2_GSI_CUSTOM_ROM=STOPPED_UNTIL_FURTHER_NOTICE
TITAN2_ELITE_GSI_CUSTOM_ROM=STOPPED_UNTIL_FURTHER_NOTICE
TITAN2_STOCK_ANDROID_RECOVERED=YES_OPERATOR_REPORTED
TITAN2_BOOT_FAILURE_ROOT_CAUSE=UNKNOWN
ACTIVE_DEVICE_FOCUS=ZINWA_Q25,ZINWA_Q27
PIXEL_PANTHER=FROZEN_HISTORICAL_REFERENCE
TITAN2_TITAN2_ELITE_SOURCE_HISTORY=PRESERVE
REOPEN_REQUIRES=EXPLICIT_NEW_OWNER_DECISION
```
