# DISPATCH #02: The Pizza Doxx — Why Paying Someone 20 USDC on Ethereum Exposes Your Life Savings, and How 1:1 Isolated Slips Protect Your Wallet Without Mixers

**Release Order:** `#02 — Step 2 After Launch (How CryptoSLIP V3.2 Works: 1:1 Slips & Keep in Slips)`  
**Primary Goal:** Explain how creating, sending, claiming, and keeping a **CryptoSLIP** works (`CryptoSlipVaultV32` at `0x38BF0321FA7489576978353a6a8da4A3e85C2F18`), and how `Keep in Slips` (`acceptWithPermit`) protects personal wallet balances on BaseScan using 1:1 isolated vouchers instead of commingled mixers.

---

## Full Article Text (Frozen House Style)

Suppose you walk into a pizzeria tonight, pull out a 20-unit bill to pay for dinner, and the cashier looks over your shoulder and says:

*"Thanks. By the way, I can now see that you have 48,320 in your savings wallet, that you got paid on Friday at 4:12 PM, how much you sent your landlord on the first of the month, and the medical clinic you paid last Tuesday."*

You would walk out of the restaurant and never come back.

Yet every single time you send `20 USDC`, `ETH`, or `cbBTC` from a standard Ethereum or Base wallet to another person's `0x` address, **that is literally what you just did**.

Because every transfer on a public blockchain writes a permanent link between `Sender Wallet A` and `Recipient Wallet B`, anyone who receives `5 USDC` from you can paste your address into BaseScan and audit your entire financial life in fifteen seconds.

For ten years, crypto developers tried two ways to fix this—and both of them blew up in users' faces:

1. **Attempt #1: The Soup-Pot Mixer (`Tornado`-style pools).** Developers threw ten thousand strangers' tokens into a single commingled pot. If one hacker or sanctioned group dropped stolen funds into that same pot, every honest user's money came out tainted, frozen by compliance filters, and flagged by regulators.
2. **Attempt #2: Stealth Addresses (`ERC-5564` / `Umbra`).** Both users had to pre-register cryptographic keys in a public registry before anyone could send a cent. And when the `20 USDC` arrived at the new stealth address, **it was trapped there with `0 ETH` for gas**. If the recipient funded that address with `ETH` from their main wallet to get the `20 USDC` out, they immediately linked the two wallets on BaseScan and ruined the entire point.

We threw both of those broken designs in the trash and built **CryptoSLIP V3.2** on Base (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`).

Here is how a Slip works—and why it never mixes a single cent while keeping your personal wallet balance nobody else's business.

---

### Step 1: Turn Any Amount of `USDC`, `cbBTC`, `ETH`, or `NARA` Into a 1:1 Slip in 700 Milliseconds

To send value in CryptoSLIP, you never ask the other person for a `0x` wallet address.

1. You open CryptoSLIP, pick your asset (**`USDC`**, **`cbBTC` (Bitcoin)**, **`ETH`**, or **`NARA`**), and type the amount—say `20 USDC`.
2. You tap **`SLIP'IT`**.
3. If your wallet has `0 ETH`, you don't buy gas. Your phone signs a single gasless EIP-2612 / EIP-712 permit (`mintSlipERC20WithPermit`), and our relay sponsors the Base transaction in **700 milliseconds** at **`0%` creation fee**.

Inside `CryptoSlipVaultV32`, that `20 USDC` is locked inside its own isolated, numbered on-chain compartment (`slipId`). It is **never** pooled or commingled with anyone else's Slip.

Your phone generates a one-time cryptographic key (`claimKey`) that lives strictly inside the `#` fragment of your shareable link or QR code—a fragment that browsers and servers never even see over the network.

You drop that link into WhatsApp, iMessage, Signal, or Telegram—or hold up the QR code.

---

### Step 2: How "Keep in Slips" (`0%` Fee) Breaks the Wallet-Spying Chain

When your friend taps your link, they don't need to install a browser extension, write down 12 seed words, or own `ETH`.

They see two buttons on their screen: **Claim to Wallet** or **Keep in Slips (`0%` fee)**.

Watch what happens when they tap **Keep in Slips**:

* Their phone derives a brand-new, deterministic one-time key (`fresh.address`) from their personal recovery key and calls `acceptWithPermit(slipId, fresh.address, fresh.address, keepEscrowedInVault: true)`.
* In less than one second—and at **`0%` fee** (`20.000000 USDC` stays `20.000000 USDC`, down to the last micro-unit)—the smart contract cancels your old link key and locks that `20 USDC` Slip exclusively to your friend's phone.
* **The Critical Part:** Neither your friend's personal wallet address nor your personal wallet address ever touched each other. The `20 USDC` never had to land in a public `0x` wallet where nosey strangers could trace their savings.

Your friend now owns a live `20 USDC` Slip on their phone. And what can they do with it next?

They don't ever have to withdraw it to a public wallet to use it. They can **spend part of it, combine it with other Slips, or swap it into Bitcoin or NARA directly inside the vault**.

| Architecture | Needs Recipient `0x` Address? | Needs `ETH` for Gas? | Commingles Funds With Strangers? | Exposes Personal Wallet Balance to Counterparty? |
| :--- | :---: | :---: | :---: | :---: |
| **Standard EVM Transfer** | Yes | Yes | No | **Yes (100% Exposed on BaseScan)** |
| **Legacy Mixers** | Yes | Yes | **Yes (High Regulatory / Taint Risk)** | No |
| **Stealth Addresses (`ERC-5564`)** | Yes (Registry) | Yes (`0 ETH` Trap) | No | Breaks if funded from main wallet |
| **CryptoSLIP V3.2 (`Keep in Slips`)** | **No (Link / QR)** | **No (`$0` Sponsored)** | **Never (`1:1` Isolated `slipId`)** | **Never (Ephemeral One-Time Slip Keys)** |

---

### Step 3: Three-Tier On-Chain Visibility & Why Compliance Auditors Love UTXO `slipId` Genealogy

A natural question from both traders and institutional compliance officers is:

*"If everyone lives inside Slips—receiving payments via `Keep in Slips`, splitting change, and swapping `USDC` $\leftrightarrow$ `NARA` $\leftrightarrow$ `cbBTC` inside the vault—what do market bots see on DEX Screener, and how do exchanges stop hackers from laundering stolen money?"*

Because `CryptoSlipVaultV32` (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`) is engineered like **serialized digital banknotes (a UTXO-style `slipId` model)** rather than a commingled pool, it achieves both **personal wallet privacy** and **100% regulatory provenance**:

1. **Tier 1 — Automated Market Scanners, Copy-Trade Bots & Bubblemaps:**
   - When a user claims a Slip to a fresh wallet (`cashWithPermit`), top-level transaction headers show `From: Relayer` and `To: CryptoSlipVaultV32 (0x38BF...2F18)`. Automated wallet-clustering graphs do not parse internal `slips[slipId]` structs, so the destination wallet appears with zero prior EOA transfer links.
   - When a user swaps assets inside the vault (`CryptoSlipTradeRouterV32.swapSlipToSlipWithPermit`), DEX Screener sees only the router (`0x295E72cB3dc09582F32e0fEB7e0e650752219a7f`) and the vault (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`). The user's personal MetaMask or Coinbase Wallet never touches the DEX.
2. **Tier 2 — Retail Recovery Mode (`creator = userAddress`) vs. Ephemeral Creator Mode (`mintSlipFor`):**
   - In the standard public web and Telegram app, `creator = userAddress` is used by default for two user-protection reasons: (a) if a user clears their browser cache or switches phones without a local backup, the app scans `SlipMinted(null, userAddress)` events on Base to recover active Slips automatically; and (b) when `creator != claimSigner`, the creator can cancel an unclaimed link (`Take back`) at a **`0%` fee**, plus use Circle's gasless `EIP-3009` `receiveWithAuthorization` (which requires the `from` authorizer to match `creator`).
   - For advanced programmatic or router integrations that persist keys locally, `CryptoSlipVaultV32.mintSlipFor(token, amount, ephemeralCreator, claimSigner, ...)` (line 448 of `CryptoSlipProtocolV32.sol`) allows passing a single-use random `ephemeralCreator` (`creator == claimSigner`, applying a `0.50%` refund fee if recalled), preventing naive event-log scraping on `topic[2]` (`indexed creator`).
3. **Tier 3 — Unbroken UTXO `slipId` Serial Genealogy (Why Hackers Cannot Launder Stolen Funds):**
   - Unlike illicit mixers that destroy provenance by pooling funds, every in-vault operation in `CryptoSlipProtocolV32.sol` (lines 1862–1885) emits an explicit, immutable serial-number lineage linking parent `slipId`s to child `slipId`s:
     - `SlipSwappedToSlip(inputSlipId, changeSlipId, outputSlipId, ...)`
     - `SlipSplitAndPaid(originalSlipId, changeSlipId, merchant, ...)`
     - `SlipsTradedP2P(slipIdA, slipIdB, newSlipForA, newSlipForB, ...)`
   - **Result:** Your *personal identity* (whose phone currently holds the ephemeral `claimSigner` key for `Slip #150`) stays completely private from merchants and copy-traders, while the *mathematical provenance of the funds* (`Slip #100 -> Slip #105 -> Slip #118 -> Slip #150`) remains 100% auditable on Base. If an attacker ever deposited stolen funds into `Slip #100` and swapped them 50 times inside the vault, compliance tools at any exchange can trace `Slip #150` straight back to `Slip #100` in milliseconds.

In Dispatch #03, we show what happens the next morning when your friend walks into a coffee shop with a **`50 USDC` Slip** on their phone, buys a **`12 USDC` breakfast**, and gets **`38 USDC` in change** back in 700 milliseconds.
