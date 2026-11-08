# Coldhold Desktop 0.4.0-beta.1

Coldhold is a local wallet unlocked with your password and a selected physical USB drive. The installer includes the application runtime and native signer: users do not need a terminal, Node.js, Rust, Docker, a provider account or an API key.

## What's new

- English interface and native approval dialogs, with the website's charcoal and orange colors.
- Solana mainnet for SOL and supported SPL operations; HyperEVM (chain 999) for HYPE and standard ERC-20 transfers.
- Elysium (chain 99901) is listed as unavailable while its official free public RPC and explorer are pending. It is not silently replaced with HyperEVM or a test network.
- Test networks are removed from the normal desktop selector. Saved test-network transfers cannot be sent through the desktop app.
- Network-specific currencies and fees, plus protection against showing an old transfer or launch result after the selected network changes.
- Stronger Windows USB descriptor validation, bounded USB-key reads, private wallet-directory permissions, and session invalidation when USB reads fail or the unlock period expires during an operation.
- Expanded automated checks for native signing policy, USB/session failures and Windows installation, reinstallation, shortcuts, uninstall and profile retention.

## Networks and availability

| Network              | Currency | This beta                           |
| -------------------- | -------- | ----------------------------------- |
| Solana mainnet       | SOL      | Enabled                             |
| HyperEVM, chain 999  | HYPE     | Enabled for transfers               |
| Elysium, chain 99901 | HYPE     | Disabled; public connection pending |

HyperEVM and Elysium are separate networks with separate balances. Coldhold does not bridge between them. The Coldhold project token and its pool are planned only for Elysium and are **not deployed or available in this release**. The internal Elysium testnet launch implementation does not establish a live mainnet token or pool.

## Install and run

Download the Windows x64 `.exe` from this release, run the installer, and open Coldhold from the desktop or Start menu. Connect a physical USB drive and follow the wallet creation or recovery steps. To update, close Coldhold before running the new installer. Windows uninstall is configured to retain application data; keep your recovery phrase separately.

For a Mac with Apple Silicon, download the `mac-arm64.dmg`, open it and drag Coldhold to Applications. A ZIP of the same application is also provided. The release ZIP was extracted and launched in a temporary test profile, its ad-hoc signature was verified, and the DMG checksum was verified. Intel Mac and Linux packages are not updated in this release.

Running Coldhold is free on your computer. Public RPC can be rate-limited or unavailable. Transactions still require real SOL or HYPE for network fees, and token/pool operations may require deposits or liquidity. Coldhold does not purchase services or enable a paid RPC fallback.

## Beta limitations and verification

- This is experimental wallet software, not an independently audited release or a completed mainnet acceptance test.
- The Windows installer is unsigned. macOS packages, when supplied, use an ad-hoc signature without Apple notarization. OS security warnings may appear; do not disable system-wide protections.
- Physical USB behavior on Windows has **not** been verified. Automated fault tests and installer checks do not replace a real unplug/reinsert test on Windows hardware.
- A USB secret file can be copied; a flash drive is not a hardware wallet. Malware running as the user can compromise an unlocked wallet.
- Elysium's free public connection and the Coldhold token/pool deployment remain outstanding. No real-money transaction is part of the packaging tests.

Source revision: `ae5acec56c467775c16a90f99588dddee60a48b0`.

- [CI run](https://github.com/nish1-arch/coldhold-backend/actions/runs/42529989198): Node/integration tests, Rust checks and release builds on Linux/macOS/Windows, container lifecycle and security gates passed. The Node suite reports **38 passed, 0 failed, 0 skipped**. Source CI links require access to the private source repository; public evidence is attached as `verification.json`.
- [Windows packaging run](https://github.com/nish1-arch/coldhold-backend/actions/runs/42529989541): installation and same-version reinstallation in a Unicode path, Desktop/Start menu shortcuts, actual installed app launch, uninstall and preservation of the test profile marker passed on Windows Server 2022 x64.
- The Windows and Mac packaged app reports confirm English UI, the three expected network entries, Solana selected by default, disabled Elysium, isolation from Node in the renderer, local backend restart and the free RPC profile. Packaged UI/backend source was compared with the release revision on both platforms.
- Read-only checks of the public Solana and HyperEVM RPCs passed. HyperEVM nonce, simulation, gas estimation and priority-fee requests were also checked against its live official endpoint. These checks do not broadcast transactions or establish a real-funds acceptance test.

Assets: `Coldhold-0.4.0-beta.1-win-x64.exe`, `Coldhold-0.4.0-beta.1-mac-arm64.dmg`, `Coldhold-0.4.0-beta.1-mac-arm64.zip`. SHA-256 hashes are in the attached `SHA256SUMS.txt`; detailed scope and limitations are in `verification.json` and the individual smoke reports.
