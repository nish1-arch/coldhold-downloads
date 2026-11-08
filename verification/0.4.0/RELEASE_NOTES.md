# Coldhold Desktop 0.4.0

All three mainnets are enabled: **Solana, Elysium and HyperEVM**. The English interface keeps the website's colors and now displays version **0.4.0** without a pre-release suffix.

## Changes

- Elysium chain 99901 connects to the free, keyless Eleos community RPC at `https://node.eleos.fun`, with `https://elysiumexplore.com` as its explorer. [Provider documentation](https://eleos.fun/docs), [RPC limits](https://elysiumexplore.com/api-docs).
- Network discovery and readiness checks include all three networks. Elysium balances, native HYPE transfer preparation/simulation, Nitro fee estimation, WHYPE metadata and an ERC-20 transfer simulation were checked through the actual application adapter.
- ERC-20 preparation rejects a simulated transfer that returns `false` or malformed data before a signing intent can be created. Canonical `true` and legacy empty responses after successful metadata validation remain supported.
- Packaged-app checks verify English, three enabled network entries, the default Solana selection, absence of the pre-release label and operation without external runtimes or paid API keys.

## Download and run

Windows x64: download `Coldhold-0.4.0-win-x64.exe`, run it, then open Coldhold from the desktop or Start menu. Mac with Apple Silicon: open `Coldhold-0.4.0-mac-arm64.dmg` and drag Coldhold to Applications; a ZIP of the same app is also included. No terminal is required. Close Coldhold before updating. Intel Mac and Linux installers are not updated in this release.

Runtime is free on your computer. Transactions still cost SOL or HYPE gas. Eleos describes sponsored parent-chain relaying during the launch window; confirmation can wait for an inbox batch. Keep checking the saved transaction after an unknown result instead of creating a duplicate payment. Elysium and HyperEVM are separate chains; bridging and HyperCore trading are not included.

The Coldhold project token and pool are **not deployed**. Mainnet connectivity does not create or enable those contracts.

## Verification

Source revision: `d03104fe9fcadc6b9c3867834cccdd2102d9da9d`.

- [CI](https://github.com/nish1-arch/coldhold-backend/actions/runs/42532165466): **58 Node.js tests passed, 0 failed, 0 skipped**, including local Anvil transfers on mainnet chain IDs, LiteSVM SPL/Raydium execution and ERC-20 simulation regressions; Rust checks/builds on Windows, macOS and Linux; container and security gates.
- [Windows installer](https://github.com/nish1-arch/coldhold-backend/actions/runs/42532165564): install, same-version reinstall, Unicode paths, both shortcuts, actual app launch, uninstall and profile retention checked on Windows Server 2022 x64.
- Mac release ZIP extracted and launched in an isolated profile; ad-hoc signature and DMG checksum verified. Packaged UI/backend sources compared with the exact source revision on both platforms.
- Live public checks verified all three network identities and heights. Elysium's genesis and a fixed block hash matched independent nodes. Reads and simulation do not establish successful real-funds broadcasting; no valid mainnet transaction was sent.

Source CI links require repository access. Public results and file sizes/hashes are attached in `verification.json`, `SHA256SUMS.txt` and the smoke reports.

## Remaining limits

No independent security audit or real-funds mainnet acceptance test is claimed. Physical USB behavior on Windows, a separate Windows 10/11 manual GUI session and cross-version upgrades remain unverified. Windows is unsigned; macOS uses an ad-hoc signature without Apple notarization. OS security controls can warn or block. A USB secret can be copied and is not a hardware wallet. RPC services have their own rate limits and availability; the Eleos endpoint is operated by the community provider.
