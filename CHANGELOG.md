# Changelog

## Unreleased

- Add Edge-equivalent branch/tag rules, mandatory handwritten release notes, and the GitHub Desktop/CLI push hook.

- Extract identity capture, OCR exposure analysis, passport, driver licence and bank-card redaction from EdgeDetectionSDK.
- Add an identity scanner screen and shared 60-second unlicensed camera trial.
- Add runnable Android/iOS example hosts, an analysis/redaction preview, and physical-device integration coverage for camera capture, native OCR, masking, licence rejection and trial expiry.

- Fix solid masking for indexed PNG images and nonempty fractional OCR regions.
- Report capture failures to host applications instead of silently swallowing them.

- Use the public AresEdge camera API with immutable AresIdentityImage captures and a standalone Identity controller.
- Run classification, sensitive-field policies and solid masking in compiled Android/iOS code; enforce native Identity product access before processing and before returning results.
- Package compiled AAR/XCFramework implementations with thin registration adapters; exclude private helper/policy source and require hashed native inputs.
- Add full integration, platform, licensing, recognition, API, example, performance and release-status guides, checked examples, clean API HTML generation and native third-party notices.
- Verify protected artifacts on Galaxy S24 and iPhone 13: five passing tests per device, sandbox licence matrix after trial, plus production-pin rejection of the same sandbox fixtures. Publication remains disabled pending coordinated release gates.

- Verify a US$600 Stripe sandbox-paid Identity key offline and on both physical devices with renewal off; keep coordinated publication and live checkout gated.

AresIdentity v0.0.1
