# Dependency security decisions

Lockfiles are committed. npm installs use `--ignore-scripts`; the SQLite addon is then explicitly built from verified source. Required native test packages distribute prebuilt optional binaries. CI runs npm audit, RustSec, formatting, lint, typecheck, unit/integration tests and secret scanning. A clean check is not an independent security audit.

## SQLite WAL correction in 0.4.3

The native wallet now uses rusqlite 0.41.1 / libsqlite3-sys 0.39.1, bundling SQLite 3.54.1. This fixes [RUSTSEC-2026-0459](https://rustsec.org/advisories/RUSTSEC-2026-0459.html), where inconsistent database and WAL page sizes can expose uninitialized memory.

Electron 45.2.3 still embeds SQLite 3.53.4. The gateway journal and desktop outbox therefore use the pinned `better-sqlite3@13.0.3` addon with the exact [upstream WAL fix](https://github.com/sqlite/sqlite/commit/e5bf119821f25be1d7f10ef0a2edbe175289166e) backported to its bundled SQLite. `scripts/prepare-sqlite-native.mjs` verifies the original source hashes, applies the fix, verifies the patched hashes, and removes downloaded prebuilds and stale build output. The build adds a diagnostic source ID suffix; the shared adapter verifies this ID in an in-memory database before opening any profile database. It loads only the explicitly compiled addon. An absent or unpatched addon fails closed.

After `npm ci --ignore-scripts`, run `npm run native:node` for Node tools/tests. Desktop build commands prepare the pinned source and rebuild the addon for Electron; `npm run native:electron` does this explicitly for local development. Python 3 and a C++ compiler are required to build from source. End users receive the compiled addon in the installer. Packaged smoke reports record its version, source ID and patch identity on each tested platform.

Regression coverage includes a database/WAL page-size mismatch, durable crash recovery with an incomplete trailing WAL frame, reopening existing journal data, and rejecting an unpatched engine before opening a disk database. No WAL or shared-memory files are deleted to work around the issue. Re-evaluate this backport when the addon ships an upstream-fixed SQLite version; update source pins, the diagnostic check, and regression tests together.

## JavaScript

On 2026-10-31, the refreshed audit reported GHSA-g3hv-75wg-m249 in minimatch across the development lint tree. ESLint was upgraded to 10.14.0 and typescript-eslint to 8.72.1, with minimatch pinned to the patched 11.0.1. The complete lint/typecheck/build and chain tests exercise compatibility. No audit rule was disabled and no forced downgrade was applied. See the [advisory](https://github.com/advisories/GHSA-g3hv-75wg-m249).

The official legacy Solana SDK tree still requests older dependencies. npm overrides remove the vulnerable native `bigint-buffer` implementation in favor of the pure JavaScript `@exodus/bigint-buffer@1.1.5-exodus.1`. No native buffer converter is loaded. Jayson uses UUID 11.1.1; its unused streaming-server dependency is updated to stream-json 3.8.0. Coldhold/Solana use Jayson's browser HTTP client, not its standalone Node streaming server (the latter's legacy imports are not supported by the override). The integration suite exercises the actual RPC/SDK paths. `tmp` is pinned to 0.2.7 for the development Solidity compiler. These overrides must be re-evaluated when updating the Solana SDK; do not blindly run `npm audit fix --force` or downgrade SPL Token.

## Rust

The enabled EVM signing/derivation path uses rust-bitcoin's BIP32 and `libsecp256k1`, with Alloy only encoding transactions. The k256 path was removed after RUSTSEC-2026-0387 (crypto-bigint timing variability).

Cargo's lockfile still resolves Alloy's **disabled optional** k256/crypto-bigint dependency. Raw `cargo audit` reports that lockfile entry. `scripts/audit-rust.py` does not blanket-ignore it: it allows only the exact advisory/version `RUSTSEC-2026-0387`, `crypto-bigint@0.5.5`, and only when `cargo tree --locked --target all --edges normal,build,dev -i` has no reachable dependency on any target. Re-enabling it fails CI. All other security advisories fail the gate. The report explicitly prints this lockfile-only finding.

RustSec also flags bincode 1.3.3 as unmaintained (RUSTSEC-2025-0141). It is required by the pinned Solana message format. Payloads are tightly bounded, structures validated, and canonical reserialization compared before signing. This informational maintenance warning is retained in audit output, not suppressed. Migration to newer Solana message/serialization crates requires protocol compatibility tests.

RustSec additionally reports the transitive `derivative` (RUSTSEC-2024-0388) and `paste` (RUSTSEC-2024-0436) crates as unmaintained. These informational findings remain visible in CI and are part of dependency-upgrade review. Gitleaks has one narrowly scoped allowlist entry for the exact public Solana test-vector address in the example pool configuration; no credential pattern is globally ignored.

## Bundled font

DM Sans is bundled locally under the SIL Open Font License 1.1. Its copyright and full license are included at `desktop/fonts/OFL.txt` inside the application archive. Font loading does not contact a CDN or font service.
