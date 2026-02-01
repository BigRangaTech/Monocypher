# Error Handling Plan

This fork uses explicit error codes to reduce misuse and make failures
actionable. This document captures the plan for improving error handling
over time.

Current state
-------------

- `crypto_err` enum provides stable error codes:
  `CRYPTO_ERR_NULL`, `CRYPTO_ERR_SIZE`, `CRYPTO_ERR_OVERFLOW`,
  `CRYPTO_ERR_AUTH`, `CRYPTO_ERR_CONFIG`.
- Checked wrappers validate pointers/sizes and return `crypto_err`.
- `crypto_random` returns `crypto_err` on failure.

Plan
----

1) **Complete checked coverage**
   - Audit remaining public APIs and add `*_checked` variants where missing.
   - Ensure auth failures consistently map to `CRYPTO_ERR_AUTH`.

2) **Error-string helper (optional)**
   - Add a tiny `crypto_strerror(int)` helper behind a build flag
     (e.g., `MONOCYPHER_STRERROR=1`) to avoid binary bloat for embedded users.
   - Keep strings stable and documented.

3) **Documentation alignment**
   - Update manpages and examples to prefer checked APIs for application code.
   - Document the error mapping for each checked wrapper in the API docs.

4) **RNG failure detail (optional)**
   - Consider a compile-time switch to expose more RNG error detail for
     debugging without changing the default API surface.

Policy
------

New public APIs should include a checked variant unless there is a clear
reason not to. Documentation should call out expected error codes.
