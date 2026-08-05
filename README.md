[![Verification of extendedGCD](https://github.com/arquintl/go-gcd/actions/workflows/verify-gcd.yml/badge.svg?branch=master)](https://github.com/arquintl/go-gcd/actions/workflows/verify-gcd.yml?query=branch%3Amaster)
[![Test of extendedGCD](https://github.com/arquintl/go-gcd/actions/workflows/test-gcd.yml/badge.svg?branch=master)](https://github.com/arquintl/go-gcd/actions/workflows/test-gcd.yml?query=branch%3Amaster)

# Verification of `extendedGCD`

This repository is a fork of the [Go standard library](https://github.com/golang/go).

We have successfully applied the [Gobra program verifier](https://gobra.ethz.ch) to verify the `InverseVarTime`, `GCDVarTime`, and `extendedGCD` functions in the [`crypto/internal/fips140/bigmod`](https://github.com/arquintl/go-gcd/tree/master/src/crypto/internal/fips140/bigmod) package.
More specifically, Gobra takes the following three files as input and verifies them:
- [nat.go](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/nat.go) contains the implementation of arbitrary-length natural numbers and the three functions mentioned above.
- [nat-spec.gobra](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/nat-spec.gobra) contains ghost code such as predicate definitions and lemmata.
- [sync_flag_fixed.go](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/sync_flag_fixed.go) defines the `UseSynchronizedWrappingInExtendedGCD` constant to be `true`, such that `extendedGCD` uses our proposed and verified fix instead of the original implementation that deviates from BoringSSL and Fiat Cryptography. Our proposed implementation has been merged into the Go standard library as commit [`856af77`](https://github.com/golang/go/commit/856af779c97aa926474d7a9d5496ea703ff2a51f).

In addition, branch [`legacy-impl-proof`](https://github.com/ArquintL/go-gcd/tree/legacy-impl-proof) contains a proof that covers both, the fixed and the legacy implementations.
