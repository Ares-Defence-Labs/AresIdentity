# Model, asset and distribution security

## Independent encrypted camera model

AresIdentity owns its camera, native licence verifier and model loader. It has no
runtime `ares_edge` dependency and uses the same authenticated AES-256-GCM design
as AresEdge. The private source model is `ares_identity_inferencer.onnx`.
Customer Android and iOS artifacts each contain one encrypted
`ares_identity_inferencer.aem`, never the raw ONNX file or a public decryption API.
Android loads decrypted weights through a direct memory buffer. The current iOS
ONNX Runtime interface requires a protected temporary file, removed after session
creation or a loading error. Corrupted/truncated containers and cleartext model
replacements are rejected.

The encryption key is embedded in compiled native code for offline use. Someone
controlling the host process can inspect or patch the binary, recover a key or
inspect decrypted memory. This raises the cost of ordinary archive extraction;
it does not make the model impossible to extract. Hash receipts detect changes
relative to a reviewed snapshot; they are not publisher signatures.

AresScan's owner/store-delivery configuration is separate. Do not copy its raw
store assets, owner entitlement or private model-delivery configuration into a
customer package. Customer builds carry the Identity encrypted resource.

## Identity processing and licensing

The public Dart layer contains the supported API, models and thin adapters.
Sensitive-field policies and masking run in compiled Android/iOS code. Private
helper algorithms, issuer tools, licence tokens, test trust, debug mappings and
source-only dependencies are excluded from customer artifacts. Native product
access is checked before processing and returning licensed results. Public APIs
cannot replace issuer trust. Sandbox keys belong only in isolated test builds;
production trust must reject them.

## Public documentation assets

The banner, store badges, screenshots and synthetic fixtures are intentionally
readable documentation assets. They are not runtime model weights or secrets.
They are inventoried with SHA-256 hashes and included in the whole-package
receipt. Neither `pubspec.yaml` nor the Identity plugin registers these images
as Flutter runtime assets. Screenshot metadata only advertises the images on the
package page. The supplied screenshot of an editor shows the AresScan host app;
that editor is not part of AresIdentity.

## Customer package checks

Standalone assembly requires hashed Identity camera and processing binaries.
Auditing rejects raw model files, including inside nested AAR/JAR archives;
unexpected implementation source; private helpers and paths; credential-shaped
material; unsafe archive paths; symlinks; extra/unreceipted files; and changed
reviewed documentation assets. Exactly one correctly named encrypted model per
platform is required. No `ares_edge` package dependency or imports are permitted.

The standalone migration changes the native artifacts. Earlier Edge-backed
physical results remain historical evidence and do not validate these new
binaries. Recheck authenticated readers, native licence isolation, host linking,
camera capture and package contents before publishing the independent SDK.

## Document privacy

OCR is on device; field detection is heuristic and does not establish identity
or document authenticity. Use the exact upright pixels for OCR and masking.
Review the result before export. The host app owns consent, original-image
storage, retention, transport and access controls. Removing temporary inputs or
re-encoding a redacted image cannot erase copies already retained elsewhere.

Report suspected vulnerabilities privately using [SECURITY.md](../SECURITY.md).
