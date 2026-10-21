# Physical device verification — 21 October 2026

Status: **Android 4/4 passed; iPhone 4/4 passed**, using the earlier private source camera/processing edition. No SDK changes have been pushed or published during this verification.

## Scope

The private source edition uses the sibling Edge camera plugin and native licence verifier. Tests run real platform channels, the physical camera and ML Kit OCR. OCR fixtures are generated on the device with synthetic passport, driver licence and Visa test-card data. Captured camera bytes stay in memory; fixtures are deleted after each case. Logs record dimensions, region counts and outcomes, not camera images or OCR text.

## Android

Samsung Galaxy S24 (SM-S921B), Android 16 / API 36: **4/4 integration tests passed**.

- Real camera capture decoded at 1200 × 1600; 2,885,583 encoded bytes. Scanner disposal completed without a Flutter exception.
- Malformed Identity and Edge licence strings were rejected by the native Identity verifier.
- Native OCR automatically classified passport, driver licence and bank card fixtures. Respectively 15, 17 and 5 sensitive regions were found; existing-mask verification passed after redaction.
- Native 60-second camera trial expired. Remounting did not reset access, the embedded licence message appeared and the ready-made screen returned without capture.

## iOS

iPhone 13, iOS 26.7.1: **4/4 integration tests passed**.

- Real camera capture decoded at 1200 × 1600; 3,727,147 encoded bytes. Scanner disposal completed without a Flutter exception.
- Malformed Identity and Edge licence strings were rejected by the native Identity verifier.
- Native OCR automatically classified passport, driver licence and bank card fixtures. Respectively 10, 17 and 5 sensitive regions were found; existing-mask verification passed after redaction.
- Native 60-second camera trial expired. Remounting did not reset access, the embedded licence message appeared and the ready-made screen returned without capture.

Initial build exposed duplicate ONNX symbols when the source dependency graph used Swift Package Manager and CocoaPods together. The example now opts out of SPM locally and uses CocoaPods static framework linkage, with an explicit iOS 16 minimum. A clean rebuild also removed a stale ad-hoc signed ONNX framework left by the earlier mixed-manager build. Deep code-signature verification then passed and device installation succeeded. No global Flutter configuration or third-party runtime version was changed.

## Defects found and fixed

- Indexed PNG masking could write palette index zero (white in the iOS fixture) instead of black. Normalize decoded images to 8-bit RGB before filling masks. The new regression failed before the fix and passes after it; iOS native OCR/redaction now passes all three document types.
- Nonempty OCR rectangles one pixel wide or smaller were skipped. Mask every nonempty rectangle; the fractional-region regression failed before the fix and passes after it.
- Capture exceptions were silently swallowed. The scanner now sends a failure status to the host and emits only an error code/type in debug builds.
- The physical harness delivers real visibility callbacks immediately and retries early `NO_FRAME` responses within the unchanged native trial. No native channels or camera frames are mocked.

## Reproduction

From `ares_identity/example`, with the device unlocked and camera permission allowed:

```sh
flutter test integration_test/identity_device_test.dart -d <physical-device-id> --reporter expanded --timeout 5m

# iOS command-line debugger for this invocation only (Xcode 26+):
FLUTTER_LLDB_DEBUGGING=true flutter test integration_test/identity_device_test.dart -d <iphone-id> --reporter expanded --timeout 5m
```

Android minimum SDK is 24. iOS requires the host application's own signing team. The checked-in example signing team is the development team's identifier, not a signing credential.

Local evidence is under `ares_identity/build/physical-device-20261021/` (ignored): `android.log`, `android-results.json`, `ios.log`, `ios-results.json`, `source-sha256.json`, `sdk-analysis.log`, `sdk-tests.log`, and `example-widget-test.log`.

SDK static analysis reports no issues. The then-current 13 SDK/core tests and the example landing-page widget test pass.

## Sandbox entitlement follow-up

After granting the separate QA application's camera permission, both physical devices passed the additional sandbox test. Each checked four signed cases (valid Identity, expired Identity, future Identity and wrong-product Edge) using the native Identity verifier. Each waited for the real shared 60-second trial to expire, rechecked the valid sandbox Identity entitlement, then captured and decoded a 1200 × 1600 image. No native channels were mocked. These are locally signed sandbox fixtures; they do not represent a completed Stripe test payment.

Evidence: `build/physical-sandbox-20261021/` contains platform logs, result JSON and a test-source digest. No licence keys or captured images are included. Android's Flutter teardown reported an uninstall warning after the passing test; capture and assertions completed successfully.

The public camera adapter and compiled Identity processing implementation were changed after the original four-test runs. Their exact customer artifact still requires fresh device tests. The previous successful runs must not be treated as verification of that new artifact.

## Remaining release verification

These tests cover the private source edition and trial path. Protected customer artifacts, release-mode distribution, varied real document/camera conditions and broader device coverage remain separate release checks. The tests do not establish document authenticity or identity verification. SDK publication remains disabled.

## Protected customer artifact verification

The compiled customer candidate then passed **5/5 physical tests on each device**. Public Dart used the `ares_edge` camera; native Identity policies and masking came from a compiled AAR/XCFramework, with no Dart algorithm fallback.

| Check | Galaxy S24 / Android 16 | iPhone 13 / iOS 26.7.1 |
| --- | --- | --- |
| Real capture | 3060 × 4080 | 3024 × 4032 |
| Native passport/DL/card OCR analysis and masking | Passed | Passed |
| Real 60-second expiry, remount denied, screen closed | Passed | Passed |
| Sandbox four-case native policy enforcement after expiry | Passed | Passed |
| Valid sandbox OCR/masking and capture after expiry | Passed | Passed |

Native channels, OCR and camera frames were not mocked. Android's Flutter teardown attempted to uninstall the old source bundle identifier and emitted a cleanup warning after all assertions passed; the tested sandbox app used its separate QA identifier. Initial Android customer-host builds found namespace, obfuscated-class and AAR-attachment packaging defects; these were corrected before the successful device run.

Private evidence is in `build/physical-protected-20261021/`: platform logs/results, test-source digest and customer-package file hashes. Captured camera images and licence strings are absent. Production-pinned compiled binaries subsequently passed physical rejection checks on both devices: all four sandbox fixtures were rejected, ordinary trial processing worked, and analysis/masking were denied after the real trial expired. Varied real-world document accuracy, minimum-OS coverage, real Stripe sandbox checkout and coordinated hosted publication remain separate release gates.

## Actual Stripe sandbox-paid licence

A US$600 Stripe test payment then completed through the local staging portal, with automatic renewal off. The issued AID2 key expires on 21 October 2027. It was saved privately outside source control and separately verified offline: eight Swift/JVM checks accepted sandbox trust and rejected production trust across two application IDs, including product isolation, tampering and expiry. Network access was denied during those native probes.

The same paid sandbox key then passed the full **5/5 protected-package suite on both physical devices**, including valid native OCR/masking and real camera capture after the shared 60-second trial expired. Results were 3060 × 4080 on Galaxy S24 and 3024 × 4032 on iPhone 13. Earlier locally signed fixture evidence remains separate. No production customer key or real payment was tested.

Evidence: `build/physical-paid-sandbox-20261021/` contains token-free logs/results, test digest and offline-probe report. Actual keys, define files and camera images are excluded.

## AresScan customer-package compatibility — 21 October 2026

AresScan was analyzed locally against the compiled customer candidate and the compatible production-trusted Edge authority candidate. This found 40 compile errors hidden by the private-source dependency: duplicate enum identities and a private normalization-helper import. Identity consumer screens and the processing route now use public `ares_identity` types; the app bakes image orientation with its existing `image` dependency. The private owner compatibility layer retains its own legacy core types.

After the fix, targeted analysis of four Identity consumer files plus the generated route reported zero errors (existing style notices and one unused-widget warning remain). All four route, compatibility and scanner-guide tests passed. These tests do not exercise native channels and do not replace the physical-device results above or a combined native AresScan release build. Evidence: `build/aresscan-customer-validation-20261021/`. Temporary customer-package resolution was restored afterward. No package was published.


## AresScan owner integration — 21 October 2026

The full AresScan sandbox integration test passed on Galaxy S24 and iPhone 13.
Both used the real compiled native scanner/Identity channels and a separately
signed sandbox six-SDK ADLSA2 fixture, never the production owner key. Checks
covered owner acceptance, protected model/runtime initialization, rejection of
invalid owner/model inputs, the real shared trial reaching zero, licensed native
analysis/redaction afterward, and Redact/Analyse controls at 320×568 with 200%
text and a 100-pixel ad area. Android's active camera and native inference cadence
were independently observed. These full-app checks supplement the earlier five
protected-package capture/OCR tests per device; they do not claim broader real
document accuracy or minimum-OS coverage.

Android QA must use the existing App Bundle local-testing delivery flow. A plain
APK lacks the on-demand runtime/components and is not a valid delivery test.
The iOS pack contained an older ORT-format file under an ONNX filename; it now
uses the same approved ONNX bytes as Android (13,167,814 bytes, SHA256
`950070fd4a46e25d46db7c35d0ed77612d9063f70d4543c7c504f0da6a943a89`).
The private owner helper refreshes an old-format Apple cache once. Native
signature, model hash, encrypted-container and inference checks remain required.

AresScan uses an explicit Identity key when provided, otherwise its shared
six-product owner key. Customer-test builds clear that fallback. Model startup
precedes camera creation; failure offers a retry. Scanner controls and the guide
share measured available space above ads; large text uses a privacy dropdown.
The final 12 layout/configuration tests, nine SDK Dart tests, and 18 issuer tests
passed. PDF's shared-admin verifier also accepts the Identity claim while still
requiring its own product scope (12 compatibility tests passed).

Normal builds use the genuine private owner configuration; native licence QA
remains sandbox only. Local native dependency overrides are retained until the
coordinated hosted Edge version is available. Source/example publication metadata,
release configuration and the GitHub publication switch remain disabled. Nothing
was published to pub.dev. Documentation-only draft updates follow the successful
local physical verification; the new app/Edge integration implementation is local. Token-free
local evidence is in AresScan's ignored `build/identity-integration-20261021/`.


## Supplied route and consecutive captures — 21 October 2026

The SDK-owned route passed Strict Redact followed by Analyse on Galaxy S24 and
iPhone 13. Real native camera, crop, OCR and masking channels were used with a
sandbox six-product owner fixture. Both captures respected the configured
three-second countdown and returned `(level, bytes)` without an image editor.
Strict returned `strict` and decoded sensitive-region centre pixels were black;
Analyse returned `none`. Both routes reopened successfully on the same Navigator.

The iPhone evidence was collected after retiring a stale app process and verifying
a fresh process plus native startup diagnostics. A separate parser regression
covers all model-output orderings: precise corners and an explicit score take
priority over heatmap fallback. Native capture thresholds were unchanged. The
synthetic card needed to fill the guide; photographing a distant monitor introduced
moire and weak/rejected geometry. The samples are in [Synthetic fixtures](test-fixtures.md).

Diagnostic logging and analyzer images stayed local. Normal signed apps were
restored afterward. Source and customer-package publication remain disabled;
compiled core dependencies still require the coordinated protected Edge release.
The earlier Android buffer-acquisition warning remains a separately tracked
resize/teardown concern; this final two-route run did not reproduce it.
