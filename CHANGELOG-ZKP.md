# Changelog (ZKP)

All notable changes to the ZKP modules and related packaging in this fork will be documented in this file.

The format matches [CHANGELOG.md](./CHANGELOG.md) and is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
Package and library versions omit the `-zkp` suffix (for example, tag `v0.8.1-zkp` corresponds to package version `0.8.1`); the suffix appears only in release tags and in this changelog.

## [0.8.1-zkp] - 2026-09-11

#### Fixed
 - ZKP modules: Fixed Autotools/CMake CI failures across Windows, shared-library symbol checks, sanitizers (ASan/UBSan/MSan), Valgrind, big-endian, and related build/link issues after the `v0.8.0-zkp` cut.
 - build: Restored Windows scratch-space symbol export (`SECP256K1_API` / `extern "C"`) and fixed C++ linkage against the scratch API.
 - build: Silenced `-Werror` issues (MSVC C4334, trailing whitespace) and fixed mingw static bench links via `SECP256K1_STATIC`.
 - Module `bulletproofs`: Fixed undefined behavior in test/helper shifts at `nbits == 64`, signed overflow in random helpers, and ChaCha20 limb packing.
 - Module `schnorrsig-mw` / `bulletproofs`: Initialize working scalars with `set_int(0)` instead of `scalar_clear` so Valgrind/MSan VERIFY paths stay defined.
 - sync: Updated ZKP sources to match the main ZKP branch.

#### Added
 - ci: Added `ci/local-smoke.sh` for local CI smoke testing.

#### ABI Compatibility
The ABI is backward compatible with version 0.8.0 (tag `v0.8.0-zkp`).

## [0.8.0-zkp] - 2026-09-09

#### Added
 - Ported Litecoin MWEB/ZKP modules onto bitcoin-core/secp256k1 `0.8.0` / current master APIs:
   - Module `generator`: NUMS generators, parse/serialize, and blinded generation (`include/secp256k1_generator.h`).
   - Module `commitment`: Pedersen commit, blind sum, and related scalar helpers (`include/secp256k1_commitment.h`).
   - Module `bulletproofs`: Rangeproof and inner-product prove/verify with scratch space (`include/secp256k1_bulletproofs.h`).
   - Module `aggsig`: Single and aggregated signature workflows for MWEB (`include/secp256k1_aggsig.h`).
   - Module `schnorrsig-mw`: Mimblewimble Schnorr signatures using a SHA256 challenge (not BIP-340) (`include/secp256k1_schnorrsig_mw.h`).
   - Shared scratch-space API (`include/secp256k1_scratch.h`) and ZKP helpers (`include/secp256k1_zkp.h`, including `secp256k1_ec_seckey_tweak_inv`).
 - build: Enabled ZKP modules by default in Autotools and CMake, with `--enable-module-*=no` / matching CMake options to build without them; enforced module dependencies (e.g. bulletproofs requires generator and commitment).
 - Tests: Unit tests for all ZKP modules; benches for generator and bulletproofs; exhaustive coverage for generator, commitment, aggsig, schnorrsig-mw, and partial bulletproofs; ctime coverage for secret blind/seckey/nonce paths.

#### ABI Compatibility
This is the first ZKP release on top of upstream 0.8.0. Compared to upstream 0.8.0 without ZKP modules, new public symbols are introduced for the modules listed above. Otherwise, the non-ZKP ABI matches upstream 0.8.0.

[0.8.1-zkp]: https://github.com/adgl-enterprises/secp256k1/compare/v0.8.0-zkp...v0.8.1-zkp
[0.8.0-zkp]: https://github.com/adgl-enterprises/secp256k1/compare/v0.8.0...v0.8.0-zkp
