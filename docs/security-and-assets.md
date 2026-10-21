# Model, asset and distribution security

## Shared encrypted camera model

AresIdentity uses the protected AresEdge camera and its bundled model. It does
not ship a second ONNX model, a separate model URL or a public decryption API.
The approved Edge customer artifact uses authenticated AES-256-GCM `.aem`
containers. Android reads decrypted model bytes into a direct memory buffer.
The current iOS ONNX Runtime interface requires a protected temporary file,
which the native loader removes after session creation or a loading error.
Corrupted/truncated containers and cleartext model replacements are rejected.

The encryption key is embedded in compiled native code for offline use. Someone
controlling the host process can inspect or patch the binary, recover a key or
inspect decrypted memory. This raises the cost of ordinary archive extraction;
it does not make the model impossible to extract. Hash receipts detect changes
relative to a reviewed snapshot; they are not publisher signatures.

AresScan's owner/store-delivery configuration is separate. Do not copy its raw
store assets, owner entitlement or private model-delivery configuration into a
customer package. Customer builds inherit the protected Edge resource.

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

Local assembly requires hashed Android/iOS Identity binaries and the exact
protected Edge artifact used for review. Auditing rejects plaintext model files,
including inside nested AAR/JAR archives; unexpected implementation source;
private helpers and paths; credential-shaped material; unsafe archive paths;
symlinks; extra/unreceipted files; and changed reviewed documentation assets.
Identity rejects even an encrypted model copy of its own: it must inherit Edge's.

Local checks do not assert that an unreleased hosted dependency exists. The
current Edge review artifact still identifies itself as 2.1.1; Identity needs the
coordinated compatible release (currently targeted at 2.1.2). Its version mismatch
is recorded explicitly. Rebuild/audit the final dependency and repeat physical
checks against the exact final artifacts before approving any release.

## Document privacy

OCR is on device; field detection is heuristic and does not establish identity
or document authenticity. Use the exact upright pixels for OCR and masking.
Review the result before export. The host app owns consent, original-image
storage, retention, transport and access controls. Removing temporary inputs or
re-encoding a redacted image cannot erase copies already retained elsewhere.

Report suspected vulnerabilities privately using [SECURITY.md](../SECURITY.md).
