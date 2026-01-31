Fuzzing harnesses

LibFuzzer:
  clang -fsanitize=fuzzer,address,undefined -I src -I src/optional \
    tests/fuzz/fuzz_aead.c src/monocypher.c -o fuzz_aead

  clang -fsanitize=fuzzer,address,undefined -I src -I src/optional \
    tests/fuzz/fuzz_x25519.c src/monocypher.c -o fuzz_x25519

  clang -fsanitize=fuzzer,address,undefined -I src -I src/optional \
    tests/fuzz/fuzz_eddsa.c src/monocypher.c -o fuzz_eddsa

  clang -fsanitize=fuzzer,address,undefined -I src -I src/optional \
    tests/fuzz/fuzz_argon2.c src/monocypher.c -o fuzz_argon2

AFL:
  afl-clang-fast -I src -I src/optional tests/fuzz/afl_aead.c \
    src/monocypher.c -o afl_aead

  afl-clang-fast -I src -I src/optional tests/fuzz/afl_x25519.c \
    src/monocypher.c -o afl_x25519

  afl-clang-fast -I src -I src/optional tests/fuzz/afl_eddsa.c \
    src/monocypher.c -o afl_eddsa

  afl-clang-fast -I src -I src/optional tests/fuzz/afl_argon2.c \
    src/monocypher.c -o afl_argon2
