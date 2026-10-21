# Ready-made scanner screen

Import Flutter Material and the SDK:

```dart
import 'dart:typed_data';
import 'package:flutter/material.dart';
import 'package:ares_identity/ares_identity.dart';

Future<void> scanIdentity(
  BuildContext context, {required ValueChanged<Uint8List> onCaptured}
) async {
  await Navigator.of(context).push<void>(MaterialPageRoute(
    builder: (scanContext) => AresIdentityScreen(
      licenceKey: const String.fromEnvironment('ARES_IDENTITY_LICENCE'),
      forceEdgeOverlay: false,
      overlayColor: Colors.teal,
      onCapture: (capture) {
        final croppedBytes = capture.bytes;
        onCaptured(croppedBytes); // Host reviews/processes this image.
        Navigator.of(scanContext).pop();
      },
    ),
  ));
}
```

The screen is a Flutter route hosted by your existing Activity/UIViewController. It provides back navigation, torch, capture and status text. Your onCapture callback decides when to dismiss it and what to store. Automatic capture is enabled; the capture button can also request a manual capture.

The callback receives AresIdentityImage: immutable encoded document bytes, optional width/height and isDocumentCropped. It does not return the original uncropped frame, detector geometry or a face box. This capture is an image, not an exposure report or an authenticity verdict.

`forceEdgeOverlay: false` follows the device capability policy. True requests continuous overlay tracking. `overlayColor` defaults to the host theme. These options do not change native licensing.

After unlicensed trial expiry the route attempts to return without capture. Handle route dismissal as cancellation. For a camera at the root of your app, use the embedded integration and its licence-required callback.
