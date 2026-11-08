# Coldhold Desktop

An English-language local wallet with password + USB unlocking. The Windows installer includes the app, local backend and native signer. No terminal, Node.js, Rust, Docker, server subscription or API key is needed.

**[Download Coldhold for Windows x64 — 0.4.0-beta.1 (.exe)](https://github.com/nish1-arch/coldhold-downloads/releases/download/v0.4.0-beta.1/Coldhold-0.4.0-beta.1-win-x64.exe)**

**[Download Coldhold for Mac with Apple Silicon — 0.4.0-beta.1 (.dmg)](https://github.com/nish1-arch/coldhold-downloads/releases/download/v0.4.0-beta.1/Coldhold-0.4.0-beta.1-mac-arm64.dmg)**

[Release notes and verification](https://github.com/nish1-arch/coldhold-downloads/releases/tag/v0.4.0-beta.1). The app uses the website's charcoal background, light text and orange accents. Open the installer, choose a directory, then launch Coldhold from the desktop or Start menu.

On a Mac with an M-series chip, open the DMG and drag Coldhold to Applications. The Mac build uses an ad-hoc signature and has not been notarized by Apple; macOS may block it. This release does not include an updated Intel Mac or Linux package.

## Networks

| Network  | Status in this version                                     | Assets                                              |
| -------- | ---------------------------------------------------------- | --------------------------------------------------- |
| Solana   | Available; selected by default                             | SOL and supported standard SPL tokens               |
| HyperEVM | Available; chain ID 999                                    | HYPE and explicitly selected standard ERC-20 tokens |
| Elysium  | Awaiting its official free public RPC; disabled in the app | Planned HYPE/ERC-20 support on chain ID 99901       |

Development networks are not shown or usable through the desktop interface. Elysium's official connection documentation has published its mainnet chain ID, but still marks the public RPC and explorer as coming soon. The app does not substitute another network or silently use a paid provider. [Official Elysium connection details](https://elysium.kinetiq.xyz/docs/connecting-to-elysium)

The Coldhold project token and its pool belong on Elysium. They have **not been deployed**. Local contract tests are not proof of a live public deployment. HYPE on HyperEVM and HYPE on Elysium have separate balances; sending to the same address does not bridge between them. HyperCore exchange trading and bridging are not included.

## First launch

1. Connect a real USB flash drive and select it explicitly. The app does not format it or change unrelated files.
2. Create a wallet, record the recovery phrase offline, confirm it and set a password. Existing phrases can be recovered in the app.
3. Unlock with the matching USB and password. Sessions expire after five minutes. Observed USB removal or changed key material blocks further signing and invalidates pending approval.
4. Choose the network and account, review the recipient, amount and fees, then approve the separate native confirmation dialog.
5. If a send has an unknown result, reopen the saved operation and check its status before creating another payment. Retries reuse the existing signature.

A normal USB flash drive is not a hardware wallet: its secret file can be copied, and malware running as the user can compromise an unlocked wallet. Keep the recovery phrase separately. Settings provides password changes, USB-key rotation and recovery with address verification.

## Free operation

The app and backend run locally without subscriptions or paid APIs. Public RPC endpoints have rate and availability limits. **Blockchain transactions still require network fees and the user's own funds.** Installation and automated checks do not buy assets or move real funds.

This is an experimental beta, without an independent security audit or a purchased publisher certificate. Windows may show an unsigned-app reputation warning. Do not disable system-wide security protections. Tests do not establish suitability for storing material funds.

## Data and updates

Wallet data is stored separately in `%APPDATA%/Coldhold` on Windows and `~/Library/Application Support/Coldhold` on macOS. Close the app before installing a newer version. The Windows uninstaller preserves the application data directory. Keep backups; old encrypted vault backups still use their original password and USB material. There is no automatic updater.

The wallet supports bounded balances/history, saved transfer reviews, transaction status and immutable retries. Complete EVM wallet history and automatic discovery of every ERC-20 are not provided. Token-2022 extensions are not supported. The advanced Solana token/pool form requires explicit JSON configuration and separate approval; it does not launch the Coldhold project token or provide a trading interface.

## Verification

The release contains `SHA256SUMS.txt`, `verification.json` and installer smoke reports. Those records identify the source revision and distinguish local-chain tests, actual Windows API/installer tests and public read-only RPC checks.

Physical USB removal/reinsertion on Windows remains unverified: software and CI tests cannot replace a real Windows USB device. A previous Kingston hardware test ran on macOS. No real mainnet transfer or independent security audit is claimed.

[Older Windows packages](https://github.com/nish1-arch/coldhold-downloads/releases/tag/v0.3.0-beta.2) and [older macOS/Linux packages](https://github.com/nish1-arch/coldhold-downloads/releases/tag/v0.2.0-beta.1) are historical builds and do not contain all changes described above. Do not download GitHub's **Source code.zip** to install Coldhold.

Coldhold is distributed under the [MIT license](LICENSE). Bundled third-party components retain their own licenses. See [DEPENDENCIES.md](DEPENDENCIES.md) for dependency policy and recorded advisories.
