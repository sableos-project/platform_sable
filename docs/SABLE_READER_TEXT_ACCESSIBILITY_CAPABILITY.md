# Sable Reader / Sable Text Reader product split

Status: **current product architecture — supersedes the earlier one-Reader composition plan**

The earlier R8-D2 plan proposed folding Vaachak Text Reader capabilities into one
Sable Reader product. Final Panther R9 architecture instead physically accepted
two separate launcher-visible products.

## Sable Reader

```text
package: org.sableos.reader
role: publication/EPUB/Readium reader
```

Owns publication/library/reading behavior, including qualified TTS behavior
appropriate to the publication reader.

It does not own Text Reader OCR/camera flows.

## Sable Text Reader

```text
package: org.sableos.textreader
role: local text/accessibility utility
```

Accepted capability boundary includes:

- TXT opening;
- ACTION_VIEW;
- SEND;
- PROCESS_TEXT;
- paste/edit;
- local-only TTS;
- WAV export;
- OCR for Latin + Devanagari;
- shared global appearance consumption;
- bounded input size;
- no INTERNET permission;
- no translation/model-download path;
- network-required TTS voices filtered;
- backup disabled.

## Provenance

Publication/Readium and Vaachak Text Reader provenance remain separately
traceable. Product branding removes upstream product identity while retaining
appropriate attribution/licensing.

## Portability

Both products are common Sable apps. Keyboard-first Titan work may adapt
presentation/focus/navigation, but must not create Titan-specific forks of their
core capability semantics.
