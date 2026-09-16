# Sable Reader text and accessibility capability

Status: **R8-D2 normative supplement to `SABLE_APP_REUSE_AND_INTEGRATION_PLAN.md`.**

This supplement adds `vaachak-platform/vaachak-textreader` as a second upstream capability source for Sable Reader. It does **not** define a second launcher-visible Reader product.

## 1. Product composition

Sable Reader remains one product identity with independently qualified reading paths:

```text
Sable Reader
  |
  +-- publication path
  |     Vaachak Mobile / Readium
  |     EPUB, library, progress, bookmarks, highlights, search
  |
  +-- text-accessibility path
        Vaachak Text Reader capability
        TXT, Android share/process-text, TTS, OCR
```

The two upstreams must retain separate provenance and qualification evidence until composition is proven in the final Sable Reader artifact.

## 2. Pinned capability source

Initial qualification source:

```text
repository: https://github.com/vaachak-platform/vaachak-textreader.git
commit:     50fca365baae9869264716569830690fb62029a7
upstream release intent: v1.4.0
```

A later upstream update requires a deliberate repin and requalification. Tracking `main` implicitly is not acceptable for an integration freeze.

## 3. Reusable capability

The pinned source provides directly relevant Android capability for:

- local `text/plain` document selection;
- pure stream-to-text parsing suitable for JVM testing;
- Android `ACTION_SEND` text share-target integration;
- Android `ACTION_PROCESS_TEXT` selected-text integration;
- Android `TextToSpeech` playback;
- user-directed WAV synthesis/export through the document picker;
- CameraX scanner flow;
- gallery-image OCR;
- Latin-script and Devanagari ML Kit text recognition;
- local translation execution after ML Kit model acquisition.

The stream reader is especially suitable for extraction/refactoring because it is Android-context independent.

## 4. Privacy and offline boundary

The upstream application currently declares `android.permission.INTERNET` and its translation path explicitly invokes ML Kit model download when a model is absent.

Therefore:

- on-device translation must not be described as equivalent to network-free operation;
- the first strict-offline Sable Reader may omit translation entirely;
- if translation is retained, model acquisition must have an explicit product/privacy policy;
- a pre-provisioned-model approach may be accepted only after reproducible packaging/runtime evidence proves it;
- OCR model packaging/runtime behavior must be independently verified before an offline-availability claim;
- camera permission is optional capability authority and must not become a prerequisite for ordinary TXT/EPUB/TTS reading.

Removing `INTERNET` from a manifest is not sufficient evidence by itself. Feature behavior, model availability and failure behavior must also be tested.

## 5. Integration rule

The preferred implementation is shared Reader modules/adapters, not installation of two separate reader APKs.

The Sable integration should preserve clear internal boundaries such as:

```text
reader-publication
  -> Readium/publication engine

reader-text
  -> TXT/input/share adapters

reader-speech
  -> Android TTS + export adapter

reader-ocr
  -> CameraX / gallery / recognition adapter

reader-app
  -> one Sable Reader package and shared design policy
```

Exact module names are implementation details; the architectural separation is normative.

## 6. Qualification gates

The Text Reader capability must have independent gates for:

1. exact repository + commit identity;
2. upstream JVM/unit tests;
3. debug/qualification APK compilation;
4. Android lint/static analysis;
5. manifest capability inventory;
6. proof of TXT picker/share/process-text/TTS/OCR source surfaces;
7. explicit observation of upstream network/model-download behavior;
8. APK SHA-256 for the qualification artifact.

These gates prove capability-source health. They do not prove Sable Reader composition.

## 7. Composition gate

Before Sable Reader may claim the new capability, evidence must establish:

```text
qualified Vaachak Mobile source
          +
qualified Vaachak Text Reader capability
          |
          v
one Sable Reader source composition
          |
          v
one package identity
          |
          +-- EPUB path works
          +-- TXT path works
          +-- share/process-text works
          +-- TTS works
          +-- OCR works when enabled/authorized
          +-- final permission inventory matches product policy
          +-- offline/network behavior matches product policy
```

A green upstream Text Reader APK alone is not sufficient for any of these final product claims.

## 8. Licensing and provenance

The source headers and upstream project description identify MIT terms, but product integration still requires a normal license/provenance inventory for the source and its dependencies, including ML Kit, CameraX, Compose and any assets.

The release freeze should retain the exact upstream commit and the corresponding license/provenance record alongside the final Sable Reader artifact evidence.
