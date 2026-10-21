# Android and iOS setup

| Requirement | Android source example | iOS source example |
| --- | --- | --- |
| Minimum OS | API 24 | iOS 16 |
| Camera | Declare and grant CAMERA | NSCameraUsageDescription and runtime permission |
| Signing | Host application signing | Your development/distribution team and profiles |
| OCR | ML Kit Latin recognizer | ML Kit Latin recognizer |

Android manifest:

```xml
<uses-permission android:name="android.permission.CAMERA" />
```

iOS Info.plist:

```xml
<key>NSCameraUsageDescription</key>
<string>Capture identity documents for local review and redaction.</string>
```

The source example also declares NSPhotoLibraryUsageDescription for host gallery dependencies. Include that permission description if your own UI accesses the photo library.

## Source-edition native dependencies

The private example currently uses CocoaPods static framework linkage (`use_frameworks! :linkage => :static`) with an explicit iOS 16 platform. Its local Flutter configuration sets `enable-swift-package-manager: false`. This prevents the source dependency graph from linking ONNX Runtime twice. Changing dependency managers requires a clean rebuild to remove stale embedded frameworks. Do not apply this workaround globally to other projects. The new shared native candidate also supports Swift Package Manager. Follow its release-specific setup; avoid a second ONNX Runtime supplied by another plugin. Customer release remains gated on exact-artifact verification.

The protected shared AresEdge camera supplies its native model resources. Do not add a second camera plugin registration or a second copy of the model. AresScan's store-delivered model policy is separate from the SDK example's packaged resources.

Physical QA passed on Galaxy S24/Android 16 and iPhone 13/iOS 26.7.1. This is not a complete minimum-OS/device compatibility matrix. Current Google ML Kit dependencies also warn about iOS 26 Apple Silicon simulator architecture support; use physical devices for this verified workflow.

The compiled Identity customer candidate currently registers through CocoaPods on iOS and a compiled AAR on Android. It adds no second camera or ONNX Runtime. Swift Package Manager support in the shared Edge package does not imply SPM support for the Identity adapter; use the documented CocoaPods integration for the current Identity candidate.
