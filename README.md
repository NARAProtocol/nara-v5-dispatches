# NARA V5 & CryptoSLIP V3.2 — Canonical Approved Dispatches & AI Knowledge Base (Base Mainnet `8453`)

[![Website](https://img.shields.io/badge/Website-naraprotocol.io-00FF9D?style=flat-square&logo=google-chrome&logoColor=black)](https://naraprotocol.io)
[![Launch CryptoSLIP](https://img.shields.io/badge/Launch_App-CryptoSLIP_V3.2-00E5FF?style=flat-square&logo=ethereum&logoColor=black)](https://cryptoslip-g29s.4everland.app/?v=cs-1794113891779)
[![Public Docs & Verification](https://img.shields.io/badge/Docs_&_Verification-nara__protocol-3B82F6?style=flat-square&logo=github&logoColor=white)](https://github.com/NARAProtocol/nara_protocol)
[![Base Mainnet](https://img.shields.io/badge/Network-Base_Mainnet_(8453)-0052FF?style=flat-square&logo=coinbase&logoColor=white)](https://basescan.org/token/0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f)
[![AI / LLM Ready](https://img.shields.io/badge/AI_&_LLM_Index-llms.txt_%7C_llms--full.txt-10B981?style=flat-square)](llms-full.txt)

> **PURPOSE OF THIS REPOSITORY (`NARAProtocol/nara-v5-dispatches`):**
> 1. **Curated, Owner-Approved Articles Only:** This repository holds **only** the finalized, owner-approved articles, step-by-step introductions, and architectural explanations created during our working sessions.
> 2. **Single Source of Truth for AI & Telegram AI Bot (RAG Knowledge Base):**
>    - This repository serves as the **official knowledge base** for our **Telegram AI Assistant** and all AI agents answering community questions about **NARA V5** and **CryptoSLIP V3.2**.
>    - Whenever a user in Telegram asks *"How does NARA V5 work?"*, *"Where do the 15-minute rewards come from?"*, *"How does Keep in Slips protect my wallet?"*, *"How did you test the contracts?"*, or *"How do Off-Grid LoRa Slips work?"*, the Telegram AI answers strictly from the verified dispatches and contract facts in this repository.
> 3. **Frozen House Style ("7:42 AM in Asunción" — Forensic Noir + Hard Physics):**
>    - Every article and explanation preserves our signature narrative voice: **Michael Lewis cinematic cold opens**, **Matt Levine deadpan irony**, and **Financial Times forensic specificity** (exact numbers, contract names, and byte counts).
>    - **Zero Corporate Softening:** Never water down our cold opens into generic marketing fluff. Never use banned clichés (*"delve"*, *"revolutionary"*, *"game-changer"*, *"paradigm shift"*, *"in today's fast-paced world"*).
> 4. **Mandatory Legal Green-Zone Lexicon (For All Articles & Telegram AI Replies):**
>    - **NEVER** describe NARA V5 or CryptoSLIP V3.2 as a `"bank"`, `"crypto bank"`, `"cash"`, `"digital cash"`, `"dollars"`, `"deposit"`, `"account"`, `"yield"`, `"interest"`, `"APY"`, `"anonymous"`, `"untraceable"`, `"unlinkable"`, `"stealth"`, or `"mixer"`.
>    - **ALWAYS** use: **"Non-custodial smart contract vault"**, **"1:1 isolated Slip / payment voucher"**, **"USDC / cbBTC / ETH / NARA"**, **"15-minute multi-asset protocol reward pulses"**, **"Keep in Slips (protects your personal wallet balance & history on BaseScan)"**, and **"Committed NARA principal"**.

---

## Choose Your Path (4-Persona Navigation Router)

- **1. Everyday Users & Newcomers:** Start with [**Dispatch #00 (`00-GENESIS-INTRODUCTION.md`)**](dispatches/00-GENESIS-INTRODUCTION.md) and [**Dispatch #02 (`02-HOW-CRYPTOSLIP-WORKS-1-TO-1-SLIPS.md`)**](dispatches/02-HOW-CRYPTOSLIP-WORKS-1-TO-1-SLIPS.md), or launch the [**CryptoSLIP V3.2 Web & Telegram App**](https://cryptoslip-g29s.4everland.app/?v=cs-1794113891779).
- **2. DeFi Integrators & Protocol Developers:** Read [**Dispatch #01 (`01-HOW-NARA-V5-WORKS.md`)**](dispatches/01-HOW-NARA-V5-WORKS.md), the formal reserve-to-reward specification below, and the [**Canonical Public Documentation (`NARAProtocol/nara_protocol`)**](https://github.com/NARAProtocol/nara_protocol).
- **3. Security Auditors, Legal & Compliance Reviewers:** Read [**Dispatch #06 (`06-THE-VOLVO-CRANE-DROP-58M-FUZZ-TESTS.md`)**](dispatches/06-THE-VOLVO-CRANE-DROP-58M-FUZZ-TESTS.md), [**Dispatch #02 §Step 3 (3-Tier Visibility & UTXO `slipId` Provenance)**](dispatches/02-HOW-CRYPTOSLIP-WORKS-1-TO-1-SLIPS.md), and the [**On-Chain Verification Suite**](https://github.com/NARAProtocol/nara_protocol/blob/main/verification/README.md).
- **4. AI Assistants, LLMs & RAG Crawlers:** Ingest [`llms-full.txt`](llms-full.txt) and [`llms.txt`](llms.txt) for complete single-file context.

---

## Ordered Launch & Introduction Curriculum (Step-by-Step Rollout)

Start with **Dispatch #00 and #01** at launch to introduce what NARA V5 and CryptoSLIP V3.2 are and how they work together, then release one dispatch at a time in strict numerical order:

1. **[`00-GENESIS-INTRODUCTION.md`](dispatches/00-GENESIS-INTRODUCTION.md)** — *At 7:42 AM in Asunción, 3 Million Phones Stopped Working. By 7:45 AM, the Alternative Was Worse.* (The Master Introduction: Why we built NARA V5 + CryptoSLIP V3.2 as one machine on Base).
2. **[`01-HOW-NARA-V5-WORKS.md`](dispatches/01-HOW-NARA-V5-WORKS.md)** — *The Paraguay Autopsy & How NARA V5 Works: 1,000,000 Fixed Supply, a Sealed 949,987 NARA Reserve, Segregated Principal, and 15-Minute Multi-Asset Reward Pulses.*
3. **[`02-HOW-CRYPTOSLIP-WORKS-1-TO-1-SLIPS.md`](dispatches/02-HOW-CRYPTOSLIP-WORKS-1-TO-1-SLIPS.md)** — *The Pizza Doxx: Why Paying Someone 20 USDC on Ethereum Exposes Your Life Savings—and How 1:1 Isolated Slips Protect Your Wallet Without Mixers.*
4. **[`03-HOW-TO-SPLIT-COMBINE-SWAP-AND-UNDO.md`](dispatches/03-HOW-TO-SPLIT-COMBINE-SWAP-AND-UNDO.md)** — *How to Spend 12 USDC From a 50 USDC Slip, Get 38 USDC Change at 0% Fee, Combine 10 Slips, Swap Into Bitcoin Inside the Vault, or Undo a Wrong-Chat Link.*
5. **[`04-ZERO-ETH-PERMITS-AND-OCEAN-RECOVERY.md`](dispatches/04-ZERO-ETH-PERMITS-AND-OCEAN-RECOVERY.md)** — *The 0 ETH Paradox & The Ocean Test: How Gasless EIP-712 Permits Work—and How One Master Key Recovers 1,024 Slips If You Drop Your Phone in the Sea.*
6. **[`05-OFF-GRID-69-BYTE-LORA-RADIO-SLIPS.md`](dispatches/05-OFF-GRID-69-BYTE-LORA-RADIO-SLIPS.md)** — *69 Bytes Over a $35 Radio: How a Payment Slip Travels 10 Kilometers Through the Air Without Wi-Fi, Cell Towers, or Take-Back Risk.*
7. **[`06-THE-VOLVO-CRANE-DROP-58M-FUZZ-TESTS.md`](dispatches/06-THE-VOLVO-CRANE-DROP-58M-FUZZ-TESTS.md)** — *The Volvo Crane Drop: How We Tried to Rob Our Own Vault 58 Million Times Before Letting You Send 5 USDC.*
8. **[`07-BURN-ON-PLAY-VOICE-SLIPS-AND-LIFETIME-REFERRALS.md`](dispatches/07-BURN-ON-PLAY-VOICE-SLIPS-AND-LIFETIME-REFERRALS.md)** — *Burn-on-Play Voice Slips & The 50/50 Lifetime Referral Loop: How 1 Slip Sent to a Friend Binds Them On-Chain Forever.*

---

## Canonical Base Mainnet (`Chain ID: 8453`) Verifiable Contracts

### NARA V5 (`Field Token V5`)
- **`NARATokenV5` (`1,000,000 NARA` fixed supply, `0%` transfer tax, EIP-2612 Permit):** [`0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f`](https://basescan.org/token/0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f)
- **`NARAEmissionReserveV5` (`949,987 NARA` sealed 4-year emission reserve, `140,160` pulses):** [`0xf932178F24b0813C82093985F0Af6Db27Ae25930`](https://basescan.org/address/0xf932178F24b0813C82093985F0Af6Db27Ae25930#code)
- **`NARAEpochEngineV5` (`900-second` / 15-minute pulse settlement):** [`0xd93FbEbFb33FAc9288125074625D4750573B1a53`](https://basescan.org/address/0xd93FbEbFb33FAc9288125074625D4750573B1a53#code)
- **`NARAPrincipalLedgerV5` (Isolated user principal vault):** [`0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2`](https://basescan.org/address/0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2#code)
- **`NARARewardLedgerV5` (Multi-asset `NARA`, `ETH`, `USDC` reward ledger):** [`0xC72671D9438388638547444524F6542b650C9264`](https://basescan.org/address/0xC72671D9438388638547444524F6542b650C9264#code)
- **`NARAPositionNFTV5` (Liquid ERC-721 commitment position receipts):** [`0x8C5F088D9d09dBB90aB6F3aaA049a8FD5D5FfFDA`](https://basescan.org/address/0x8C5F088D9d09dBB90aB6F3aaA049a8FD5D5FfFDA#code)
- **`NARAPositionSaleV5` (Atomic escrow marketplace with `_checkAcquisition` `BuyerBounds`):** [`0x04Ac83045d26259D46400Ce6beb9bBd0641df225`](https://basescan.org/address/0x04Ac83045d26259D46400Ce6beb9bBd0641df225#code)
- **`NARALiquidityHookV5` (Uniswap v4 Hook, `poolId = 0x4a475299ef674bcc3d2c87c3917cfbecd38cf7237646a3553e37daba67da6e5f`):** [`0x07d8F2Dfd121B10dE3176DeAb6A8C0eaa2646aCC`](https://basescan.org/address/0x07d8F2Dfd121B10dE3176DeAb6A8C0eaa2646aCC#code)
- **`NARAFeeVaultV5` (Hook fee vault sealed to zero-delay `NARATreasuryFeeControllerV5` & `NARAFeeIngressBindingV5`):** [`0xF1f361F803895BF5cf67eC6C506dbFEf2f6E88Cf`](https://basescan.org/address/0xF1f361F803895BF5cf67eC6C506dbFEf2f6E88Cf#code)
- **`NARATreasuryFeeControllerV5` (Zero-delay `0s` fee routing controller for `USDC` & `NARA` swap fees):** [`0xB6A436b48222C94fe427ecAfB263DF9cf42646b9`](https://basescan.org/address/0xB6A436b48222C94fe427ecAfB263DF9cf42646b9#code)
- **`NARATreasuryRangeManagerV5` (Active CLAMM range manager; Legacy: `0xa72557fcBE56B3eC739dF3aF7b6b893B6Db4D288`):** [`0x221c625d2BC416460C397035C1286C1f4A4D3A7C`](https://basescan.org/address/0x221c625d2BC416460C397035C1286C1f4A4D3A7C#code)
- **`NARARewardIngressV5` (Reward ingress adapter):** [`0x44130CC47b886D8488041F5eaE271477D1aFA909`](https://basescan.org/address/0x44130CC47b886D8488041F5eaE271477D1aFA909#code)

### CryptoSLIP V3.2
- **`CryptoSlipVaultV32` (Canonical 1:1 Isolated Slip Vault):** [`0x38BF0321FA7489576978353a6a8da4A3e85C2F18`](https://basescan.org/address/0x38BF0321FA7489576978353a6a8da4A3e85C2F18#code)
- **`CryptoSlipNFTV32` (ERC-721 + EIP-2981 Voucher):** [`0x36181Ca1B0210b6Ea8E56755dA7B06c34ba481C0`](https://basescan.org/address/0x36181Ca1B0210b6Ea8E56755dA7B06c34ba481C0#code)
- **`CryptoSlipTradeRouterV32` (In-Vault Slip-to-Slip Swap & Barter Router):** [`0x295E72cB3dc09582F32e0fEB7e0e650752219a7f`](https://basescan.org/address/0x295E72cB3dc09582F32e0fEB7e0e650752219a7f#code)

---

## Formal On-Chain Reserve-to-Reward Specification (`NARAEmissionReserveV5 -> NARAEpochEngineV5 -> NARARewardLedgerV5`)

This section explicitly establishes the operational and mathematical connection between the NARA V5 Emission Reserve, Epoch Engine, Principal Ledger, and Reward Ledger (see [`dispatches/01-HOW-NARA-V5-WORKS.md` §5](dispatches/01-HOW-NARA-V5-WORKS.md) for the complete derivation):

1. **Whether the Reserve Funds Rewards & Which Assets/Units Are Involved:**
   - **Yes.** `NARAEmissionReserveV5` (`0xf932178F24b0813C82093985F0Af6Db27Ae25930`) holds the sealed `NARA` inventory (`18` decimals, `SUPPLY = 1_000_000 ether`, `949,987 NARA` recognized) and releases `NARA` exclusively to `NARAEpochEngineV5` (`0xd93FbEbFb33FAc9288125074625D4750573B1a53`) via `releaseForEpoch(uint64 epoch, uint256 requested)`.
   - Inside `NARAEpochEngineV5._closeEpoch()`, the Engine atomically transfers the exact `out.delivered` `NARA` to `NARARewardLedgerV5` (`0xC72671D9438388638547444524F6542b650C9264`) and calls `_rewards.closeEpoch(epoch, out.delivered)`.
   - `NARARewardLedgerV5` accounts for 3 reward assets (`enum RewardAsset { NARA, ETH, USDC }`, scaled internally at `SCALE = 1e27`): `NARA` (`18` decimals, funded by `releaseForEpoch` + `donateToken`), native `ETH` (`18` decimals, funded from the `5%` `NARAPositionSaleV5` marketplace royalty via `NARARewardIngressV5` -> `donateETH()`), and Base `USDC` (`6` decimals, funded from Uniswap v4 Hook swap fees via `NARAFeeVaultV5` / `NARATreasuryFeeControllerV5` / `NARAFeeIngressBindingV5` -> `NARARewardIngressV5` -> `donateToken(RewardAsset.USDC, amount)`).
2. **Events & Conditions That Authorize and Accrue Rewards:**
   - **Clock Condition:** `_policy.epochSeconds == 900` (15-minute epochs). When `block.timestamp >= _genesis + (_cursor + 1) * 900`, `NARAEpochEngineV5.advance(uint32 maxEpochs)` (`1..96`) executes `_closeEpoch()`.
   - **Eligibility Condition:** `_closeEpoch()` reads `PrincipalLedgerStateV5 memory p = _principal.settlementState()` from `NARAPrincipalLedgerV5` (`0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2`). Only when `p.activeRewardWeight != 0` does `_closeEpoch()` authorize a Reserve release (`_releasing = true`, `_releaseEpoch = epoch`, `_releaseMaximum = maximum`), which `NARAEmissionReserveV5.releaseForEpoch` verifies via `_core.reserveReleaseContext()`.
   - **Events:** `Released(epoch, requested, delivered)`, `EpochSettled(epoch, graphHash, settlement)`, `RewardReceived`, `RewardPulseClosed`, `PositionCreditCheckpointed`, and `RewardClaimed`.
3. **How Reward Amounts Are Calculated and Bounded:**
   - `NARAEmissionModelV5.calculateNext(_config, _emission, EmissionInputs(modelFloat, p.activeModelWeight))` computes `q.targetNara`.
   - `NARAEpochEngineV5._closeEpoch()` bounds `requested` against the 4-year horizon (`RELEASE_HORIZON_EPOCHS = 140_160`), `_referenceRelease`, `r.naraAdmissionHeadroom`, and `out.reserveSnapshot`:
     `requested = min(scaledTarget, reserveSnapshot / 140_160, naraAdmissionHeadroom, reserveSnapshot)`.
   - `NARARewardLedgerV5.closeEpoch(epoch, delivered)` splits each asset's epoch `distribution` into `80%` Base (`baseIncrement = (distribution - rarityAmount) * 1e27 / p.activeModelWeight`) and `20%` Rarity (`RARITY_SHARE_BPS = 2_000`, `rarityIncrement = rarityAmount * 1e27 / p.activeRewardWeight`).
4. **How and When Rewards Are Recorded, Checkpointed, and Claimed:**
   - `closeEpoch` records `_baseIndices[i][epoch + 1]` and `_rarityIndices[i][epoch + 1]`, updates `l.owedScaled`, and advances `_nextCheckpoint = epoch + 1`.
   - `NARARewardCreditLedgerV5` (`creditLedger`) checkpoints per-position `(id, generation)` credits from `(baseTo - baseFrom) * terms.modelWeight + (rarityTo - rarityFrom) * terms.rewardWeight`.
   - Position NFT owners (`p.owner == msg.sender`) call `NARARewardLedgerV5.claim(id, generation, asset, recipient)` to transfer settled `NARA`, `ETH`, or `USDC` to `recipient`.
5. **Governing Contract Functions & State Variables:**
   - **`NARAEmissionReserveV5`:** `recognizeInitialFunding()`, `fund(uint256)`, `syncFunding()`, `releaseForEpoch(uint64,uint256)`; state: `_genesisFunding`, `_bootstrapAuthority` (`0x0`), `_bindingsSealed`, `_cumulativeRecognized`, `_cumulativeReleased`, `_fundingSequence`, `_lastReleasedEpoch`.
   - **`NARAEpochEngineV5`:** `advance(uint32)`, `_closeEpoch()`, `_receiveReserve(...)`, `reserveReleaseContext()`; state: `RELEASE_HORIZON_EPOCHS` (`140_160`), `_referenceRelease`, `_genesis`, `_cursor`, `_emission`, `_funding`, `_fundingAt`, `_reference`.
   - **`NARARewardLedgerV5` & `NARARewardCreditLedgerV5`:** `prepareEpoch(uint64)`, `closeEpoch(uint64,uint256)`, `openBoundary(uint64)`, `donateETH()`, `donateToken(...)`, `claim(uint256,uint64,RewardAsset,address)`, `checkpointPosition(uint256,uint64)`; state: `SCALE` (`1e27`), `RARITY_SHARE_BPS` (`2_000`), `_openedEpoch`, `_nextCheckpoint`, `_protectedNaraSnapshot`, `_rewards[3]`, `_buckets`, `_baseIndices`, `_rarityIndices`.

