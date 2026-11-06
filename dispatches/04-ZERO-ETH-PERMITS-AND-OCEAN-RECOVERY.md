# DISPATCH #04: The 0 ETH Paradox & The Ocean Test — How Gasless EIP-712 Permits Work, and How One Master Key Recovers 1,024 Slips If You Drop Your Phone in the Sea

**Release Order:** `#04 — Step 4 After Launch (How 0-ETH Gasless Relays & One-Key Recovery Work)`  
**Primary Goal:** Explain how CryptoSLIP V3.2 eliminates the `ETH` gas trap across all 7 operations using EIP-712 permits and dual independent relays (`Akash + Cloudflare`), and how a single master key recovers every kept, split, and swapped Slip in $O(1)$ math if a user loses their phone.

---

## Full Article Text (Frozen House Style)

Give a normal crypto wallet to your friend, send them `20 USDC`, and ask them to send `10 USDC` back to you.

Nine times out of ten, they will stare at their phone and say:

*"It says I have `20 USDC`, why is the Send button grayed out asking me for `0.00004 ETH`? Where am I supposed to buy four cents of Ethereum?"*

That single error message has killed more peer-to-peer crypto payments than every bear market combined. Asking someone to hold a volatile second asset (`ETH`) just to move `USDC` or `NARA` is like a post office refusing to hand you your package unless you pay the clerk three cents in foreign coins.

And there is a second problem that is even worse.

Earlier we showed how tapping **Keep in Slips**, **Split**, **Combine**, or **Swap** generates a fresh, one-time cryptographic key on your phone so your personal wallet stays invisible on BaseScan.

A sharp reader immediately asks: *"If my phone creates a brand-new one-time key every time I keep or split a Slip—what happens if my phone falls into the sea on vacation? Do I lose all 15 of my Slips unless I backed up 15 different keys?"*

Here is how **CryptoSLIP V3.2** killed both the **`0 ETH` Gas Trap** and the **Lost-Phone Trap** at the same time.

---

### Part 1: Seven Cryptographic Permits, Two Independent Relays, and the Escape Hatch

In `CryptoSlipVaultV32` (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`) and `CryptoSlipTradeRouterV32` (`0x295E72cB3dc09582F32e0fEB7e0e650752219a7f`), **every single action** is powered by an off-chain **EIP-712 cryptographic signature**:

1. **`MintPermit` (+ EIP-2612 `permit`):** Create a `USDC` or `NARA` Slip from a wallet that has `0.000000 ETH`.
2. **`CashPermit`:** Claim a Slip to any destination address with `0 ETH`.
3. **`AcceptPermit`:** Rotate a Slip to a fresh one-time key (**Keep in Slips**) with `0 ETH`.
4. **`SplitPermit`:** Split a Slip, pay a merchant, and mint a Change Slip with `0 ETH`.
5. **`RefundPermit`:** Take back or redirect an unclaimed Slip with `0 ETH`.
6. **`SwapOfferPermit`:** Swap a Slip into `cbBTC`, `USDC`, `ETH`, or `NARA` (or barter two Slips) with `0 ETH`.
7. **`ReferralConsent`:** Cryptographically bind your first-touch referrer on-chain during Claim or Keep.

When you tap a button in the app, your phone signs a 65-byte mathematical authorization in under 10 milliseconds. Our dual independent relay mesh (**Cloudflare Workers** + decentralized **Akash Network** nodes) wraps that signature into a Base transaction and pays the `ETH` gas for you.

#### "Wait—Can the Relay Steal My Slip or Change Where the Money Goes?"

**Mathematically impossible.**

Every EIP-712 permit hashes the exact `slipId`, the exact `recipient` or `newHolder` address, the exact `paymentAmount`, the `nonce`, and the `deadline` (clamped against the live Base head block timestamp for L2 clock-skew safety). If a relay changed even a single bit of your destination address, `ecrecover` inside the smart contract would produce a random address and the transaction would revert instantly.

#### "What If Both Relays Go Down at the Same Time?"

Every permit function in `CryptoSlipVaultV32` supports **`relayer = address(0)`**. None of our functions are locked to a private company server. Anyone—your own wallet, a friend's wallet, or any public RPC node—can broadcast your signed permit directly to Base (`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`) without asking us for permission.

---

### Part 2: The Ocean Test — How One Master Key Recovers 1,024 One-Time Slip Keys

Now let's drop your phone off a boat in the Mediterranean.

Before it sank, you had:
* Three Slips you created and hadn't sent yet;
* Five Slips friends sent you that you tapped **Keep in Slips** on (even re-kept across multiple hops);
* Four Change Slips left over from splitting larger Slips at cafés and swapping into `cbBTC`.

Every single one of those 12 Slips was sitting on a **different, isolated one-time address** on Base so nobody could link them to your personal wallet.

When you buy a new phone and enter your **single master recovery key**, how does the app find all 12 Slips without any central database?

Because CryptoSLIP V3.2 never uses random throwaway keys that get lost when your browser cache clears. Instead, it uses a **Deterministic `HMAC-SHA256` Key Tree**:

* **Created Slips (`deriveCreatedSlipKey`):** Derived deterministically from `HMAC-SHA256(masterKey, "created-slot-" + slotIndex)` across slots `0..1023`.
* **Kept Slips & Multi-Hop Re-Keeps (`deriveKeptSlipHopKey`):** Derived deterministically from `HMAC-SHA256(masterKey, chainId + vaultAddress + slipId + hop)` across up to 5 consecutive hops (`MAX_KEPT_SLIP_HOPS = 5`).
* **Split Change & Swap Output Slips (`deriveNextSlipHolderKey`):** Derived deterministically from the parent Slip and your master key.

When you paste your single master key into a brand-new phone (`scanVaultSlipsForWallet`), the phone recomputes the entire key ring in **under 50 milliseconds** in local RAM, scans `CryptoSlipVaultV32` on Base, and pops all 12 of your active Slips right back onto your screen—ready to spend.

* **On-Chain to Strangers:** Your 12 Slips look like 12 completely unrelated one-time addresses with zero connection to each other.
* **To Your Master Key:** All 12 Slips are recoverable from a single backup string.

Now you have a self-custody wallet that needs `0 ETH`, never doxxes your net worth, and survives the bottom of the ocean.

In **Dispatch #05**, we test what happens when there is **no internet at all**—and how a `69-byte` Slip travels 10 kilometers through the air over a **$35 LoRa radio**.
