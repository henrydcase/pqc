# PQClean

**PQClean**, in short, is an effort to collect **clean** implementations of the post-quantum
schemes that are in the 
[NIST post-quantum project](https://csrc.nist.gov/projects/post-quantum-cryptography).
The goal of PQClean is to provide *standalone implementations* that 
* can easily be integrated into libraries such as [liboqs](https://openquantumsafe.org/#liboqs) or [libpqcrypto](https://libpqcrypto.org/);
* can efficiently upstream into higher-level protocol integration efforts such as [Open Quantum Safe](https://openquantumsafe.org/#integrations);
* can easily be integrated into benchmarking framworks such as [SUPERCOP](https://bench.cr.yp.to/supercop.shtml);
* can easily be integrated into framworks targeting embedded platforms such as [https://github.com/mupq/pqm4][pqm4];
* are suitable starting points for architecture-specific optimized implementations;
* are suitable starting points for evaluation of implementation security; and
* are suitable targets for formal verification.

What PQClean is **not** aiming for is
* a build system producing an integrated library of all schemes;
* including benchmarking of implementations; and
* including integration into higher-level applications or protocols.

As a first main target, we are collecting C implementations that fulfill the requirements
listed below. 

## Requirements on C implementations that are automatically checked

* Code is valid C99
* Passes functional tests
* API functions do not write outside provided buffers
* Compiles with `-Wall -Wextra -Wpedantic -Werror` with `gcc` and `clang`
* Consistent test vectors across runs
* Consistent test vectors on big-endian and little-endian machines
* Consistent test vectors on 32-bit and 64-bit machines
* No errors/warnings reported by valgrind
* No errors/warnings reported by address sanitizer
* Only dependencies:
  * `common_crypto.c` (Keccak, AES, SHA-2)
  * `randombytes.c`
* API functions return 0 on success, negative on failure
* No dynamic memory allocations
* No branching on secret data (dynamically checked using valgrind)
* No access to secret memory locations (dynamically checked using valgrind)
* Separate subdirectories (without symlinks) for each parameter set of each scheme
* Builds under Linux, Mac OSX, and Windows
* Makefile-based build for each separate scheme
* Makefile-based build for Windows (nmake)
* All exported symbols are namespaced with `PQCLEAN_SCHEMENAME_`
* Each implementation comes with a LICENSE file (see below)
* Each implementation comes with a META file giving details about version of the algorithm, designers, etc.


## Requirements on C implementations that are manually checked

* Makefiles without explicit rules (rely on implicit, built-in rules)
* #ifdefs only for header encapsulation
* No stringification macros
* Output-parameter pointers in functions are on the left
* const arguments are labelled as const
* All exported symbols are namespaced inplace
* All integer types are of fixed size, using `stdint.h` types (including `uint8_t` instead of `"unsigned char`)
* Integers used for indexing are of size `size_t`
* variable declarations at the beginning (except in `for (size_t i=...`)


## Clean C implementations currently in PQClean

Currently, the continuous-integration and testing environment of PQClean is still work in progress 
and as a consequence PQClean does not yet have any implementations.

<!--
 Currently, PQClean includes clean C implementations of the following KEMs:

 * [Kyber-512](https://pq-crystals.org/kyber/)
 * [Kyber-768](https://pq-crystals.org/kyber/)
 * [Kyber-1024](https://pq-crystals.org/kyber/)

 Currently, PQClean includes clean C implementations of the following signature schemes:

 * [Dilithium-III](https://pq-crystals.org/dilithium/)
-->

## API used by PQClean

PQClean is essentially using the same API as required for the NIST reference implementations, 
which is also used by SUPERCOP and by libpqcrypto. The only two differences to that API are
the following:
* All lengths are passed as type `size_t` instead of `unsigned long long`; and
* Signatures offer two additional functions that follow the "traditional" approach used
in most software stacks of computing and verifying signatures instead of producing and
recovering signed messages. Specifically, those functions have the following name and signature:

```
int crypto_sign_signature(uint8_t *sig, size_t *sigbytes, const uint8_t *m, size_t mlen, const uint8_t *sk);
int crypto_sign_verify(const uint8_t *sig, size_t sigbytes, const uint8_t *m, size_t mlen, const uint8_t *pk);
```

## License

Each subdirectory containing implementations contains a LICENSE file stating under what license
that specific implementation is released. All other code for testing etc. in this repository
is released under the conditions of [CC0](http://creativecommons.org/publicdomain/zero/1.0/).

