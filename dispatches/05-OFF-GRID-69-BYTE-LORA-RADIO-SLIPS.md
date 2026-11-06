# DISPATCH #05: 69 Bytes Over a $35 Radio — What Happens When They Switch Off the Internet, and How a Slip Travels 10 Kilometers Through the Air Without Cell Towers

**Release Order:** `#05 — Step 5 After Launch (Off-Grid QR, No Take-Back Locks & 69-Byte LoRa Radio Slips)`  
**Primary Goal:** Explain how CryptoSLIP V3.2 solves offline, blackout, and remote payments using **`69-byte` code-mode payloads**, **$35 Meshtastic LoRa radios**, `scrypt` 12-character Claim Codes, and immutable **`No take-back` (`cancelLockedUntil = 9_999_999_999`)** on-chain locks.

---

## Full Article Text (Frozen House Style)

Ask any Bitcoin or Ethereum maximalist what happens to their "unstoppable money" if the local cell tower goes down during a storm, at a crowded music festival, on a boat 15 miles offshore, or during a nationwide telecom blackout.

The answer is embarrassing: **their screen spins forever, and nobody can buy a loaf of bread**.

Why? Because a standard Ethereum or Bitcoin transaction is between **250 and 600 bytes** of raw signed data and requires a live two-way handshake with a mempool node to check nonces, gas prices, and balances before a seller knows if they actually got paid.

Now look at the physical law of off-grid radio communication.

Millions of hikers, preppers, rural communities, and off-grid engineers around the world carry **$35 Meshtastic LoRa radios** (`Heltec V3`, `LILYGO T-Beam`, `RAK WisBlock`) that hop text messages from hilltop to hilltop over 10 to 30 kilometers using zero cell towers, zero Wi-Fi, and zero satellites.

But LoRa radio physics has an iron ceiling: **a single Meshtastic text packet can only carry 200 bytes**.

Try to push a 500-byte Ethereum transaction or a bloated wallet URL through a 200-byte LoRa radio channel, and the packet drops dead in the air.

When we designed **CryptoSLIP V3.2**, we engineered the payment link to obey the laws of radio physics.

---

### 1. 69 Bytes: Fitting Inside a Single LoRa Radio Packet With 131 Bytes to Spare

In **Code Mode (`Needs a code`)**, a complete CryptoSLIP V3.2 payload (`encodeSlipPayload`)—carrying the vault version, the `slipId`, the exact amount, the token (`USDC`, `cbBTC`, `ETH`, or `NARA`), the off-grid lock flag (`_l1`), and the cryptographic `scrypt` salt—is **69 bytes**.

That is **one-third of a single 200-byte Meshtastic LoRa packet**.

Before you leave home (or while your camp still has signal), you pre-mint three or four Off-Grid Slips (`10 USDC`, `20 USDC`, `50 USDC`, or `10 NARA`) on your phone.

Later, when there is zero internet for 50 kilometers in every direction:
* **Face-to-Face:** You hold up the **Off-Grid QR Code** on your screen. The seller scans it in Airplane Mode in 200 milliseconds.
* **Across a Valley or Town:** You paste the **69-byte Slip string** into Meshtastic and beam it 10 kilometers over 915 MHz / 868 MHz LoRa radio to the recipient's phone, then tell them the 12-character Claim Code over the radio or in person.

---

### 2. The Offline Double-Spend Problem: "What Stops the Buyer From Pressing 'Take Back'?"

Here is where every previous "offline crypto" idea failed.

Suppose a buyer hands you a payment link or QR code at a flea market where the internet is spotty. You give them the watch or the sack of flour. Two minutes later, the buyer walks around the corner, catches a bar of 5G signal before you do, and hits **Cancel / Take back**—or tries to **Swap** or **Combine** that Slip back to themselves—leaving you with an empty link.

How do you prove—with zero internet—that the buyer **cannot** yank the money back?

In CryptoSLIP V3.2, when you tick **Off-grid QR (`✓ No take-back`)**, the smart contract writes:

```solidity
cancelLockedUntil = 9_999_999_999; // Year 2286
```

Look at what `CryptoSlipVaultV32` and `CryptoSlipTradeRouterV32` enforce on-chain the millisecond that flag is set:

1. **The Buyer's "Take Back" Button Is Permanently Bricked:** Every single refund path (`refundSlip`, `refundWithPermit`, `redirectRefund`) reverts with `CancelLocked()`.
2. **Self-Claim, Self-Split, Self-Combine & Self-Swap Are Blocked:** Even if the buyer keeps a copy of the QR code and tries to cash, split, combine, or swap that Slip back to their own wallet (`recipient == creator` or `refundBeneficiary`), both the Vault and the Trade Router reject the transaction on-chain.
3. **Offline Auto-Queue on the Seller's Phone:** When the seller scans your Off-Grid QR code in Airplane Mode, their app verifies the cryptographic structure locally (`scrypt` `N = 16384, r = 8, p = 1`) and saves the Slip as **`Active (Saved, not secured)`**. The exact second the seller's phone catches a bar of signal hours later, `reconcilePendingOps` wakes up in the background and automatically fires `acceptWithPermit` (`Keep in Slips`) to rotate the Slip onto the seller's private key.

| Capability | Standard Crypto Wallet | Legacy Link Apps | CryptoSLIP V3.2 Off-Grid Mode |
| :--- | :---: | :---: | :---: |
| **Payload Size** | `300–600 bytes` (Raw Tx) | `180–300+ bytes` | **`69 bytes` (Code Mode)** |
| **Fits in 1 Meshtastic LoRa Packet (`<200B`)?** | No | Rarely / Fragile | **Yes (131 bytes to spare)** |
| **Protection Against Buyer "Take-Back"** | None offline | None (Buyer can cancel) | **`cancelLockedUntil = 9_999_999_999` (Year 2286)** |
| **Brute-Force Resistance on Code** | N/A | Weak | **`scrypt (N=16384, r=8, p=1)` + `PBKDF2 210k`** |
| **Offline Scan -> Auto-Lock on Signal** | Fails immediately | Fails immediately | **Auto-secures via `reconcilePendingOps`** |

Wait a second.

What if a nation-state hacker or a clever Solidity developer tries a 4-step trick—like splitting 1 wei off an Off-Grid Slip, or passing it through a 2-hop DEX swap—to strip off that `No take-back` lock?

In **Dispatch #06**, we show you how we dropped our Vault off a 30-meter crane **58 million times** to make sure every single trick hits a concrete wall.
