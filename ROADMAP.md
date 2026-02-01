# Roadmap

This document captures forward-looking work for this fork.

Priority legend: (Near) next, (Mid) medium-term, (Long) longer-term.

Project policy: any new or modified functionality must include strong error
handling and tests that exercise both success paths and error paths. Keep
documentation updated alongside code changes. Everything must remain open
source.

Security and diagnostics
------------------------

- (Mid) Thread-safe RNG diagnostics (TLS-backed `crypto_random_last_error()`).
  - Switch stored error to thread-local storage with a no-TLS fallback.
  - Document semantics, lifetime, and thread-safety expectations.
  - Add tests under `RNG_DIAGNOSTICS=1`.
- (Near) Add `*_checked` wrappers for `crypto_wipe` and `crypto_random` to keep
  the checked API surface uniform.
  - Define signatures and error mappings.
  - Update docs/examples to prefer checked variants.
  - Add tests for NULL and size edge cases.
- (Near) Improve error discoverability (consistent error paths, unified
  diagnostics, and easy-to-find error reporting patterns).
  - Define a unified error reporting pattern (last-error + strerror + guidance).
  - Ensure every checked API maps failures consistently.
  - Add a single doc section that explains how to find errors quickly.
- (Near) Side-channel hardening: add `ctgrind`/`valgrind`/constant-time checks
  in CI, and document the results.
  - Add CI jobs with stored reports.
  - Add constant-time tests for key primitives.
  - Record results and known limitations in docs.
- (Mid) Constant-time verification suite integration (`dudect`, `ctgrind`)
  with sample configs and expected baselines.
  - Add `dudect` harnesses for high-risk operations.
  - Track baselines and variance thresholds.
  - Provide a local runner script.
- (Mid) Hardware RNG policy (document stance on `RDRAND`/`RDSEED` and optional
  support).
  - Decide default policy and build-time opt-in.
  - Document risks and rationale.
  - Add tests that ensure default behavior is unchanged.
- (Mid) Key erasure auditing (automated checks to ensure sensitive buffers are
  wiped).
  - Add lint/grep checks for wipe coverage in sensitive paths.
  - Add tests that verify wipes are not optimized out.
  - Document guidelines for new code.
- (Long) Memory-safety hardening (optional hardened allocators or
  `-fstack-clash-protection` where available).
  - Evaluate allocator options and platform support.
  - Add optional build flags and documentation.
  - Measure overhead and document tradeoffs.
- (Near) DoS/resource limits guidance for Argon2 parameters and input sizes.
  - Publish recommended limits for interactive/server use.
  - Add examples with safe defaults.
  - Document how to tune limits per platform.

Static analysis and verification
--------------------------------

- (Near) Add CI jobs for `clang-tidy`, `cppcheck`, and `scan-build`.
  - Add baseline configs and suppressions.
  - Wire into CI with artifacts.
  - Document how to run locally.

Testing and fuzzing
-------------------

- (Mid) Structure-aware fuzzers and dictionaries for complex inputs.
  - Add dictionaries for AEAD/Argon2/EdDSA formats.
  - Add structure-aware mutators where helpful.
  - Track coverage improvements.
- (Mid) Long-run fuzzing with corpus minimization and periodic refresh.
  - Add a scheduled fuzz job with time budgets.
  - Automate corpus minimization.
  - Refresh corpora on significant changes.
- (Near) CI fuzzing jobs for regression detection.
  - Run short fuzz jobs per PR.
  - Keep crashing inputs as artifacts.
  - Gate merges on new crashes.
- (Near) Expand and document test vectors (SHA-256, BLAKE3, HKDF, Ed25519ph).
  - Add reference vectors and provenance.
  - Update vector loader documentation.
  - Add regression tests for each vector set.
- (Mid) Interoperability test vectors against other libraries (libsodium,
  BoringSSL).
  - Add cross-library vector comparisons.
  - Document toolchain requirements.
  - Gate on expected output equivalence.

Benchmarks
----------

- (Mid) Deterministic micro-benchmark harness and baseline performance tables
  per platform/CPU.
  - Add a small benchmark binary with fixed inputs.
  - Capture baseline tables for major CPUs.
  - Add regression thresholds for CI (optional).

Build and release integrity
---------------------------

- (Mid) Reproducible builds documentation and CI verification.
  - Document required flags and environment.
  - Add CI job to compare build artifacts.
  - Record variance sources and mitigations.
- (Mid) SBOM generation and signed release artifacts.
  - Add SBOM generator tooling.
  - Sign release artifacts and document verification.
  - Store signatures alongside releases.
- (Near) Release automation (scripted checklist, version bump helper, tag
  signing).
  - Add scripts for version bumps and changelog updates.
  - Automate tag creation and signing.
  - Document a minimal release checklist.
- (Near) Documentation CI (fail on doc generation warnings and dead refs).
  - Run `doc/doc_gen.sh` and doc checks in CI.
  - Fail on broken references or warnings.
  - Keep docs artifacts for inspection.

Platform and build coverage
---------------------------

- (Mid) Windows and macOS CI builds.
  - Build and run tests on each OS.
  - Record supported toolchains.
  - Document platform-specific caveats.
- (Mid) MSVC / clang-cl build support and documentation.
  - Add build instructions and flags.
  - Fix portability warnings.
  - Add CI coverage for MSVC.
- (Mid) Architecture matrix builds (x86_64/arm64) with QEMU for cross-arch
  tests.
  - Add QEMU runners and build matrix.
  - Run tests cross-arch.
  - Document emulation limitations.

API and packaging polish
------------------------

- (Mid) Versioned symbols / SONAME handling review.
  - Audit exported symbols and version scripts.
  - Align SONAME policy with ABI changes.
  - Document compatibility guarantees.
- (Near) `pkg-config` sanity checks for install paths.
  - Add CI check for installed `.pc` files.
  - Validate include/lib paths on install.
  - Document overrides for custom prefixes.
- (Near) Stricter compile warnings and optional `-Werror` profile.
  - Add a `STRICT=1` profile.
  - Fix or suppress known warnings.
  - Document expected toolchain versions.
- (Mid) ABI stability policy and `MONOCYPHER_VERSION_*` macros.
  - Define ABI policy and update headers.
  - Add version macros and docs.
  - Add a compatibility check note for downstreams.
- (Near) Memory-allocation policy and stack-usage documentation.
  - Document zero-allocation guarantee.
  - Note stack usage for key APIs.
  - Add guidance for constrained targets.
- (Mid) Packaging targets (Debian, Arch, Homebrew) and release checklist.
  - Add packaging docs/templates.
  - Add a release checklist section.
  - Validate install paths with CI.
- (Long) Optional API misuse lints / helpers.
  - Add compile-time helper macros.
  - Document recommended usage patterns.
  - Evaluate false-positive rates.
- (Near) Compliance notes (document non-FIPS status or a compliance roadmap).
  - Publish a compliance stance.
  - Document any required work for certifications.
- (Near) Threat model document (what is in/out of scope for this fork).
  - Define adversary model and assumptions.
  - List out-of-scope attack classes.
  - Link to recommended usage guidance.
- (Mid) Threat-model-driven API review (identify risky APIs and safe defaults).
  - Identify APIs that commonly lead to misuse.
  - Add safe-default guidance or wrappers.
  - Document deprecations if needed.
- (Near) Key management guidance (rotation, storage, lifecycle).
  - Add practical guidance and examples.
  - Include key erasure practices.
  - Link to external references.
- (Mid) Public API deprecation policy (timeline + versioning rules).
  - Define deprecation timeline and notices.
  - Document migration paths.
  - Track deprecations in changelog.

Bindings and ecosystem
----------------------

- (Long) Roadmap for Rust/C++/Python bindings with CI coverage.
  - Define supported language versions.
  - Outline CI strategy and release cadence.
  - Identify maintainers/owners.
- (Mid) Rust API crate (safe/unsafe layers, feature flags, and CI).
  - Define crate structure and feature flags.
  - Add CI for `std`/`no_std`.
  - Publish safety policy for unsafe blocks.
- (Long) Non-C bindings policy (stability guarantees, safety rules).
  - Define API stability for bindings.
  - Document safety rules and review expectations.
  - Require CI coverage for binding changes.
- (Mid) FFI stability tests for ABI/struct layout compatibility.
  - Add ABI snapshot tests.
  - Gate on layout changes.
  - Document compatibility constraints.
- (Mid) Language-specific safe wrappers (Rust `no_std`, C++ RAII, Python wheels).
  - Add RAII wrappers for C++.
  - Add `no_std` support for Rust.
  - Build wheels for Python.
- (Mid) Registry release automation (crates.io, PyPI, npm).
  - Add release scripts per registry.
  - Automate version syncs.
  - Document publish steps.
- (Mid) CMake/Meson build files for downstreams and bindings.
  - Provide minimal build files.
  - Ensure consistent feature flags.
  - Document integration examples.
- (Mid) Interop test suites per binding (validate against C vectors).
  - Run vector tests in each binding.
  - Keep vectors in sync with C.
  - Gate on test parity.
- (Mid) Version-compat matrix for bindings vs core releases.
  - Publish a compatibility table.
  - Update on each release.
  - Add policy for breaking changes.
- (Mid) Binding security policy (memory safety guidelines, unsafe reviews).
  - Define review steps for unsafe code.
  - Require threat model notes for bindings.
  - Add security contact points.
- (Near) Minimal examples/tutorials per binding.
  - Add quickstart examples.
  - Include safe usage notes.
  - Keep examples tested in CI.
