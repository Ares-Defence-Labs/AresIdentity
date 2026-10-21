# Licensing, renewal and sandbox verification

AresIdentity is configured at **US$600 per twelve-month term**, equivalent to US$50/month billed annually, matching AresEdge. A signed customer licence covers the customer's apps on supported Android/iOS platforms. Stripe live and test product records exist; public package availability and live checkout remain release-gated.

## Configure your application

```dart
const identityLicence = String.fromEnvironment('ARES_IDENTITY_LICENCE');
// Pass identityLicence to AresIdentityScreen or IdentityIntelligenceScanStreamer.
```

AresScan's private integration uses the separate define ARES_IDENTITY_SDK_LICENCE. Do not pass an Edge/PDF/Office key or a portal licence ID. Pass the complete Identity token. It uses prefix AID2 and signed product claim AresIdentitySDK. Never implement an end-user payment/key-entry requirement for using your app: developers configure the SDK entitlement in their application/build system.

Native code verifies the issuer signature, supported product, application/customer scope and twelve-month validity. Editing a claim invalidates the signature. No issuer private key belongs in your application or repository. Customer analysis, masking and camera access are enforced natively. The public Dart layer serializes input and decodes typed results; it contains no policy or masking fallback. Legacy Dart helpers remain only in the private owner compatibility package and are excluded from customer artifacts.

## Trial and renewal

Missing, malformed, wrong-product or expired keys fall back to a shared 60-second native trial shared by camera and processing per application process. Remounting or navigating does not reset it. After expiry the embedded camera is replaced by a white licence-required message; the ready-made route attempts to return without capture. Restarting the app starts a new process trial.

Renewal issues a replacement signed key. Ship it in updates before the old term ends; installed applications do not automatically acquire a new key. Payment/renewal configuration and licence validity are separate from app-store billing and your own end-user subscription.

## Sandbox-only licence tests

All tested keys for this project must be sandbox keys, as requested by the owner. Use Stripe test payments and the independent staging issuer. Production configuration retains production trust and must reject sandbox signatures. No real payment or production customer key is needed to test the sandbox workflow.

A sandbox key works only in a clearly isolated sandbox-trusted QA build. Prepare that copy outside the release checkout, use a distinct application ID/label, keep publish_to:none and publication disabled, and verify the release pins are unchanged. Never add a runtime test-key bypass to production binaries. Never publish sandbox-trusted artifacts.

A locally signed sandbox fixture can test cryptographic/date behavior without payment, but it does not prove checkout/webhook issuance. A sandbox-paid licence additionally verifies the test payment → signed webhook → paid term → AID2 issuance path. Report those two evidence types separately.
