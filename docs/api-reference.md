# Public API reference

Import `package:ares_identity/ares_identity.dart`. The current API belongs to the private source edition; customer distribution is not published yet. Internal src paths and native method-channel names are not supported integration APIs.

| API | Contract |
| --- | --- |
| AresIdentityScreen | Ready-made route; requires onCapture. Optional licenceKey, overlayColor, forceEdgeOverlay=false. Host dismisses after accepting capture. |
| IdentityIntelligenceScanStreamer | Embedded camera. Two positional callbacks: status string and AresIdentityImage capture. Required showHint and flashOn. Optional controller, isManualMode=false, licenceKey, forceEdgeOverlay=false, overlayColor, onLicenceRequired, extractDocument=true. |
| LiveIdentityEdgeScanController | trigger(), cancelTimer(), triggerIdentityChange(mode), readable identityMode. |
| IdentityIntelligenceMode | redact, analyse. Application mode; it does not itself run OCR or masking. |
| IdentityLevelAggressionMode | lowAggression, balancedAggression, maxAggression. |
| recogniseIdentity | Future<AresIdentityAnalysis>; required inputImage, imageWidth, imageHeight; optional documentType, aggression, faceRect, licenceKey. Owns/closes a Latin recognizer. |
| analyseIdentity | Future<AresIdentityAnalysis>; required RecognizedText text, imageWidth, imageHeight; optional documentType, aggression, faceRect, licenceKey. Field analysis executes in the compiled Identity core; OCR is not repeated. |
| AresIdentityImage | Immutable bytes, optional width/height, isDocumentCropped. Does not expose original/quad/face outputs. |
| AresIdentityAnalysis | report, immutable sensitiveRects, redact(encodedBytes, jpegQuality=94). |
| IdentityAnalysisReport | type, recommendedAggression, reasons, findings, hasAnyExposure. |
| IdentityFinding | key, exposed, rects, confidence, note. Confidence is heuristic, not an identity-verification score. |
| IdentityRedactionProcessor | apply(imageBytes, rects, jpegQuality=94, licenceKey); containsUnredactedRegions(imageBytes, rects, licenceKey). Compiled native processing with Identity licence enforcement. |
| ClassificationType | Document category enum; automatic classification is performed by the compiled Identity core. |
| InputImage / RecognizedText / TextRecognizer | Re-exported ML Kit types; externally created recognizers remain the host's responsibility. |
| getDocumentFriendlyName | Display label for a classification. |

Legacy Dart helper paths are private owner compatibility only and are excluded from the customer artifact. Public recognition/analysis and IdentityRedactionProcessor operations require product-authorized native processing; there is no Dart algorithm fallback.

AresIdentityImage contains bytes, optional width and height, and isDocumentCropped. Camera geometry and face detection are not public capture outputs. See [scanner screen](scanner-screen.md) and [recognition](recognition-and-redaction.md) for lifecycle and coordinate details.

Dart API HTML is generated from the private SDK using `python3 tool/documentation/build_api.py`, which pins an isolated Dartdoc 9.0.11 tool (Dart 3.13.4+). The bundled Dartdoc 9.0.6 currently crashes while indexing dependencies. Complete guide examples are checked with `python3 tool/documentation/verify_examples.py`. Generated output is a local build artifact, not a declaration that a public release exists.
