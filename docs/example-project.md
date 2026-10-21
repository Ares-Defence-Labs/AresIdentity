# Example application

The approved local customer candidate includes `example/` for capture, on-device
sensitive-field analysis and redacted-image preview. The example app owns its
preview and save controls. The supplied SDK route returns cropped image bytes
and an applied-level enum without opening an editor.

## Run on a physical device

From the local customer package:

```sh
cd example
flutter pub get
flutter devices
flutter run -d <physical-device-id> --dart-define=ARES_IDENTITY_LICENCE=YOUR_PORTAL_ISSUED_KEY
```

Configure [platform permissions and signing](platforms.md), grant camera access
and use the candidate's verified native dependencies. For iOS, choose your host
app's development team and provisioning profile. Omitting a key uses the shared
native 60-second trial. Sandbox keys require an isolated sandbox-trusted candidate.

## Exercise both integrations

1. Open the [supplied scanner](scanner-screen.md), select a privacy level and
   capture the [synthetic bank card](test-fixtures.md).
2. Review the cropped pixels and returned `level`. Repeat in Analyse mode, which
   returns unmasked bytes and `none`.
3. Reopen the scanner; check torch, back navigation, privacy policies and countdown.
4. Use [embedded camera controls](embedded-camera.md) when composing your own UI
   and recognition/redaction flow.

## Verify your integration

Exercise permission denial/retry, background/foreground, disposal and repeated
scans. Keep the entire card large and in focus within the guide; monitor moiré can
interfere with detection. Review masked pixels before sharing. See
[performance and device checks](performance-testing.md) for a repeatable matrix.

The customer candidate is not a hosted release. Example results do not replace
final artifact verification or enable publication. See [release status](release-status.md).
