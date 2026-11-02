# Dependency security decisions

Lockfiles are committed. npm installs use `--ignore-scripts`; required native test packages distribute prebuilt optional binaries. CI runs npm audit, RustSec, formatting, lint, typecheck, unit/integration tests and secret scanning. A clean check is not an independent security audit.

## JavaScript

On 2026-10-31, the refreshed audit reported GHSA-g3hv-75wg-m249 in minimatch across the development lint tree. ESLint was upgraded to 10.14.0 and typescript-eslint to 8.72.1, with minimatch pinned to the patched 11.0.1. The complete lint/typecheck/build and chain tests exercise compatibility. No audit rule was disabled and no forced downgrade was applied. See the [advisory](https://github.com/advisories/GHSA-g3hv-75wg-m249).

The official legacy Solana SDK tree still requests older dependencies. npm overrides remove the vulnerable native `bigint-buffer` implementation in favor of the pure JavaScript `@exodus/bigint-buffer@1.1.5-exodus.1`. No native buffer converter is loaded. Jayson uses UUID 11.1.1; its unused streaming-server dependency is updated to stream-json 3.8.0. Coldhold/Solana use Jayson's browser HTTP client, not its standalone Node streaming server (the latter's legacy imports are not supported by the override). The integration suite exercises the actual RPC/SDK paths. `tmp` is pinned to 0.2.7 for the development Solidity compiler. These overrides must be re-evaluated when updating the Solana SDK; do not blindly run `npm audit fix --force` or downgrade SPL Token.

## Rust

The enabled EVM signing/derivation path uses rust-bitcoin's BIP32 and `libsecp256k1`, with Alloy only encoding transactions. The k256 path was removed after RUSTSEC-2026-0387 (crypto-bigint timing variability).

Cargo's lockfile still resolves Alloy's **disabled optional** k256/crypto-bigint dependency. Raw `cargo audit` reports that lockfile entry. `scripts/audit-rust.py` does not blanket-ignore it: it allows only the exact advisory/version `RUSTSEC-2026-0387`, `crypto-bigint@0.5.5`, and only when `cargo tree --locked --target all --edges normal,build,dev -i` has no reachable dependency on any target. Re-enabling it fails CI. All other security advisories fail the gate. The report explicitly prints this lockfile-only finding.

RustSec also flags bincode 1.3.3 as unmaintained (RUSTSEC-2025-0141). It is required by the pinned Solana message format. Payloads are tightly bounded, structures validated, and canonical reserialization compared before signing. This informational maintenance warning is retained in audit output, not suppressed. Migration to newer Solana message/serialization crates requires protocol compatibility tests.

RustSec additionally reports the transitive `derivative` (RUSTSEC-2024-0388) and `paste` (RUSTSEC-2024-0436) crates as unmaintained. These informational findings remain visible in CI and are part of dependency-upgrade review. Gitleaks has one narrowly scoped allowlist entry for the exact public Solana test-vector address in the example pool configuration; no credential pattern is globally ignored.
