[![Ares Defence Labs](https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/1a554f511259b818e3ef84faed64de61057fe269/branding/ares-defence-labs-banner.png)](https://aresdefencelabs.com)

# AresIdentity

[Official website](https://aresdefencelabs.com/) · [GitHub repository](https://github.com/Ares-Defence-Labs/AresIdentity) · [Developer documentation](https://aresdefencelabs.com/development-sdks?sdk=identity-scanner&view=overview)

> **Commercial SDK — paid licence required for production use.**
> AresIdentity is proprietary software from Ares Defence Labs. Developers can
> evaluate the SDK using its native 60-second live-session trial. Without a valid
> licence, scanning deactivates when the trial expires.
>
> Configure the full signed licence key in application source or private build
> configuration. One customer licence covers unlimited apps across supported
> platforms. Ship a replacement key before expiry so existing app installations
> retain access.
>
> [Licensing and availability](https://aresdefencelabs.com/development-sdks?sdk=identity-scanner&view=overview) · [Read the commercial licence](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/LICENSE)

Commercial Flutter identity-document capture, on-device sensitive-field analysis
and solid image redaction for Android and iOS. Supports passports, driver licences,
bank cards and heuristic identity-card field detection with Latin OCR.

Open **AresIdentityScreen** for the supplied AresScan-style scanner, or use
**IdentityIntelligenceScanStreamer** for an embedded camera under your own UI.
The supplied route returns the cropped image after the selected privacy treatment
and an enum describing the applied level. Your app handles the result.

**Package:** `ares_identity` v2.0.2 is a self-contained Android/iOS SDK.
Install it from pub.dev; the GitHub repository provides documentation and support,
rather than the SDK implementation. No `ares_edge` package or Edge licence is
required. See [release status](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/release-status.md).

```yaml
dependencies:
  ares_identity: ^2.0.2
```

## Choose your scanner integration

**Build your own UI:** embed `IdentityIntelligenceScanStreamer`, supply your own
controls and hints, and use `LiveIdentityEdgeScanController` for capture and mode
selection. Your app chooses when to analyse and redact the captured image.

**Use the provided UI:** open `AresIdentityScreen` for privacy controls,
Redact/Analyse selection, animated guide and state hints. Configure guide colour,
state-based hints and the stable-document countdown without rebuilding its layout.
The route applies the selected privacy policy and returns the cropped image/level
tuple directly, without opening an image editor.

### 1. Capture an identity document

<img src="https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/1a554f511259b818e3ef84faed64de61057fe269/media/identity-scanner.png" alt="AresIdentity supplied scanner detecting a synthetic bank card with Minimal privacy selected" width="280">

Hold a confident document steady, follow the countdown and receive a
perspective-corrected crop. Movement or stale detection cancels pending capture.
See [the supplied scanner](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/scanner-screen.md).

### 2. Apply the selected privacy treatment

Redact offers Minimal, Balanced and Strict policies. In the synthetic card below,
the number is masked while the cardholder and expiry remain visible. Analyse
returns cropped pixels without masks. Review the output before sharing.

<img src="https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/1a554f511259b818e3ef84faed64de61057fe269/media/redacted-card-aresscan.png" alt="Cropped synthetic bank card with its number masked, displayed in AresScan+" width="280">

This result is displayed in **AresScan+**, the host application. Its editor,
Save/Share, Pages, AI Tools and surrounding controls are app features.
AresIdentity returns image bytes and an applied-level enum; it does not include
that editor. See [screenshots and feature scope](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/visual-demos.md).

### 3. Return the image to your app

`AresIdentityScanResult` is a named Dart record:
`({AresIdentityRedactionLevel level, Uint8List bytes})`.
The enum is `none`, `minimal`, `balanced` or `strict`. Analyse and scans with no
identified masking regions return `none`. The level describes the applied policy;
it does not certify complete discovery of sensitive fields.

## Scanner appearance, hints and countdown

Set `overlayColor`, `hintBuilder` and `captureTimeout` on `AresIdentityScreen.open`.
`captureTimeout` accepts whole seconds from 1 through 10 (default 2), configuring
the countdown after stable detection. It is not a maximum session duration.
The complete example below shows each setting.

The guide stays inactive while searching and animates during detection/countdown;
no filled overlay obscures the document. Custom settings preserve native
confidence, motion, freshness and licence checks. For the embedded camera, use
`onScanStateChanged` to drive your own state hints. See [scanner configuration](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/scanner-screen.md).

## Try AresIdentity in AresScan+

Experience identity-document scanning in **AresScan+**, the official
Ares Defence Labs app.

[![Get AresScan+ on Google Play](https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/1a554f511259b818e3ef84faed64de61057fe269/branding/google-play-badge.png)](https://play.google.com/store/apps/details?id=ai.aresdefencelabs.aresscan) [![Download AresScan+ on the App Store](https://raw.githubusercontent.com/Ares-Defence-Labs/AresIdentity/1a554f511259b818e3ef84faed64de61057fe269/branding/app-store-badge.png)](https://apps.apple.com/us/app/aresscan/id6759765449)

[Discover AresScan+](https://aresdefencelabs.com/ares-scan) · [Explore more Ares Defence Labs SDKs](https://aresdefencelabs.com/development-sdks)

## See scanning and redaction in action

The screenshots use a fictional sandbox bank card. Download the
[bank card and driver-licence PNG fixtures](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/test-fixtures.md)
for integration screenshots and repeatable camera checks. All fixture names and
identifiers are synthetic. See [visual demos](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/visual-demos.md) for
original screenshots and the distinction between SDK and host-app features.

## Run the example application

The customer candidate includes an `example/` app for capture, on-device analysis
and redacted-image preview. If its platform host folders are absent, run
`flutter create --platforms=android,ios .` from `example/` first. Configure the
[platform permissions, minimum OS and signing](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/platforms.md)
before running it on a physical device:

```sh
# From an approved local customer package:
cd example
flutter pub get
flutter run --dart-define=ARES_IDENTITY_LICENCE=YOUR_PORTAL_ISSUED_KEY
```

Install the released package from pub.dev, or use an approved local candidate. Do not use this documentation repository as a Flutter dependency.
See [example setup](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/example-project.md) and
[performance testing](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/performance-testing.md) for device checks.

## Documentation

- [Installation and quick start](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/getting-started.md)
- [Embedded camera and custom controls](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/embedded-camera.md)
- [Ready-made scanner](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/scanner-screen.md)
- [Recognition, exposure analysis and redaction](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/recognition-and-redaction.md)
- [Complete public API reference](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/api-reference.md)
- [Offline licensing, trial and renewal](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/licensing.md)
- [Platform setup and troubleshooting](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/platforms.md)
- [Screenshots and feature scope](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/visual-demos.md)
- [Performance and device testing](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/performance-testing.md)
- [Example application](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/example-project.md)
- [Synthetic bank card and driver licence](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/test-fixtures.md)
- [Model, asset and distribution security](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/security-and-assets.md)
- [Release status](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/release-status.md)

- [Report a bug or ask a question](https://github.com/Ares-Defence-Labs/AresIdentity/issues)

## Configure your licence

Supply the full signed AID2 customer key authorizing AresIdentitySDK, rather than
a portal licence ID or another SDK's key. End users do not enter licence keys.
The configured fee matches AresEdge: US$600 per twelve-month term. Live checkout
is managed separately from package publication; contact Ares Defence Labs for
production licence availability.

```dart
const licenceKey = String.fromEnvironment('ARES_IDENTITY_LICENCE');
```

```sh
flutter run --dart-define=ARES_IDENTITY_LICENCE=YOUR_PORTAL_ISSUED_KEY
```

Keys verify offline. Missing, invalid or expired keys use the shared native
60-second process trial; remounting the camera does not restart it. Replace the
embedded key in every app before expiry. Never commit a real key to a repository.

**Sandbox keys require an isolated sandbox-trusted test build.** Release trust
rejects sandbox signatures. Native licence QA uses sandbox keys, without requiring
a live payment. See [licensing and renewal](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/licensing.md).

## Ready-made scanner: AresIdentityScreen

```dart
import 'package:flutter/material.dart';
import 'package:ares_identity/ares_identity.dart';

Future<AresIdentityScanResult?> scanIdentity(BuildContext context) async {
  final result = await AresIdentityScreen.open(
    context,
    licenceKey: const String.fromEnvironment('ARES_IDENTITY_LICENCE'),
    overlayColor: const Color(0xFF00FFD1),
    captureTimeout: const Duration(seconds: 3),
    hintBuilder: (state) => switch (state.phase) {
      'searching' => 'Place your document inside the guide',
      'holding' => 'Keep the document steady',
      'countdown' => 'Capturing in ${state.remainingSeconds}',
      'capturing' => 'Taking photo…',
      'processing' => 'Applying privacy treatment…',
      'error' => 'Could not process this document. Please try again.',
      _ => state.message,
    },
  );
  if (result == null) return null; // Back or trial expiry.
  // Named Dart tuple: result.level is the applied enum;
  // result.bytes is the cropped image after the chosen privacy treatment.
  return result;
}
```

The route returns cropped encoded image bytes after privacy treatment. In Redact
mode, processing failures keep the route open with an error rather than returning
untreated pixels as a redacted result. Analyse returns unmasked cropped bytes and
level `none`. `onAnalysis` optionally receives the same capture's findings.
Cancellation or trial expiry returns null. Your app can display
`Image.memory(result.bytes)`, save the bytes or continue its own workflow.

## Custom camera: IdentityIntelligenceScanStreamer

Retain the controller and flash notifier in State, add your own controls and
handle captured bytes:

```dart
import 'package:flutter/material.dart';
import 'package:ares_identity/ares_identity.dart';

class IdentityCamera extends StatefulWidget {
  const IdentityCamera({super.key, required this.licenceKey});
  final String licenceKey;
  @override
  State<IdentityCamera> createState() => _IdentityCameraState();
}

class _IdentityCameraState extends State<IdentityCamera> {
  final flash = ValueNotifier(false);
  final controller = LiveIdentityEdgeScanController();
  @override
  void dispose() {
    flash.dispose();
    super.dispose();
  }
  @override
  Widget build(BuildContext context) => Scaffold(
    body: IdentityIntelligenceScanStreamer(
      (status) { /* Show capture status. */ },
      (capture) { /* Review capture.bytes, the cropped encoded image. */ },
      showHint: (visible) {},
      flashOn: flash,
      controller: controller,
      isManualMode: true,
      licenceKey: widget.licenceKey,
      forceEdgeOverlay: false,
      captureTimeout: const Duration(seconds: 3),
      overlayColor: Colors.teal,
      onLicenceRequired: () { /* Display your renewal/help UI. */ },
    ),
    floatingActionButton: FloatingActionButton(
      onPressed: controller.trigger,
      child: const Icon(Icons.camera_alt),
    ),
  );
}
```

The host owns and disposes the flash notifier. The controller has no dispose
method. `trigger()` requests capture; permission, readiness and licence checks
still apply. The callback returns `AresIdentityImage`, rather than the supplied
route's processed tuple. Your app runs recognition/redaction explicitly.
See [embedded controls and lifecycle](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/embedded-camera.md).

## Model and asset protection

AresIdentity is self-contained: its camera and authenticated encrypted model
ship inside this SDK, with no `ares_edge` dependency. The private source model
is `ares_identity_inferencer.onnx`; customer artifacts contain only
`ares_identity_inferencer.aem`. Native processing is compiled, and public
Dart adapters expose the documented API. Branding, screenshots and synthetic
fixtures are readable documentation assets, not registered runtime model assets.
Reviewed asset inventories and whole-package hashes detect unexpected changes.
Encryption resists casual extraction but cannot prevent a host that controls
the process from inspecting binaries or decrypted memory. See
[security and asset boundaries](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/security-and-assets.md).

## Platform requirements

Android API 24+, iOS 16+, Flutter 3.44+, Dart 3.12+. The example tooling requires
Dart 3.13.4+. Identity includes its own camera, native product verifier and
processing core. No separate AresEdge package or licence is required.
The current Identity candidate uses CocoaPods on iOS. Only one SDK camera can be
active at a time. See [platform setup](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/platforms.md).

## Document extraction and redaction

Camera captures extract the detected document and correct perspective. The
supplied route applies the selected privacy treatment to that crop. The embedded
camera exposes `bytes`, `isDocumentCropped` and optional `width`/`height`; set
`extractDocument: false` only for a host-controlled full-frame processing flow.

`recogniseIdentity` runs Latin OCR on device and analyses fields. `analyseIdentity`
accepts existing recognized text with matching upright image dimensions.
`AresIdentityAnalysis.redact` creates a new JPEG with solid masks. Policies and
masking run in the compiled native Identity core. The SDK does not automatically
upload images or prove document authenticity or a person's identity.
See [recognition and redaction](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/recognition-and-redaction.md).

### Supported public API

Import `package:ares_identity/ares_identity.dart` for `AresIdentityScreen`,
`AresIdentityScanResult`, `AresIdentityRedactionLevel`,
`IdentityIntelligenceScanStreamer`, `LiveIdentityEdgeScanController`,
`IdentityScanState`, `AresIdentityImage`, `AresIdentityAnalysis`,
`recogniseIdentity`, `analyseIdentity` and the documented models/policies.
Internal helper paths, owner integrations and native channels are private.
See the [API reference](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/docs/api-reference.md).

A commercial licence is required for production use. Downloading documentation,
examples or compiled binaries does not grant a free production licence.
See [LICENSE](https://github.com/Ares-Defence-Labs/AresIdentity/blob/main/LICENSE). Third-party components retain their own terms.
