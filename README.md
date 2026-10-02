[![Verification of extendedGCD](https://github.com/arquintl/go-gcd/actions/workflows/verify-gcd.yml/badge.svg?branch=master)](https://github.com/arquintl/go-gcd/actions/workflows/verify-gcd.yml?query=branch%3Amaster)
[![Test of extendedGCD](https://github.com/arquintl/go-gcd/actions/workflows/test-gcd.yml/badge.svg?branch=master)](https://github.com/arquintl/go-gcd/actions/workflows/test-gcd.yml?query=branch%3Amaster)

# Verification of `extendedGCD`

This repository is a fork of the [Go standard library](https://github.com/golang/go).

We have successfully applied the [Gobra program verifier](https://gobra.ethz.ch) to verify the `InverseVarTime`, `GCDVarTime`, and `extendedGCD` functions in the [`crypto/internal/fips140/bigmod`](https://github.com/arquintl/go-gcd/tree/master/src/crypto/internal/fips140/bigmod) package.
More specifically, Gobra takes the following three files as input and verifies them:
- [nat.go](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/nat.go) contains the implementation of arbitrary-length natural numbers and the three functions mentioned above.
- [nat-spec.gobra](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/nat-spec.gobra) contains ghost code such as predicate definitions and lemmata.
- [sync_flag.go](https://github.com/arquintl/go-gcd/blob/master/src/crypto/internal/fips140/bigmod/sync_flag.go) defines the `UseSynchronizedWrappingInExtendedGCD` global boolean variable. If this variable is set to true `extendedGCD` uses our proposed fix instead of the existing implementation that deviates from BoringSSL and Fiat Cryptography. Since this variable may be modified at runtime, Gobra proves the implementation's correctness under both potential values for this variable and, thus, the proof covers both the existing and fixed implementation. By now, our proposed implementation has been merged into Go's standard library as commit [`856af77`](https://github.com/golang/go/commit/856af779c97aa926474d7a9d5496ea703ff2a51f).
