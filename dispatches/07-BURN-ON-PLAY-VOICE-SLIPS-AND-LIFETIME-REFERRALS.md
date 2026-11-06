# DISPATCH #07: Burn-on-Play Voice Slips & The 50/50 Lifetime Referral Loop — How One Slip Sent to a Friend Binds Them On-Chain Forever

**Release Order:** `#07 — Step 7 After Launch (Zero-Knowledge Text/Voice Slips + Lifetime 50/50 On-Chain Referrals)`  
**Primary Goal:** Explain how **Zero-Knowledge Encrypted Text Notes & "Burn-on-Play" Voice Slips** protect private human context from block explorers and chat history forensics, and how the **Pure "Claim = Referee" (`50/50` fee split)** turns every Slip sent to a friend into a lifetime on-chain revenue stream.

---

## Full Article Text (Frozen House Style)

Suppose you send `100 USDC` to your brother to help pay for a medical bill, or you tip an investigative journalist `0.01 cbBTC`, or you send your partner `50 USDC` for an anniversary dinner with a personal note attached.

Look at where that note lives on every other payment app on earth:

* **On Venmo, PayPal, or Revolut:** Your note sits in plaintext inside a corporate marketing database forever, sold to data brokers and handed over in bulk whenever an agency sends a spreadsheet request.
* **On Ethereum, Solana, or Base (`calldata` memos):** It is ten times worse. Any memo written into a blockchain transaction is **carved into stone on 10,000 public nodes until the end of time**. Ten years from now, your employer, your ex-spouse, or a data scraper can read that note on BaseScan.
* **Even inside Signal, WhatsApp, or Telegram:** If someone's phone is unlocked at an airport border crossing six months later, the text message and voice note are still sitting right there in the chat history next to the payment link.

Money without a message feels cold. A message that lives on a server forever is a liability.

So we built **Zero-Knowledge Encrypted Notes & "Burn-on-Play" Voice Slips**, and paired them with the most powerful viral engine on Base: **1 Slip = 1 Referee For Life**.

---

### Part 1: Two Ways to Attach a Message That Leaves Zero Trace on BaseScan

#### Mode A: Fragment-Only Private Text Notes (`0 Bits` on the Blockchain, `0 Bits` on Any Server)

When you type a short note on a CryptoSLIP (`"For Friday's flight — don't tell Mom"`), that text **never touches Base Mainnet** and **never touches our relay servers**.

It is compressed and encrypted directly inside the `#` URL fragment of your Slip link—derived via `HKDF-SHA256` from the link's one-time secret. By internet RFC standards, browsers never transmit `#` fragments in HTTP requests, and `App.tsx` wipes the URL bar (`history.replaceState`) within `<1 millisecond` of opening.

When your friend taps **Claim** or **Keep in Slips**, `forgetPendingPayload()` overwrites the note in local memory. Even if their phone is inspected the next day, the note is gone.

#### Mode B: "Burn-on-Play" Encrypted Voice Slips (`32 KB` Padded `AES-256-GCM` + RAM Wipe)

What if you want to hold the microphone button for 8 seconds and attach your real voice—or a scrubbed **Ghost Voice**—to a `50 USDC` or `10 NARA` Slip?

1. **Client-Side Encryption & Traffic-Analysis Padding:** Your browser records 5–15 seconds of Opus audio, strips the `120–3800 Hz` electrical grid hum (`ENF`) and device metadata, pads the buffer to an exact **`32,768-byte` (`32 KB`) constant size** (so no network observer can guess the length of what you said), and encrypts it with **`AES-256-GCM`** using a key that never leaves the `#` link fragment.
2. **Split-Key Dual-Blind Storage (`Cloudflare` $\oplus$ `Akash`):** The encrypted ciphertext sits in an ephemeral TTL bucket while a 32-byte server share is split via XOR across two independent jurisdictions. Neither server can ever decrypt a single syllable.
3. **Physical Destruction on First Play:** When your recipient taps **▶ Play Voice Note**, the server atomically deletes its XOR share on the very first read **before** returning the bytes, and your browser decodes the audio inside an isolated `WebWorker` that immediately zeroes its own memory buffers (`audioBuffer.fill(0)`) and terminates (`worker.terminate()`).

They hear your voice once. A split-second later, the voice note no longer exists anywhere on earth—not on Base, not on our relays, and not in their phone's storage.

---

### Part 2: The Viral Engine — How 1 Slip Sent to a Friend Pays You Forever (`50/50` On-Chain Split)

Now look at how **NARA V5** and **CryptoSLIP V3.2** grow without paying a single dollar to ad networks or bot farms.

Every other crypto referral program is broken in one of two ways:
* Either they print fake inflationary points out of thin air;
* Or they use `/?ref=` web links that bots squat on by the millions without ever sending a cent.

Inside `CryptoSlipVaultV32` (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`), there are no referral cookies and no empty invite links that bind on-chain.

There is only one rule: **Pure "Claim = Referee."**

1. **You Harvest & Send 1 Real Slip:** You claim your 15-minute `NARA`, `ETH`, or `USDC` pulse rewards from NARA V5 directly into a CryptoSLIP link (or create any Slip of at least `0.10 USDC`, `100 sats cbBTC`, `0.0001 ETH`, or **`0.001 NARA`**) and send it to a friend.
2. **Your Friend Taps Claim or Keep:** The moment your friend signs their `CashPermit` or `AcceptPermit`, the smart contract permanently writes:
   ```solidity
   referrerOf[friend] = you; // Immutable on Base Mainnet for life
   ```
3. **You Earn `50%` of the Protocol Fee Forever:** CryptoSLIP charges `0%` to create a Slip and `0%` for the first recipient to tap **Keep in Slips**. Whenever a fee-bearing action happens (cashing out to an external wallet, splitting to pay a merchant, combining, or swapping), the protocol charges a tiny **`0.50%` (`50 bps`)** fee—and splits it **50/50** on-chain:
   * **`0.25%` (`50%` of the fee)** is credited immediately in the native token (`USDC`, `cbBTC`, `ETH`, or `NARA`) to `referralCredits[you][asset]`.
   * Your referral earnings sit inside the vault's protected `liabilities(asset)` invariant—and you can withdraw them at `0%` fee or wrap them into a new **NARA Slip** in one tap (`claimReferralAsNaraSlipOnChain`) to bind the next friend.

| Action | Protocol Fee | What You Earn as Referrer |
| :--- | :---: | :---: |
| **Friend Creates a Slip** | `0%` | Vault tags `slipReferrer[slipId] = you` |
| **Friend Claims, Splits, Combines, or Swaps** | `0.50%` | **`0.25%` (`50%` of fee) paid instantly in `USDC`, `cbBTC`, `ETH`, or `NARA`** |
| **You Claim Your Referral Credits as a New Slip** | **`0%`** | Ready to send to your next friend and bind another referee for life |

---

### The Complete Loop on Base Mainnet (`Chain ID: 8453`)

1. **Commit `NARA` in NARA V5** (`0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f`) -> earn **15-minute multi-asset pulses** in `NARA`, `ETH`, and `USDC` inside a vault where the reserve is welded shut (`949,987 NARA`) and your principal is isolated (`NARAPrincipalLedgerV5`).
2. **Harvest Pulse Rewards into a CryptoSLIP in 1 Tap** (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`) -> send `USDC`, `cbBTC`, `ETH`, or `NARA` in **700 milliseconds** with a private note or Burn-on-Play voice message, online or over a **$35 LoRa radio (`69 bytes`)**.
3. **Your Friend Taps Claim or Keep** -> bound to your address on Base **for life**, paying you **50% of the protocol fee** on every Slip they ever touch.

No bank executives borrowing your retirement in the dark. No public `0x` strip-search when you buy a coffee. Just unstoppable math on Base.
