# Roadmap

This document captures forward-looking work for this fork.

Security and diagnostics
------------------------

- Thread-safe RNG diagnostics (TLS-backed `crypto_random_last_error()`).
- Add `*_checked` wrappers for `crypto_wipe` and `crypto_random` to keep the
  checked API surface uniform.
- Side-channel hardening: add `ctgrind`/`valgrind`/constant-time checks in CI,
  and document the results.

Testing and fuzzing
-------------------

- Structure-aware fuzzers and dictionaries for complex inputs.
- Long-run fuzzing with corpus minimization and periodic refresh.
- CI fuzzing jobs for regression detection.

Platform and build coverage
---------------------------

- Windows and macOS CI builds.
- MSVC / clang-cl build support and documentation.

API and packaging polish
------------------------

- Versioned symbols / SONAME handling review.
- `pkg-config` sanity checks for install paths.
- Stricter compile warnings and optional `-Werror` profile.
