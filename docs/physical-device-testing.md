# Physical device validation

## AresScan owner integration — 21 October 2026

The full AresScan sandbox integration test passed on Galaxy S24 and iPhone 13.
Both used the real compiled native scanner/Identity channels and a separately
signed sandbox six-SDK ADLSA2 fixture, never the production owner key. Checks
covered owner acceptance, protected model/runtime initialization, rejection of
invalid owner/model inputs, the real shared trial reaching zero, licensed native
analysis/redaction afterward, and Redact/Analyse controls at 320×568 with 200%
text and a 100-pixel ad area. Android's active camera and native inference cadence
were independently observed. These full-app checks supplement the earlier five
protected-package capture/OCR tests per device; they do not claim broader real
document accuracy or minimum-OS coverage.

Android QA must use the existing App Bundle local-testing delivery flow. A plain
APK lacks the on-demand runtime/components and is not a valid delivery test.
The iOS pack contained an older ORT-format file under an ONNX filename; it now
uses the same approved ONNX bytes as Android (13,167,814 bytes, SHA256
`950070fd4a46e25d46db7c35d0ed77612d9063f70d4543c7c504f0da6a943a89`).
The private owner helper refreshes an old-format Apple cache once. Native
signature, model hash, encrypted-container and inference checks remain required.

AresScan uses an explicit Identity key when provided, otherwise its shared
six-product owner key. Customer-test builds clear that fallback. Model startup
precedes camera creation; failure offers a retry. Scanner controls and the guide
share measured available space above ads; large text uses a privacy dropdown.
The final 12 layout/configuration tests, nine SDK Dart tests, and 18 issuer tests
passed. PDF's shared-admin verifier also accepts the Identity claim while still
requiring its own product scope (12 compatibility tests passed).

Normal builds use the genuine private owner configuration; native licence QA
remains sandbox only. Local native dependency overrides are retained until the
coordinated hosted Edge version is available. Source/example publication metadata,
release configuration and the GitHub publication switch remain disabled. Nothing
was published to pub.dev. Documentation-only draft updates follow the successful
local physical verification; the new app/Edge integration implementation is local. Token-free
local evidence is in AresScan's ignored `build/identity-integration-20261021/`.
