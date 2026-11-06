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

In Dispatch #03, we show what happens the next morning when your friend walks into a coffee shop with a **`50 USDC` Slip** on their phone, buys a **`12 USDC` breakfast**, and gets **`38 USDC` in change** back in 700 milliseconds.
