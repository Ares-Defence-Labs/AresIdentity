# Testing and performance

Capture and Latin OCR run on the device; masking runs in the compiled native processing core. Hardware, native model startup, image size and document conditions affect latency. No universal throughput, accuracy or document-authenticity claim is made.

Test on a physical Android and iOS device. Cover camera permission denial/retry, first-frame startup, torch, manual/automatic capture, route dismissal, background/foreground, disposal and repeated scans. Review portrait/landscape/EXIF rotation, glare, blur, small text, different document layouts and invalid image inputs.

Verify each sensitive rectangle against the exact upright input. Exercise indexed PNGs, fractional rectangles and JPEG re-encoding. Keep original and redacted images separate; assert that outside pixels remain useful and masked fields cannot be recovered from the newly encoded output.

For licensing, use sandbox-issued keys in an isolated sandbox-trusted build. Check valid, tampered, wrong-product and expired cases; require production trust to reject all sandbox keys. For camera unlock, capture after the full trial has elapsed; signature preflight alone is insufficient evidence.

The private source physical QA passed camera/OCR/redaction/trial suites on Galaxy S24/Android 16 and iPhone 13/iOS 26.7.1. Protected artifacts, minimum-supported devices, release-mode integration and sandbox-paid licences need separate recorded sign-off before release.


Use the [synthetic test fixtures](test-fixtures.md) for repeatable screenshots and camera setup; keep the entire card large within the guide.
