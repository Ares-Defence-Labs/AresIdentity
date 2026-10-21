# Getting started

AresIdentity captures identity documents, locates sensitive fields with on-device OCR and returns redacted images. Android and iOS are supported. It does not authenticate documents or verify a person's identity.

## Obtain the package

Install the self-contained package from pub.dev. The GitHub repository holds
documentation and support, so do not use a Git dependency pointing at it. No
separate Edge SDK is required.

```yaml
dependencies:
  flutter:
    sdk: flutter
  ares_identity: ^2.0.0
```

Run `flutter pub get`, configure [Android and iOS](platforms.md), and fully
rebuild after changing native binaries. Hot reload cannot update native licence
verification. Use Flutter 3.44+, Dart 3.12+; example tooling uses Dart 3.13.4+.
Use the package's hosted dependencies; do not copy private source overrides into customer apps.

## Ready-made scanner

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

The result contains cropped bytes after the selected privacy treatment and an
applied-level enum. The SDK does not open an editor. See [scanner controls and
result semantics](scanner-screen.md).

## Choose an integration

- [Scanner screen](scanner-screen.md): ready-made Flutter route with capture and torch controls.
- [Embedded camera](embedded-camera.md): camera and overlay under your own controls.
- [Recognition and redaction](recognition-and-redaction.md): process an upright encoded document image.

Provide your application's signed Identity licence from private build configuration. Missing/invalid keys use the 60-second process trial. See [licensing](licensing.md), including isolated sandbox testing.

AresIdentity includes its own compiled scanner, model loader and Identity licence verifier. It does not depend on `ares_edge`; no separate Edge key or installation is required. Version 2.0.0 includes the protected native camera and processing implementation.