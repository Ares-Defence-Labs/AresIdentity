# Embedded camera and custom controls

Use this entry point when your app supplies the UI and chooses when to run OCR
and redaction. For the SDK-supplied AresScan-style UI and processed result tuple,
use `AresIdentityScreen.open` instead. Both share the same protected native camera.

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

Mount the camera in a visible, bounded layout. Its visibility lifecycle starts and suspends the native camera. Early capture can precede the first camera frame; wait for readiness/status and allow the user to retry. Capture failures produce a status rather than an image.

Set `flash.value` to request torch state. The host owns and disposes that notifier. The controller has no dispose method. `trigger()` requests capture; `cancelTimer()` invokes an explicitly host-bound cancellation action; it does not cancel the native automatic countdown. `triggerIdentityChange(mode)` stores an application-selected `redact` or `analyse` mode; the camera callback still returns image bytes, so the host performs analysis/redaction explicitly.

With `isManualMode: false`, stable document detection can start automatic capture. On trial expiry the embedded camera becomes a white licence-required view. Remounting it does not restart the native process trial.

`extractDocument: true` (default) requires a detected boundary and returns a perspective-corrected document. Set false to return a full camera frame for a host-controlled processing flow; check isDocumentCropped on the result. Camera QA can use full-frame capture without requiring a physical sample document.


### Detection feedback and guide animation

Identity capture leaves the detected document unobscured: its native polygon,
tint, grid and document-following countdown trace are hidden. Detection and
automatic capture still run normally. A host can draw a fixed guide using
`onScanStateChanged`: `IdentityScanState.documentDetected` controls its active
appearance, and `phase`/`remainingSeconds` drive countdown feedback. Do not use
`showHint` to infer document presence; a searching hint can be visible while no
document is detected. Brief holding/searching noise is smoothed for presentation;
native countdowns and capture authorization remain authoritative.

`captureTimeout` configures the native automatic countdown (whole seconds 1–10; default 2). Manual `trigger()` captures immediately when ready and still enforces native licensing/document extraction.
