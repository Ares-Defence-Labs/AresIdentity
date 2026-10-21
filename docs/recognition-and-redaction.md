# Recognition, exposure analysis and redaction

```dart
import 'dart:typed_data';
import 'package:ares_identity/ares_identity.dart';

Future<Uint8List> redactIdentity({
  required String uprightImagePath,
  required Uint8List uprightImageBytes,
  required int width,
  required int height,
  String? licenceKey,
}) async {
  final analysis = await recogniseIdentity(
    inputImage: InputImage.fromFilePath(uprightImagePath),
    imageWidth: width,
    imageHeight: height,
    licenceKey: licenceKey,
    aggression: IdentityLevelAggressionMode.balancedAggression,
  );
  // Present report.findings and sensitiveRects for review before export.
  return analysis.redact(uprightImageBytes);
}
```

`recogniseIdentity` opens and closes an on-device Latin TextRecognizer. Omit documentType for automatic OCR-text classification, or supply ClassificationType.passport, driversLicence or bankCard. For an existing RecognizedText from your own recognizer, await `analyseIdentity` with the same dimensions. That route can accept other OCR scripts, but field recognition is still heuristic and document/language coverage varies.

`AresIdentityAnalysis.report` describes exposed fields, reasons and recommended aggression. `sensitiveRects` is an immutable list of pixel rectangles. Omitting aggression selects the report recommendation. The enum values are lowAggression, balancedAggression and maxAggression; stronger settings cover more fields and may obscure useful content.

`redact(bytes, jpegQuality: 94)` returns a new JPEG with solid black masks, leaving input bytes unchanged. It normalizes indexed image pixels before filling and masks every nonempty rectangle, including fractional regions. The low-level IdentityRedactionProcessor can apply supplied rectangles or heuristically check dark coverage. That check is not a proof that OCR found every sensitive field.

## Coordinate and privacy rules

Use the exact upright pixels supplied to OCR. Bake EXIF orientation before analysis. Do not reuse rectangles after rotation, scaling or a new crop. Width/height must be positive and must describe those pixels. faceRect, if supplied to analyseIdentity, uses that same coordinate space.

Review document type, detected regions, face coverage, MRZ and final output. Empty OCR is not proof of safety. Passport logic conservatively masks an expected MRZ band even when OCR misses it. Keep originals and temporary OCR files under your application's retention policy; do not log raw OCR, identity images or licence strings. The SDK does not export or upload images automatically.

Pass the same Identity key to recogniseIdentity/analyseIdentity as to the camera. The analysis retains it privately for redact; it is never printed. Each compiled processing operation validates native product access again. Missing/invalid keys only have the shared native process trial, and native refusals propagate without a Dart processing fallback.

## Masking levels

| Document | Low | Balanced adds | Maximum adds |
| --- | --- | --- | --- |
| Passport | Passport number and MRZ | Dates | Detected names, sex, nationality, signature and optional face rectangle |
| Driver licence | Licence number | Dates, address and card number | Detected names, sex, class/conditions and optional face rectangle |
| Bank card | Card number | Expiry and cardholder name | Labelled CVV/CVC and optional face rectangle |

Native policy uses OCR element and line geometry. Label/value layouts may mask whole lines and nearby values, so review the suggested regions. When a passport MRZ is not recognized, the bottom 28 percent is masked as a precaution. Generic document types use heuristic identifier/date matches. These heuristics do not establish document authenticity, complete PII discovery or a certified redaction result.

Inputs are limited to 20 million image pixels and 50 MB encoded images; analysis also bounds OCR text and geometry counts. Unsupported or invalid input raises a platform error. Rectangles and dimensions must refer to the same upright pixels.
