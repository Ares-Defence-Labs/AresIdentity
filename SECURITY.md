# AresIdentity security and privacy

Report suspected vulnerabilities privately to team@aresdefencelabs.com. Include
affected artifact version, platform, reproduction and impact using synthetic
inputs. Do not post licence strings, identity documents, raw OCR, issuer keys,
owner entitlements, symbols or exploitable bypasses in public issues.

## Offline trust

Native code verifies signed product, scope and dates using pinned issuer trust.
Sandbox trust stays isolated from production. Missing or invalid access receives
the shared native process trial; remounting a scanner does not extend it. Offline
licensing cannot provide immediate revocation or establish trusted UTC after a
host-controlled restart. No public API can replace an issuer pin.

## Protected artifacts

AresIdentity owns its authenticated encrypted camera model and compiled scanner.
Its protection follows the same design as AresEdge, without an `ares_edge`
dependency. Private source uses `ares_identity_inferencer.onnx`; distribution
uses `ares_identity_inferencer.aem`. Compiled processing and thin registration
adapters keep private algorithms out of the customer source. Android uses an in-memory model
buffer; the current iOS loader removes its protected staging file after session
creation/error. Encryption and stripping resist casual extraction; a host that
controls the process can still inspect or patch binaries and memory.

Public branding, screenshots and synthetic fixtures remain readable and are
inventoried separately from runtime model assets. Hash receipts detect changes
relative to the reviewed files, but are not signatures or proof of provenance.
See [model and asset security](docs/security-and-assets.md) for packaging controls,
limitations and standalone release verification gates.

## Document privacy

OCR/classification is heuristic and does not establish authenticity or complete
sensitive-field discovery. The host owns consent, access control, storage,
retention, transport and export review. Solid redaction returns newly encoded
pixels; protect/delete originals and temporary files separately. Never log raw
identity data or complete licence tokens.
