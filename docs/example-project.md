# Runnable example and verification

The private SDK contains an example app under example/. It opens the ready-made scanner, displays captured pixels, analyses sensitive fields and previews a redacted image. Its image processing is local. Supply a development signing team for iOS and grant camera access.

```sh
cd example
flutter pub get
flutter run -d <physical-device-id>
```

From the SDK root:

```sh
flutter analyze
flutter test test packages/ares_identity_core/test
cd example
flutter test test/widget_test.dart
flutter test integration_test/identity_device_test.dart -d <physical-device-id> --timeout 5m
```

The integration harness uses physical camera/platform channels and on-device OCR with generated synthetic documents. It checks capture/disposal, malformed key rejection, classification, black masking and the real 60-second trial. It deletes synthetic OCR files and reports only dimensions/counts/states. It does not issue a valid customer licence or validate a paid checkout.

On 21 October 2026 the earlier source example passed 4/4 physical tests on Galaxy S24 and 4/4 on iPhone 13, with its then-current SDK/core regressions. Subsequent host-integration or native-binary changes require their own verification. See [performance and testing](performance-testing.md).
