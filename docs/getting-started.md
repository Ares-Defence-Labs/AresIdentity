# Getting started

AresIdentity captures identity documents, locates sensitive fields with on-device OCR and returns redacted images. Android and iOS are supported. It does not authenticate documents or verify a person's identity.

## Availability and installation

The SDK is under development. Earlier private-source tests passed; a compiled customer candidate is undergoing fresh verification and has not been published. Do not substitute an unrelated pub.dev package or assume that a package name is available.

For authorized source development, use the private IdentitySDK checkout. Its nested shared core is included; the camera depends on the public protected ares_edge package, not the private Edge source checkout. In the host pubspec:

```yaml
dependencies:
  ares_identity:
    path: ../IdentitySDK/ares_identity
```

Adjust the relative path to your app. Run `flutter pub get` and fully rebuild the native application. Hot reload cannot replace native licence verification. The source edition requires Flutter 3.44+, Dart 3.12+; the example requires Dart 3.13.4+. Use the native setup in [platforms](platforms.md).

## Choose an integration

- [Scanner screen](scanner-screen.md): ready-made Flutter route with capture and torch controls.
- [Embedded camera](embedded-camera.md): camera and overlay under your own controls.
- [Recognition and redaction](recognition-and-redaction.md): process an upright encoded document image.

Provide your application's signed Identity licence from private build configuration. Missing/invalid keys use the 60-second process trial. See [licensing](licensing.md), including isolated sandbox testing.

The current Identity adapter requires a compatible AresEdge build with Identity licence protocol 2 and native authority ABI 1. The next compatible hosted release is pending; a pub.dev dependency alone cannot supply that unreleased capability today. Internal QA uses a guarded local override. Customer installation instructions will name the verified hosted versions when the release is approved.
