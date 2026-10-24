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

The supplied gallery button and embedded gallery API require an explanatory `NSPhotoLibraryUsageDescription` in the iOS host Info.plist. The picker requests no full metadata. Android uses the system photo picker without broad storage permissions. Rebuild native hosts after adding the picker dependency.

## Source-edition native dependencies

The private example currently uses CocoaPods static framework linkage (`use_frameworks! :linkage => :static`) with an explicit iOS 16 platform. Its local Flutter configuration sets `enable-swift-package-manager: false`. This prevents the source dependency graph from linking ONNX Runtime twice. Changing dependency managers requires a clean rebuild to remove stale embedded frameworks. Do not apply this workaround globally to other projects. Avoid a second ONNX Runtime supplied by another plugin. Native binary changes require a full rebuild.

AresIdentity supplies its own compiled camera and encrypted `ares_identity_inferencer.aem` model. No `ares_edge` installation is required. Do not manually register a second camera or add raw ONNX weights to app assets. AresScan's owner/store-delivery policy is separate.

Physical QA passed on Galaxy S24/Android 16 and iPhone 13/iOS 26.7.1. This is not a complete minimum-OS/device compatibility matrix. Current Google ML Kit dependencies also warn about iOS 26 Apple Silicon simulator architecture support; use physical devices for this verified workflow.

The compiled Identity customer candidate currently registers through CocoaPods on iOS and a compiled AAR on Android. It includes the Identity camera and depends on ONNX Runtime through CocoaPods, pinned to the reviewed version. Use the documented CocoaPods integration for the current Identity candidate. Coexistence with other scanner SDKs requires one compatible ONNX Runtime and one active camera at a time.

## Android 16 KiB pages

The v2.0.5 candidate's processing library is linked with both 16 KiB maximum and
common page alignment. Release checks inspect all arm64-v8a and x86_64 native
libraries in the complete customer APK, including Flutter, ONNX Runtime and ML Kit,
and verify native ZIP alignment. The ARM64 candidate has also passed native loading,
synthetic-card OCR/redaction and encrypted document-model inference on an Android 15
emulator reporting `PAGE_SIZE=16384`, with compatibility fallback disabled and native
binary hashes matched to the audited customer APK.

Use Android Gradle Plugin 8.5.1 or newer when packaging uncompressed native
libraries. Your host application and any additional plugins must also support
16 KiB pages. Follow [Android's page-size guidance](https://developer.android.com/guide/practices/page-sizes)
and test your final app on a 16 KiB environment; SDK verification cannot cover other
native libraries that your application adds. This candidate validation does not
change the already published v2.0.5 archive.
