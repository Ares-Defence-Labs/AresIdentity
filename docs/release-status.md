# AresIdentity v2.0.0

Release date: 22 October 2026. This is a self-contained customer SDK with its own
compiled camera, Identity licence verifier and encrypted model. It has no
`ares_edge` dependency. The source model is `ares_identity_inferencer.onnx`;
customer resources are authenticated `ares_identity_inferencer.aem` files.

## Verification scope

Local Android and unsigned iOS profile consumers build successfully, both alone
and with Edge installed. Package security/metadata checks, native model-container
checks, Dart tests, sanitized policy tests and sandbox-paid native licence checks
pass. Production trust rejects sandbox keys. No production payment or
production customer-key test was performed.

Earlier physical capture tests covered the preceding Edge-backed edition.
Fresh physical validation of the independent native binaries, repeated camera
handover and the historical Android camera-buffer warning remain outstanding.
Successful linking is not proof of simultaneous camera behavior; keep one
scanner active at a time. Broader billing regression fixtures also require
follow-up independently of the passing Identity-specific checkout tests.

## Commercial access

Package publication does not enable live checkout automatically. Sandbox and
production Stripe products are configured separately. Production entitlement
must be issued by Ares Defence Labs; test keys are restricted to isolated QA
builds and are rejected by this release.

The SDK is proprietary, OCR/redaction is heuristic, and encryption cannot defeat
a host that controls its process. See [licensing](licensing.md) and
[security boundaries](security-and-assets.md).
