# Camera / Capture UX and quality opportunities

Status: design and capability contract  
Primary target: Titan 2 keyboard-first SableOS portability lane  
Scope: still capture, video capture, quick review, quality improvement opportunities, and camera-capability gating

This document defines Sable Camera as more than a minimal camera launcher. The product goal is to take full advantage of proven sensors, Camera2 streams, RAW paths, high-resolution JPEG paths, vendor-visible controls and later SableOS system privilege where those capabilities are reproducibly proven.

```text
CAMERA_CAPTURE_UX=YES
CAMERA_QUALITY_OPPORTUNITY_MATRIX=YES
CAMERA_CAPABILITY_DRIVEN=YES
DO_NOT_SKIMP_ON_SENSOR_EFFORT=YES
GCAM_LEARNINGS_REFERENCE_ONLY=YES
NORMAL_APP_CAMERA_PATH=BASELINE
PRIVILEGED_SYSTEM_CAMERA_PATH=FUTURE_SABLEOS_BACKEND
TITAN2_CAMERA_PROFILE=YES
TITAN2_ELITE_CAMERA_PROFILE=INDEPENDENT_REQUIRED
```

## Grounding

The Titan 2 camera research shows that camera capability must be treated as layered evidence, not as one broad claim.

```text
SENSOR_CAPABILITY != HAL_CAPABILITY
HAL_CAPABILITY != ORDINARY_APP_VISIBLE_CAPABILITY
ORDINARY_APP_VISIBLE_CAPABILITY != PRIVILEGED_SYSTEM_APP_CAPABILITY
TITAN2_CAPABILITY != TITAN2_ELITE_CAPABILITY
```

Known Titan 2 evidence used by this design:

```text
PUBLIC_CAMERA_0=rear main
PUBLIC_CAMERA_1=front
HIDDEN_SYSTEM_CAMERA_2=rear telephoto physical camera
HIDDEN_SYSTEM_CAMERA_3=rear logical main+tele camera
STOCK_CAMERA_USES_LOGICAL_CAMERA_3=YES
STOCK_REAR_ACTIVE_CAMERA_1X=0
STOCK_REAR_ACTIVE_CAMERA_TELE_ZOOM=2
HAL_ZOOM_STEP=3.4
REAR_HIGH_RES_JPEG_PUBLIC_PATH=8192x6144
FRONT_HIGH_RES_JPEG_PUBLIC_PATH=6560x4928
REAR_RAW_DNG_PUBLIC_PATH=4096x3072
HIDDEN_TELE_RAW_MANUAL_CAPABILITY=YES_REQUIRES_PRIVILEGED_ACCESS
```

Sable Camera must not collapse these into a single generic Android camera model. Device profiles own topology, stream maps, vendor experiments and privilege gates.

## Product position

Sable Camera should be a reusable camera application for keyboard-first and square/near-square devices, with Titan 2 as the first active target.

The first implementation path should work as a normal Camera2 app against public camera IDs. The later SableOS system path may add access to `SYSTEM_CAMERA` devices where evidence proves value and platform access is clean.

```text
NORMAL_CAMERA_APP_REQUIRED=YES
PRIVILEGED_CAMERA_BACKEND_OPTIONAL=YES_AFTER_GATES
CUSTOM_CAMERA_HAL=NO
CUSTOM_ISP_STACK=NO
CUSTOM_GCAM_DEPENDENCY=NO
GCAM_CONFIGS_AS_PRODUCT_DEPENDENCY=NO
STOCK_VENDOR_HAL_AND_ISP_INITIAL_PATH=YES
```

## Quality improvement posture

Sable Camera should not merely reproduce the Unihertz stock app UI. It should deliberately explore every safe improvement path that the sensors and HAL expose.

```text
PHOTO_QUALITY_IMPROVEMENT_OPPORTUNITY=HIGH
VIDEO_QUALITY_IMPROVEMENT_OPPORTUNITY=MEDIUM
TELEPHOTO_ZOOM_IMPROVEMENT_OPPORTUNITY=HIGH_AS_PRIVILEGED_SYSTEM_APP
GCAM_PORTS_USEFUL_AS_REFERENCE=YES
GCAM_PORTS_AS_PRODUCT_DEPENDENCY=NO
```

Quality work must be measured scene-by-scene. Sable Camera should compare stock camera output, community GCam-reference output where available, and Sable Camera output under the same scene and lighting.

```text
STOCK_CAMERA_BASELINE_SCENES=REQUIRED
GCAM_REFERENCE_SCENES=REFERENCE_ONLY
SABLE_CAMERA_SCENES=REQUIRED
SAME_SCENE_SAME_LIGHTING_COMPARISON=REQUIRED
JPEG_DETAIL_COMPARISON=REQUIRED
RAW_DNG_VALIDATION=REQUIRED
VIDEO_EIS_HFR_MATRIX=REQUIRED
HIDDEN_TELE_PRIVILEGE_GATE=DEFERRED_TO_SYSTEM_APP_PHASE
```

The goal is not to claim Pixel-class computational photography. The goal is to avoid leaving proven sensor/HAL capability unused.

## Capture modes

Sable Camera should expose a simple user model while keeping expert capture paths available.

```text
MODE_AUTO=YES
MODE_HIGH_RES=YES
MODE_RAW_DNG=YES_REAR_PUBLIC_FIRST
MODE_PRO=YES
MODE_VIDEO=YES
MODE_QUICK_REVIEW=YES
MODE_COMPARE=YES_FOR_VALIDATION_BUILDS
MODE_TELE=YES_WHEN_PRIVILEGED_CAPABILITY_PROVEN
```

### Auto

Auto is the default consumer mode. It should choose safe streams and conservative processing.

```text
AUTO_DEFAULT_SAFE=YES
AUTO_USES_PUBLIC_CAMERA_IDS=YES
AUTO_CAPTURE_CURRENT_VIEW_VISIBLE=YES
AUTO_RETURN_TO_VIEWFINDER_KEY=YES
```

### High-resolution still

Titan 2 has proven public high-resolution JPEG paths. They should be treated as first-class still modes, not hidden experiments.

```text
REAR_HIGH_RES_JPEG=8192x6144
FRONT_HIGH_RES_JPEG=6560x4928
HIGH_RES_MODE_VISIBLE=YES
HIGH_RES_MODE_EXPLAINS_LATENCY=YES
HIGH_RES_MODE_QUALITY_VALIDATION=REQUIRED
HIGH_RES_NOT_ASSUMED_NATIVE_SENSOR=YES
```

High-resolution mode must be validated against ordinary resolution captures to determine whether it adds real scene detail, vendor super-resolution, or interpolation.

### RAW / DNG

Rear RAW/DNG is proven from the ordinary app path and should become an explicit mode for advanced users and validation.

```text
REAR_RAW_DNG_MODE=YES
RAW_PUBLIC_REAR_FIRST=YES
RAW_FRONT=NO_UNLESS_CAPABILITY_PROVEN
RAW_TELE=PRIVILEGED_PHASE_ONLY
DNG_METADATA_VALIDATION=REQUIRED
COLOR_MATRIX_BLACK_WHITE_LEVEL_CHECK=REQUIRED
```

### Pro controls

Pro mode should expose only controls that are standard Camera2 or vendor controls with safe request/result evidence.

```text
MANUAL_FOCUS=YES_IF_SUPPORTED
MANUAL_EXPOSURE=YES_IF_SUPPORTED
MANUAL_ISO=YES_IF_SUPPORTED
WHITE_BALANCE_LOCK=YES
AE_AF_LOCK=YES
VENDOR_CONTROLS_EXPERIMENTAL_UNTIL_PROVEN=YES
UNKNOWN_VENDOR_ENUM_NAMES=NO_GUESSING
```

### Video

Video improvement is possible but must stay evidence-gated.

```text
VIDEO_QUALITY_IMPROVEMENT=EVIDENCE_GATED
VIDEO_1080P60_INVESTIGATION=YES
VIDEO_EIS_INVESTIGATION=YES
VIDEO_HFR_INVESTIGATION=YES
VIDEO_BITRATE_MATRIX=YES
VIDEO_FOCUS_EXPOSURE_LOCK=YES
VIDEO_LOW_LIGHT_PROFILE=EXPERIMENTAL
PIXEL_CLASS_VIDEO_CLAIM=NO
```

## Privileged SableOS backend

The normal APK path is the baseline. The privileged SableOS backend is where the larger opportunity exists for Titan 2 rear zoom and telephoto.

```text
SYSTEM_CAMERA_ACCESS=DEFERRED_TO_SABLEOS_BACKEND
LOGICAL_REAR_CAMERA_3_DESIRED_PATH=YES
DIRECT_TELE_CAMERA_2_EXPERT_PATH=YES
MANUAL_OPEN_CLOSE_SWITCHING_DEFAULT=NO
VENDOR_COORDINATED_ZOOM_PIPELINE=YES_IF_LOGICAL_CAMERA_3_ACCESS_PROVEN
NEGATIVE_ACCESS_TESTS_REQUIRED=YES
PLATFORM_PERMISSION_PATH_REQUIRED=YES
```

If logical camera `3` is accessible as a system app, it should be preferred for smooth rear zoom because stock camera behavior already indicates it coordinates main/tele switching.

If direct telephoto camera `2` is accessible, it should be exposed as an explicit tele/manual mode, not silently substituted into normal zoom until quality and stability are measured.

## GCam learnings

Community GCam work is valuable, especially for identifying the kinds of output and UX issues Unihertz devices may have: square-screen layout problems, focus-coordinate issues, preview scaling, stabilization behavior, color tuning and telephoto access experiments.

Sable Camera should use those as hypotheses and benchmarks, not as dependencies.

```text
GCAM_OUTPUT_REFERENCE=YES
GCAM_UI_COMPATIBILITY_LESSONS=YES
GCAM_CONFIG_IMPORT=NO_BY_DEFAULT
GCAM_CODE_IMPORT=NO
MODDED_GCAM_SHIPPING_DEPENDENCY=NO
ROOT_CAMERASERVER_PATCH_PRODUCT_DEPENDENCY=NO
```

## Keyboard-first UX — Camera Control Deck

Camera must be usable without touchpad-style interaction.

The accepted interaction principle is:

> **The screen is the viewfinder. The physical keyboard is the camera control
> surface.**

```text
KEYBOARD_FIRST_CAMERA=YES
CAMERA_CONTROL_DECK=YES
PORTRAIT_SUPPORTED=YES
LANDSCAPE_CONTROL_DECK=YES
GLOBAL_LANDSCAPE_LOCK=NO
PHYSICAL_KEYBOARD_SHUTTER=YES
ONSCREEN_SHUTTER_FALLBACK=YES
TOUCH_SUPPORTED=YES
TOUCH_REQUIRED_FOR_CORE_CAMERA_USE=NO
VIEWFINDER_CURRENT_CONTENT_VISIBLE=YES
CURRENT_CAPTURE_OR_REVIEW_ALWAYS_VISIBLE=YES
CONTINUOUS_SCROLL_REQUIRED_FOR_CURRENT_CONTENT=NO
TEXT_INPUT_ALWAYS_WINS=YES
SINGLE_KEY_SHORTCUTS_DISABLED_WHILE_TYPING=YES
```

Initial Sable semantic control map:

```text
Space / Enter / Camera key / Volume = shutter / start-stop video
F                                      autofocus
hold F                                 autofocus lock candidate
E                                      exposure selection
hold E                                 auto-exposure lock candidate
I                                      ISO selector
W                                      white-balance selector
S                                      shutter-speed selector
+ / -                                  zoom
Left / Right                           previous / next shooting mode
Up / Down                              adjust active parameter
C                                      switch camera
R                                      RAW / RAW+JPEG candidate
B                                      same-scene compare bracket
G                                      last capture
A                                      return selected Pro parameter to Auto
```

Keyboard focus-point movement is required without touch; the exact chord remains
physical-evidence-gated.

Manual parameter state should appear in transient strips rather than permanent
virtual DSLR controls. A keyboard/touch-dismissible control legend is required
for discoverability.

No key may bypass safety, permission or destructive confirmation. The first
release should use a strong Sable default map rather than a large arbitrary
per-key customization matrix.

## Viewfinder design

Titan 2 has a square main display and physical keyboard. The viewfinder must
avoid slab-phone assumptions.

Portrait remains a minimal conventional camera surface. Landscape should use the
Camera Control Deck layout: maximize the preview, keep only compact capture
state visible and rely on transient parameter strips for manual controls.

```text
SQUARE_VIEWFINDER_SAFE=YES
CUTOUT_ROUNDED_CORNER_AWARE=YES
PORTRAIT_MINIMAL_UI=YES
LANDSCAPE_CONTROL_DECK_UI=YES
MODE_STRIP_KEYBOARD_REACHABLE=YES
TRANSIENT_PARAMETER_STRIPS=YES
CONTROL_LEGEND_REQUIRED=YES
KEYBOARD_FOCUS_POINT_MOVE=REQUIRED
FOCUS_EXPOSURE_STATE_VISIBLE=YES
RAW_HIGH_RES_BADGES_VISIBLE=YES
LAST_CAPTURE_THUMBNAIL_VISIBLE=YES
SUBSCREEN_SELFIE_FLOW_CANDIDATE=YES
```

Advanced panels should overlay or dock without hiding shutter/focus state or
permanently consuming a large part of the preview.

## Quick review and compare

After capture, Sable Camera should make the latest capture immediately visible without forcing the user into Gallery.

```text
POST_CAPTURE_REVIEW=YES
LATEST_CAPTURE_VISIBLE=YES
DELETE_CONFIRMATION=YES
SHARE_USER_INITIATED=YES
OPEN_IN_GALLERY_HANDOFF=YES
COMPARE_MODE_FOR_VALIDATION=YES
STOCK_VS_GCAM_VS_SABLE_COMPARISON=VALIDATION_ONLY
```

Compare mode is primarily for engineering/validation builds, not the default consumer UI.

## Privacy

Camera privacy posture must be explicit.

```text
EXIF_LOCATION_VISIBILITY_EXPLICIT=YES
LOCATION_TAGGING_DEFAULT_REQUIRES_USER_CHOICE=YES
SHARE_SHEET_USER_INITIATED=YES
LOCKED_MODE_CAMERA_LIMITED=YES
LOCKED_MODE_GALLERY_REDACTION=YES
PRIVATE_MODE_REDACTION=YES
SOURCE_RESTRICTED_MODE=YES
```

## Device-profile rules

Titan 2, Titan 2 Elite and future Q27 camera behavior must be profiled independently.

```text
TITAN2_CAMERA_PROFILE=YES
TITAN2_ELITE_CAMERA_PROFILE=INDEPENDENT_REQUIRED
Q27_CAMERA_PROFILE=DEFERRED
PROFILE_PASS_INHERITANCE=NO
COMMUNITY_RESULTS_ARE_HYPOTHESES=YES
DEVICE_EVIDENCE_REQUIRED_FOR_PROFILE_PASS=YES
```

## Acceptance posture

This PR is a design/capability contract only.

```text
BUILD_CODE_CHANGED=NO
DEVICE_CODE_CHANGED=NO
FLASH_ENABLEMENT=NO
CAMERA_HAL_CHANGED=NO
CAMERA_APP_IMPLEMENTATION_CHANGED=NO
```