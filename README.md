# AresIdentity

Flutter identity document capture, on-device sensitive-field analysis and solid image redaction for Android and iOS. Supports passports, driver licences and bank cards, with a ready-made screen or an embedded camera under your own UI.

**Private source SDK in development. The customer package is not published yet.** Package publication and live checkout remain disabled until the release verification gates pass.

## Architecture and installation

The Identity SDK uses the public protected `ares_edge` camera. It does not embed another Edge camera implementation or require the private owner UI package. ML Kit performs OCR on device. Exposure policies, document classification and masking execute in the compiled `ares_identity_native` implementation. The public Dart API contains models and platform adapters. The nested `ares_identity_core` retains private owner compatibility types and legacy helpers; those helper algorithms are excluded from customer artifacts.

For authorized source development:

```yaml
dependencies:
  ares_identity:
    path: ../IdentitySDK/ares_identity
```

Use Flutter 3.44+ / Dart 3.12+; the example/documentation tools use Dart 3.13.4+. The compiled shared native core must advertise Identity licence protocol 2, AID2 support and native authority ABI 1. Older hosted Edge binaries cannot validate Identity keys; the SDK reports an actionable compatibility message. Local native candidates are development overrides, not published release versions.

## Two scanner integrations

1. **Supplied activity-style route:** `AresIdentityScreen.open(context, ...)`
   presents the AresScan-style Identity UI, applies the selected privacy
   treatment, and returns `({AresIdentityRedactionLevel level, Uint8List bytes})`.
   The image is cropped; no image editor is opened. Cancellation returns null.
   Set the guide color, state-based hints and native stable-document countdown
   (`captureTimeout`, whole seconds 1–10; default 2) without rebuilding the layout.
2. **Embedded scanner:** `IdentityIntelligenceScanStreamer` supplies the camera
   and native capture behavior. Your app owns the controls, layout, hints and
   subsequent recognition/redaction workflow.

Both integrations use the same protected camera and Identity licensing.
The route's enum is `none`, `minimal`, `balanced` or `strict`; Analyse or no
identified masking regions returns `none`. The enum is a policy label, not a
claim that OCR found all sensitive fields.

## See the scanner and redacted result

| Identity capture | Redacted image in AresScan+ |
| --- | --- |
| <img src="https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/main/docs/assets/screenshots/identity-scanner.png" alt="AresIdentity scanner with Minimal privacy selected and a synthetic bank card detected" width="280"> | <img src="https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/main/docs/assets/screenshots/redacted-card-aresscan.png" alt="Cropped synthetic bank card with its number masked, displayed by the AresScan+ host app" width="280"> |

The scanner offers Minimal, Balanced and Strict privacy levels, Redact/Analyse
modes, document-state hints and an animated capture guide. The result screenshot
shows the synthetic card number covered by a solid mask; the cardholder and
expiry remain visible in this example.

**The result is displayed in AresScan+, the host application.** Its editor,
Save/Share controls, Pages and AI Tools are application features. The
AresIdentity supplied route returns the cropped, privacy-treated image bytes
and applied-level enum directly to your app; it does not open an editor.

[View the screenshot guide](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/visual-demos.md)
· [Download synthetic test documents](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/test-fixtures.md)

## Capture, review and redact

```dart
import 'package:ares_identity/ares_identity.dart';

// AresIdentityScreen.open returns the processed level/bytes tuple.
// The embedded callback receives AresIdentityImage with cropped image.bytes.
// Analyse the exact upright captured pixels, present regions for review,
// then call analysis.redact(image.bytes) to produce a new JPEG.
```

The public capture result contains encoded document pixels, optional dimensions and an isDocumentCropped flag. It does not expose uncropped original bytes, raw detector geometry or face detection. OCR uses ML Kit's Latin recognizer; externally recognized text can be passed to analyseIdentity. Low/balanced/maximum aggression select document masking policies. Detection is heuristic and does not establish authenticity or identity.

## Licensing

The configured fee matches AresEdge: US$600 per twelve-month term, covering the customer's supported apps. Supply the complete AID2 token with signed product AresIdentitySDK, not an Edge/PDF/Office key or a portal licence ID. Missing, invalid or expired keys use the native process-wide 60-second camera trial. Remounting does not reset it; renewal requires shipping a replacement key before expiry.

All licence tests in this project use sandbox keys. Production issuer pins remain unchanged and reject sandbox signatures. Sandbox-trusted artifacts stay isolated, clearly labelled and non-publishable. Live paid-licence validation is not claimed.

## Documentation

- [Getting Started](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/getting-started.md)
- [Platforms](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/platforms.md)
- [Screenshots and Result Examples](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/visual-demos.md)
- [Scanner Screen](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/scanner-screen.md)
- [Embedded Camera](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/embedded-camera.md)
- [Recognition And Redaction](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/recognition-and-redaction.md)
- [Api Reference](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/api-reference.md)
- [Licensing](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/licensing.md)
- [Example Project](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/example-project.md)
- [Performance Testing](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/performance-testing.md)
- [Synthetic Test Fixtures](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/test-fixtures.md)
- [Release Status](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/release-status.md)

## Verification and packaging

Run flutter analyze and flutter test test packages/ares_identity_core/test. The example contains physical camera/OCR/redaction/trial tests. The protected candidate passed five physical tests on each of Galaxy S24 and iPhone 13, including public-camera capture, native OCR/masking and sandbox licensing after trial expiry. Coordinated release and production-trust gates remain separate. See the private physical-device report for evidence scope.

`python3 tool/documentation/build_api.py` generates and validates HTML API docs with isolated pinned tooling; `python3 tool/documentation/verify_examples.py` analyzes complete guide examples. `tool/distribution/build_customer_package.py` assembles a guarded local single-package candidate, requires hashed Android/iOS native binaries, includes only public models and thin adapters, and retains only public Flutter/ML Kit/AresEdge dependencies. It never publishes.

Commercial use is governed by [LICENSE](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/LICENSE). See SECURITY.md and SUPPORT.md for private reporting and safe reproductions.
