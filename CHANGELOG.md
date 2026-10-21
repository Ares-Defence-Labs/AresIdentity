# Changelog

## 2.0.0 — 22 October 2026

- Make AresIdentity self-contained with its own compiled camera, Identity product verifier, encrypted model loader and private `ares_identity_inferencer.onnx`; customer packages contain only `ares_identity_inferencer.aem` and no `ares_edge` dependency.
- Remove inherited editor/filter components, isolate native channels and obfuscated classes for coexistence with Edge, and prevent rejected expired/future keys from replacing valid cached Identity access.
- Verify standalone Android/iOS customer builds, mixed-SDK linking, native sandbox/production key isolation, authenticated model readers and package security/metadata; retain publication and live-checkout guards for user validation of the new binaries.

- Prepare package-page screenshot metadata and document encrypted shared-model/native-processing boundaries; audit exact customer/Edge artifacts, nested archives, reviewed public assets and whole-package hashes while retaining disabled publication and recording the pending compatible Edge release.

- Align customer documentation with AresEdge: branded README, integration choices, app badges, licence setup, screenshot workflows and runnable examples; add branding/media package assets while retaining source setup separately.

- Pin package README screenshots to the reviewed public documentation commit so image previews render before merge and remain stable across documentation updates.

- Add supplied scanner and redacted synthetic-card screenshots to repository guides and the package README, clarify host-app editor features, and include documentation image assets in local customer packages.

- Verify consecutive supplied-route Redact/Analyse captures on iPhone 13 and Galaxy S24; add synthetic bank-card and driver-licence documentation PNGs.
- Coordinate the iOS scanner-core fix that prioritizes precise model corners and explicit confidence over heatmap fallback independently of output order.

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

- Exclude local guarded native-review dependency overrides from source control.

- Record full AresScan sandbox owner validation on Android and iPhone, including protected model readiness, post-trial native redaction, invalid-input rejection, and compact controls above ads.

- Hide the document-following overlay for Identity capture and expose stable detection/countdown feedback separately from hint visibility, preventing idle guide activation and rapid hint flicker.

- Provide the AresScan-style standalone route with selected native privacy treatment, a typed applied-level/image-bytes record, configurable guide color, state hints and native stable-document countdown (1–10 seconds), while retaining the embedded camera and avoiding an editor. Clear active feedback on capture errors and safely retire native camera ownership when routes close.

AresIdentity v2.0.0
