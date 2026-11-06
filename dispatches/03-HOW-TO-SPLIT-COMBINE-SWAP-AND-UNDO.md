# DISPATCH #03: Living Inside Ephemeral Envelopes — How to Spend 12 USDC From a 50 USDC Slip, Get 38 USDC Change at 0% Fee, Combine 10 Slips, and Swap Into Bitcoin Inside the Vault

**Release Order:** `#03 — Step 3 After Launch (How to Spend, Split, Combine, Swap & Undo Slips)`  
**Primary Goal:** Walk users through the 5 everyday actions inside CryptoSLIP V3.2: **Split & Pay** (`splitAndPayWithPermit`), **Combine up to 10 Slips** (`rearrangeAndPayWithPermits`), **In-Vault Token Swaps** (`CryptoSlipTradeRouterV32`), **P2P Slip Barter**, and the **"Wrong Chat" Undo Button** (`Take back`).

---

## Full Article Text (Frozen House Style)

Every "crypto link" experiment before 2026 made the same fatal mistake.

They treated a payment link like a glass piggy bank: the only way to use the `50 USDC` inside was to smash the piggy bank open and dump all `50 USDC` into your public wallet at once.

Think about how absurd that is. If you have a `50 USDC` voucher and you want to pay a freelancer `12 USDC`—or buy a `4 USDC` espresso—why should you have to sweep all `50 USDC` into your public wallet, expose your address on BaseScan, and pay fees on the whole `50 USDC`?

In **CryptoSLIP V3.2**, you don't smash the voucher. You spend directly from it.

Here are the five things you can do with a Slip on your phone in under one second.

---

### 1. Break a `50 USDC` Slip for a `12 USDC` Purchase (`0%` Fee on Your Change)

Suppose you kept a `50 USDC` Slip on your phone yesterday (`Keep in Slips`), and today a merchant or friend asks you for `12 USDC`.

You select your `50 USDC` Slip, type `12 USDC`, and scan their QR or paste their address.

Behind the scenes, `CryptoSlipVaultV32` executes `splitAndPayWithPermit` in a single atomic Base transaction:
* **`12 USDC` Goes to the Merchant:** The protocol deducts its `0.50%` fee **only on the `12 USDC` you actually spent** (`0.06 USDC`), never on the whole `50 USDC`.
* **`38 USDC` Snaps Back to Your Phone at `0%` Fee:** The remaining `38.000000 USDC` is minted into a brand-new, isolated Change Slip on a fresh one-time key on your phone (`deriveSplitChangeSlipKey`) with **zero fee**.

Your personal wallet address never appeared on-chain, and your `38 USDC` change is ready for tomorrow's lunch.

---

### 2. Combine 10 Small Slips Into One Clean Slip (`rearrangeAndPayWithPermits`)

After a week of splitting Slips and collecting NARA V5 15-minute pulse rewards, you might have six or seven small leftover Slips sitting in your app: `3.40 USDC`, `8.10 USDC`, `1.25 USDC`, `14.00 USDC`.

You don't have to cash them out one by one.

Tap **Combine** (or select up to **10 Slips** at once when making a payment). The vault verifies your one-time signatures across all 10 Slips in a single transaction (`rearrangeAndPayWithPermits`), burns the small scraps, and hands you a single consolidated Slip locked to a fresh key on your phone.

---

### 3. Swap a `USDC` Slip Into `Bitcoin (cbBTC)`, `ETH`, or `NARA` Without Leaving the Vault

What if a client pays you with a `100 USDC` Slip, but you want to hold **`cbBTC` (Coinbase Wrapped Bitcoin)**, **`ETH`**, or **`NARA`**?

On any normal app, you would have to claim the `100 USDC` to your wallet, get `ETH` for gas, connect your wallet to Uniswap or Aerodrome, sign an approval, execute a swap on your public address, and then create a new voucher.

In CryptoSLIP V3.2, you tap **Swap** right on the Slip card.

Our dedicated router—**`CryptoSlipTradeRouterV32` (`0x295E72cB3dc09582F32e0fEB7e0e650752219a7f`)**—takes your `100 USDC` Slip, executes the swap through deep Base DEX liquidity (`Aerodrome` / `Uniswap`), verifies your strict minimum output (`minAmountOut`), and mints a brand-new **`cbBTC` or `NARA` Slip** directly back into the vault under a fresh one-time key on your phone.

Your personal wallet never touched the DEX.

---

### 4. Peer-to-Peer Slip Barter (`tradeSlipsWithPermits`)

Want to trade a `500 NARA` Slip directly with another trader for their `250 USDC` Slip at an agreed price with zero slippage and zero front-running?

`CryptoSlipTradeRouterV32.tradeSlipsWithPermits` settles a two-way atomic swap between two Slips in a single block: either both parties receive their new isolated Slips simultaneously, or the transaction reverts cleanly.

---

### 5. The "Wrong Chat" Undo Button (`Take back` at `0%` Fee)

Have you ever pasted a link into a 500-person Telegram group when you meant to send it to your brother's DM?

With a normal crypto transfer, your money is gone the millisecond you hit send.

With a standard CryptoSLIP, as long as the recipient hasn't claimed or kept it yet, you just tap **Take back** on your Slip card. In one second—at **`0%` protocol fee**—the vault invalidates the link in the group chat and returns 100% of your funds to your wallet.

*(And what if you are paying a stranger in person who demands a guarantee that you **cannot** press "Take back" after walking out of their shop? We solved that too—in Dispatch #05).*

---

### The Next Question Every Engineer Asks

Wait a second.

If every Slip you keep, split, combine, or swap lives on a brand-new one-time key with **`0 ETH`** on it:
1. How do those keys pay for Base network gas?
2. And what happens to all your kept Slips if you **drop your phone off a boat into the ocean**?

Turn the page to **Dispatch #04**.
