# Release status and verification gates

Status on 21 October 2026: private source SDK in development. No public ares_identity customer package has been published. The compiled protected customer candidate passed 5/5 physical tests on Galaxy S24 and 5/5 on iPhone 13, including native OCR/redaction and sandbox-licensed processing/capture after trial expiry. Three AresScan integration tests, twelve sanitized native policy cases and nineteen website documentation/link/catalogue tests also pass. A real US$600 Stripe sandbox purchase (renewal off) subsequently issued a key that passed eight offline native checks and the five-test physical suite on each device. The coordinated hosted Edge release remains gated.

SDK publication is disabled in source package metadata, repository variables and release configuration. Changelog/branch/tag governance follows the Edge concept. Public documentation can describe implemented APIs, but must not claim a package version or downloadable artifact exists before it is verified.

Before enabling sales/publication: resolve private path dependencies into supported customer artifacts; verify native Identity product enforcement and sandbox-key rejection by release trust; run device tests against the exact artifact; validate docs examples and notices; check dry-run package contents for source/secrets/models policy; pass both repository checks and review the release. Live paid-licence verification is not claimed when only sandbox payments were used.


AresScan owner integration also passed on both physical devices with a sandbox
six-SDK admin fixture, including protected model initialization, real trial expiry,
native processing and compact/ad controls. Its final 12 layout/configuration tests
and nine SDK Dart tests pass. See `physical-device-testing.md` for the separate
full-app evidence and model-delivery corrections. Normal owner configuration is
issued, while all native licence QA remains sandbox-only. Publication stays disabled.
