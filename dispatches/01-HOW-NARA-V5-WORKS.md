# DISPATCH #01: The Paraguay Autopsy & How NARA V5 Works — A Sealed 949,987 NARA Reserve, Segregated Principal, and 15-Minute Multi-Asset Pulses

**Release Order:** `#01 — Step 1 After Launch (How NARA V5 Works)`  
**Primary Goal:** Explain clearly how **NARA V5** works on Base Mainnet (`0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f`) by contrasting human-controlled balance sheets with NARA V5's four immutable smart contract boundaries.

---

## Full Article Text (Frozen House Style)

When auditors finally pried open the books of Grupo Vázquez in Asunción on the morning of November 3, 2026, they didn't find a sophisticated cyberattack.

They found a door that had been left unlocked on purpose.

Roughly **380 million (₲ 1.74 trillion)** from the IPS state workers' retirement fund had been parked inside Ueno. From that same pool, **150 million** had been quietly routed as loans to companies owned by the group's own executives. And when citizens asked why the banking superintendent hadn't blown the whistle six months earlier, investigators discovered that the chief regulator’s wife was drawing a monthly salary of **₲ 45.8 million ($6,000)** on the conglomerate's payroll.

People read stories like that and think the problem is "bad executives."

It isn't. The problem is **commingled architecture**. Whenever the people who run a financial system hold a key that can print new units, pause withdrawals, or dip into the same bucket where customer principal lives, someone eventually turns the key.

When we engineered **NARA V5** on Base Mainnet (`Chain ID: 8453`), we asked a simple engineering question:

*How do you build a value protocol where even the creators cannot touch the reserve, cannot touch user principal, and cannot alter the reward clock?*

Here is how NARA V5 works under the hood—step by step.

---

### 1. The Fixed Ceiling & The Welded Emission Reserve (`949,987 NARA`)

Most tokens fail before they are six months old because a team multisig holds a `mint()` function or an "ecosystem treasury" that dumps on holders whenever the founders want a new office.

In **NARA V5** (`NARATokenV5` at `0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f`):

* **Hard Cap of `1,000,000 NARA`:** Every single token was minted once at genesis. There is no `mint()` function left in the token contract, and transfers carry a **`0%` tax**.
* **`949,987 NARA` Sealed Inside `NARAEmissionReserveV5` (`0xf932178F24b0813C82093985F0Af6Db27Ae25930`):** Nearly 95% of the entire supply was locked inside a dedicated emission vault at `fundingSequence = 2`, governed by a hard 4-year horizon cap of **`140,160` pulses**.
* **The Setup Key Was Burned:** Once the reserve was sealed, `_bootstrapAuthority` was permanently set to `0x0000000000000000000000000000000000000000`. Nobody can accelerate emissions, mint a bonus token, or pull reserves out the back door.

---

### 2. Two Separate Steel Boxes: Why Your Principal Can Never Be Lent Out

In traditional finance—and in most DeFi vaults—your principal and the protocol's reward pool sit inside the same contract. If the reward math has a bug or an insider makes a bad trade, your principal pays the bill.

NARA V5 physically separates them into two different smart contracts on Base:

1. **Box #1 — `NARAPrincipalLedgerV5` (`0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2`):** When you commit `NARA` for a timed position, your tokens sit inside this isolated ledger. It has no external lending hook, no flash-loan function, and no admin sweep. When your commitment matures, you withdraw your exact `NARA` principal 1-to-1.
2. **Box #2 — `NARARewardLedgerV5` (`0xC72671D9438388638547444524F6542b650C9264`):** All 15-minute protocol rewards are accounted for and claimed from a completely separate ledger.

Even if the sky fell on the reward engine tomorrow, `NARAPrincipalLedgerV5` only knows how to do one thing: hand your committed `NARA` back to you.

---

### 3. The 900-Second Heartbeat: Multi-Asset Pulses in `NARA`, `ETH`, and `USDC`

Instead of making participants wait months or years to see protocol activity, `NARAEpochEngineV5` (`0xd93FbEbFb33FAc9288125074625D4750573B1a53`) runs on a strict **15-minute (`900-second`) clock**.

Every 15 minutes, a new pulse settles across three real assets simultaneously:
* **`NARA`** released deterministically from the sealed emission reserve;
* **`ETH`** and **`USDC`** routed from autonomous Uniswap v4 pool fee activity (`NARALiquidityHookV5` at `0x07d8F2Dfd121B10dE3176DeAb6A8C0eaa2646aCC` and `NARAFeeVaultV5` at `0xF1f361F803895BF5cf67eC6C506dbFEf2f6E88Cf`).

And because NARA V5 is wired directly into **CryptoSLIP V3.2**, you can claim your accrued `NARA`, `ETH`, or `USDC` pulse rewards and wrap them straight into a shareable payment Slip in a single flow.

---

### 4. What If You Need Liquidity Before Your Position Matures? (`NARAPositionNFTV5` + `BuyerBounds`)

In legacy staking contracts, locking tokens for 90 or 365 days meant your capital was frozen solid until the timer expired—or worse, you were given a "liquid wrapper token" that depegged the first time the market panicked.

In NARA V5, every committed position is represented by a live on-chain ERC-721 receipt: **`NARAPositionNFTV5` (`0x8C5F088D9d09dBB90aB6F3aaA049a8FD5D5FfFDA`)**.

If you locked `NARA` for 180 days and need `ETH` on Day 40, you don't break the vault. You list your position NFT on the built-in atomic escrow market (**`NARAPositionSaleV5`** at `0x04Ac83045d26259D46400Ce6beb9bBd0641df225`).

And what stops a dishonest seller from listing a position full of unclaimed `NARA`, `ETH`, and `USDC` rewards, waiting for you to click **Buy**, and front-running your transaction in the mempool to drain the rewards a split-second before the NFT transfers to you?

Every purchase enforces on-chain **`BuyerBounds`** inside `_checkAcquisition`:
* Your transaction locks in the exact `expectedListingId`, `minPrincipal`, `minClaimable[NARA, ETH, USDC]`, `minRewardCheckpoint`, and `minRarityTier` before it leaves your phone.
* If the seller touches a single wei of principal or rewards before your purchase settles, the entire transaction reverts atomically on Base and your `ETH` never leaves your wallet.

| Feature | Traditional App / Legacy DeFi | NARA V5 on Base (`8453`) |
| :--- | :--- | :--- |
| **Supply & Mint Key** | Unlimited or admin-controlled multisig | **`1,000,000 NARA` fixed**, `_bootstrapAuthority = 0x0` |
| **Customer Principal** | Commingled with lending & operating funds | Isolated in **`NARAPrincipalLedgerV5`** |
| **Reward Cadence** | Arbitrary or inflationary paper points | **Every 15 minutes (`900s`)** in **`NARA`, `ETH`, `USDC`** |
| **Early Exit** | Frozen or fragile wrapper token | Atomic **`NARAPositionSaleV5`** NFT market with **`BuyerBounds`** |
| **Spend / Send Rewards** | Requires manual wallet withdrawals & gas | **1-Tap Harvest directly into a CryptoSLIP link** |

---

### 5. Formal On-Chain Specification: How `NARAEmissionReserveV5` Funds `NARARewardLedgerV5` (Functions, State Variables & Equations)

For auditors, researchers, and automated verification engines, here is the exact contract-level specification governing how reserve assets become claimable rewards on Base Mainnet (`Chain ID: 8453`):

#### 5.1 Does the Reserve Fund Rewards, and Which Assets & Units Are Involved?
* **Yes.** `NARAEmissionReserveV5` (`0xf932178F24b0813C82093985F0Af6Db27Ae25930`) holds the protocol's sealed `NARA` emission inventory (`SUPPLY = 1_000_000 ether`, `18` decimals; `949,987 NARA` recognized at `fundingSequence = 2`) and releases `NARA` **exclusively** to `NARAEpochEngineV5` (`0xd93FbEbFb33FAc9288125074625D4750573B1a53`).
* Inside `NARAEpochEngineV5._closeEpoch()`, the Engine receives `out.delivered` from the Reserve and **atomically forwards the exact `delivered` `NARA` amount to `NARARewardLedgerV5` (`0xC72671D9438388638547444524F6542b650C9264`) in the same transaction**:
  ```solidity
  if (out.requested != 0) (out.delivered, out.status) = _receiveReserve(epoch, out.requested, maximum, beforeEngine);
  if (out.delivered != 0) IERC20(address(_nara)).safeTransfer(address(_rewards), out.delivered);
  if (_nara.balanceOf(address(this)) != beforeEngine) revert InvalidRewardForward();
  _rewards.closeEpoch(epoch, out.delivered);
  ```
* **Three Supported Reward Assets (`enum RewardAsset { NARA, ETH, USDC }`, scaled internally at `SCALE = 1e27`):**
  1. **`RewardAsset.NARA` (`index 0`, `18` decimals, `0x91FB78A9F46B67f54BbD4489941d19666b2fAe0f`):** Funded per epoch from `NARAEmissionReserveV5.releaseForEpoch(epoch, requested)` plus any direct `NARARewardLedgerV5.donateToken(RewardAsset.NARA, amount)` receipts.
  2. **`RewardAsset.ETH` (`index 1`, `18` decimals, native `address(0)`):** Funded via `NARARewardIngressV5` (`0x44130CC47b886D8488041F5eaE271477D1aFA909`) and `NARAFeeVaultV5` (`0xF1f361F803895BF5cf67eC6C506dbFEf2f6E88Cf`) calling `NARARewardLedgerV5.donateETH()`.
  3. **`RewardAsset.USDC` (`index 2`, `6` decimals, Base `USDC` `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`):** Funded via `NARARewardLedgerV5.donateToken(RewardAsset.USDC, amount)`.

#### 5.2 What Events & Conditions Authorize and Accrue Rewards?
1. **Wall-Clock Epoch Boundary (`900 seconds` / 15 minutes):**
   - `NARAEpochEngineV5.clock()` computes `wallEpoch = (block.timestamp - _genesis) / 900`.
   - Whenever `_cursor < c.wallEpoch`, any caller can invoke `NARAEpochEngineV5.advance(uint32 maxEpochs)` (`1 <= maxEpochs <= _policy.maxAdvance` where `maxAdvance = 96`), which loops `_closeEpoch()` to settle completed 15-minute epochs.
2. **Active Reward Weight Eligibility (`p.activeRewardWeight != 0`):**
   - `_closeEpoch()` reads `PrincipalLedgerStateV5 memory p = _principal.settlementState()` from `NARAPrincipalLedgerV5` (`0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2`) and calls `_rewards.prepareEpoch(epoch)` to snapshot `_receiptBefore = IERC20(nara).balanceOf(address(this))`.
   - Only when `p.activeRewardWeight != 0` (meaning at least one committed position in `NARAPrincipalLedgerV5` is active during `epoch`) does `_closeEpoch()` compute a positive `out.requested` and authorize a Reserve release.
   - If `p.activeRewardWeight == 0`, `out.requested = 0` (no `NARA` is pulled from `NARAEmissionReserveV5`), and any external `ETH`/`USDC`/`NARA` bucket receipts for that epoch are rolled forward in `l.banked = distribution` without loss.
3. **Transient Handshake Authorization (`reserveReleaseContext`):**
   - Immediately before calling the Reserve, `NARAEpochEngineV5._receiveReserve` sets transient storage flags `_releasing = true`, `_releaseEpoch = epoch`, and `_releaseMaximum = maximum`.
   - `NARAEmissionReserveV5.releaseForEpoch(uint64 epoch, uint256 requested)` enforces `msg.sender == address(_core)` and queries `_core.reserveReleaseContext()` to verify `active && authorizedEpoch == epoch && requested <= maximum` and `epoch == c.nextEpochToClose && epoch < c.wallEpoch`.
4. **On-Chain Events Emitted:**
   - `NARAEmissionReserveV5`: `InitialFundingRecognized(sequence, amount)`, `FundingRecognized(sequence, eligibleEpoch, funder, amount, directSync)`, `Released(epoch, requested, delivered)`.
   - `NARAEpochEngineV5`: `ReserveFundingRecorded(sequence, eligibleEpoch, amount)`, `EpochSettled(epoch, graphHash, settlement)`.
   - `NARARewardLedgerV5`: `RewardReceived(asset, payer, epoch, amount)`, `RewardPulseClosed(epoch, delivered, baseWeight, rarityWeight, allocation, baseIndexDelta, rarityIndexDelta)`, `PositionCreditCheckpointed(positionId, generation, through)`, `RewardClaimed(positionId, generation, asset, recipient, amount)`.

#### 5.3 How Reward Amounts Are Calculated and Bounded
1. **Step 1 — Adaptive Target Calculation (`NARAEmissionModelV5.calculateNext`):**
   - `NARAEpochEngineV5._closeEpoch()` computes circulating float outside protocol custody:
     $$\text{modelFloat} = \text{SUPPLY} - \text{principalSnapshot} - \text{protectedRewardSnapshot} - \text{reserveSnapshot}$$
   - It passes `EmissionInputs(modelFloat, p.activeModelWeight)` to `NARAEmissionModelV5.calculateNext(_config, _emission, inputs)` (`WAD = 1e18`), which calculates:
     - $\text{weightedLockShare} = \lfloor \frac{\text{modelWeight} \times 10^{18}}{\text{modelFloat} + \text{modelWeight} + \text{bootstrap}} \rfloor$
     - $\text{baseEmission} = \text{bound}(\lfloor \frac{\text{prev.baseEmission} \times \text{growthFactorWad}}{10^{18}} \rfloor, \text{minBaseEmission}, \text{maxBaseEmission})$
     - $\text{emission} = \text{bound}(\lfloor \frac{\text{baseEmission} \times (\text{incentive} - \text{penalty})}{10^{18}} \rfloor, 0, \text{maxBaseEmission})$
     - $\text{admitted} = \lfloor \frac{\text{emission} \times \text{retention}}{10^{18}} \rfloor$, $\text{dripTarget} = \lfloor \frac{\lfloor \text{admitted} \times \text{dripSplitWad} / 10^{18} \rfloor \times \text{warmup}}{10^{18}} \rfloor$
     - $\text{targetNara} = \lfloor \frac{\text{dripTarget} \times \text{modelWeight}}{\text{modelWeight} + \text{bootstrap}} \rfloor$
2. **Step 2 — 4-Year Horizon & Reserve Cap (`NARAEpochEngineV5._closeEpoch` lines 353–362):**
   - `RELEASE_HORIZON_EPOCHS = 140_160` (`4 years` of `900s` epochs) and `_referenceRelease = RELEASE_HORIZON_EPOCHS * (config.maxBaseEmission * config.dripSplitWad / 1e18)`.
   - The raw model target `q.targetNara` is bounded by three hard ceilings:
     $$\text{scaled} = \begin{cases} \lfloor \frac{\text{targetNara} \times \text{reserveSnapshot}}{\text{referenceRelease}} \rfloor & \text{if } \text{reserveSnapshot} < \text{referenceRelease} \\ \text{targetNara} & \text{otherwise} \end{cases}$$
     $$\text{tail} = \left\lfloor \frac{\text{reserveSnapshot}}{140,160} \right\rfloor, \qquad \text{maximum} = \min(\text{naraAdmissionHeadroom}, \text{reserveSnapshot})$$
     $$\text{requested} = \min(\text{scaled}, \text{tail}, \text{maximum})$$
   - Inside `NARAEmissionReserveV5.releaseForEpoch`, `delivered = min(requested, _cumulativeRecognized - _cumulativeReleased, _nara.balanceOf(address(this)))`.
3. **Step 3 — `80%` Base Weight vs. `20%` Rarity Weight Split (`NARARewardLedgerV5.closeEpoch` lines 190–210):**
   - For each asset `i` (`0 = NARA`, `1 = ETH`, `2 = USDC`), total epoch distribution is `distribution = _buckets[i][epoch] + l.banked + (i == 0 ? delivered : 0)`.
   - `RARITY_SHARE_BPS = 2_000` (`20%`) and `SCALE = 1e27`:
     $$\text{rarityAmount} = \left\lfloor \frac{\text{distribution} \times 2000}{10000} \right\rfloor$$
     $$\Delta\text{baseIndex}_i = \left\lfloor \frac{(\text{distribution} - \text{rarityAmount}) \times 10^{27}}{\text{activeModelWeight}} \right\rfloor, \qquad \Delta\text{rarityIndex}_i = \left\lfloor \frac{\text{rarityAmount} \times 10^{27}}{\text{activeRewardWeight}} \right\rfloor$$
   - Any integer division remainder (`scaled - assigned`) is retained inside `l.quarantinedScaled` so `recordedCustody` and scaled liabilities stay exact to the single wei.

#### 5.4 How and When Rewards Are Recorded, Checkpointed, and Claimed
1. **Epoch Index Snapshot:** At the end of `NARARewardLedgerV5.closeEpoch(epoch, delivered)`, cumulative indices are written to `_baseIndices[i][epoch + 1]` and `_rarityIndices[i][epoch + 1]`, and `_nextCheckpoint` advances to `epoch + 1`.
2. **Position Credit Checkpointing (`checkpointPosition` & `NARARewardCreditLedgerV5`):**
   - Each position (`id`, `generation`) in `NARAPrincipalLedgerV5` has canonical `RewardTermsV5` (`owner`, `startEpoch`, `endEpoch`, `modelWeight`, `rewardWeight`).
   - `NARARewardCreditLedgerV5` (`creditLedger`) reads `NARARewardLedgerV5.creditIndices(asset, start, end)` over settled epochs `[start, min(end, _nextCheckpoint)]` and accumulates:
     $$\text{earnedScaled} = (\text{baseTo} - \text{baseFrom}) \times \text{modelWeight} + (\text{rarityTo} - \text{rarityFrom}) \times \text{rewardWeight}$$
3. **Claim Execution (`NARARewardLedgerV5.claim`):**
   - The current holder of `NARAPositionNFTV5` (`p.owner == msg.sender`) calls `NARARewardLedgerV5.claim(uint256 id, uint64 generation, RewardAsset asset, address recipient)`.
   - `creditLedger.consumeClaim(id, generation, asset, p, _nextCheckpoint)` converts settled `unpaidScaled / 1e27` into whole token units `paid`, retaining `unpaidScaled % 1e27` fractional dust for future epochs.
   - `NARARewardLedgerV5` decrements `l.owedScaled -= paid * 1e27` and `l.recordedCustody -= paid`, increments `l.cumulativePaid += paid`, transfers `paid` (`NARA` via `safeTransfer`, `ETH` via native call, or `USDC` via `safeTransfer`) to `recipient`, and emits `RewardClaimed`.

#### 5.5 Master Contract Function & State Variable Governance Matrix

| Component | Contract Address (Base `8453`) | Governing Functions | Governing State Variables |
| :--- | :--- | :--- | :--- |
| **Emission Reserve** | `NARAEmissionReserveV5` (`0xf932178F24b0813C82093985F0Af6Db27Ae25930`) | `recognizeInitialFunding()`, `fund(uint256)`, `syncFunding()`, `releaseForEpoch(uint64,uint256)`, `state()`, `custody()` | `_genesisFunding`, `_bootstrapAuthority` (`0x0`), `_bindingsSealed`, `_cumulativeRecognized`, `_cumulativeReleased`, `_fundingSequence`, `_lastReleasedEpoch` |
| **Epoch Engine** | `NARAEpochEngineV5` (`0xd93FbEbFb33FAc9288125074625D4750573B1a53`) | `advance(uint32)`, `_closeEpoch()`, `_receiveReserve(...)`, `reserveReleaseContext()`, `recordInitialReserveFunding(...)`, `recordReserveFunding(...)`, `clock()` | `RELEASE_HORIZON_EPOCHS` (`140_160`), `_referenceRelease`, `_genesis`, `_cursor`, `_emission`, `_funding` (`eligibleReserve`, `futureReserve`, `cumulativeRecognized`, `cumulativeReleased`), `_fundingAt[epoch]`, `_reference` |
| **Adaptive Model** | `NARAEmissionModelV5` | `calculateNext(EmissionConfig,EmissionState,EmissionInputs)`, `validateConfig(EmissionConfig)` | Pure stateless contract (`FIXED_SUPPLY = 1_000_000 ether`, `WAD = 1e18`) |
| **Principal Ledger** | `NARAPrincipalLedgerV5` (`0x7F866a9fbe41D54F0d9C35b6892fcf27Ea766DB2`) | `settlementState()`, `rewardTerms(uint256,uint64)`, `openBoundary(uint64)`, `withdrawPrincipal(...)` | `principalSnapshot`, `activeModelWeight`, `activeRewardWeight`, `openedEpoch`, `moduleLockHolder` |
| **Reward Ledger** | `NARARewardLedgerV5` (`0xC72671D9438388638547444524F6542b650C9264`) | `prepareEpoch(uint64)`, `closeEpoch(uint64,uint256)`, `openBoundary(uint64)`, `donateETH()`, `donateToken(RewardAsset,uint256)`, `claim(uint256,uint64,RewardAsset,address)`, `claimCredit(...)`, `creditIndices(...)` | `SCALE` (`1e27`), `RARITY_SHARE_BPS` (`2_000`), `_openedEpoch`, `_nextCheckpoint`, `_protectedNaraSnapshot`, `_rewards[3]`, `_buckets[asset][epoch]`, `_baseIndices[asset][boundary]`, `_rarityIndices[asset][boundary]` |
| **Credit Ledger** | `NARARewardCreditLedgerV5` (child of `NARARewardLedgerV5`) | `checkpoint(...)`, `consumeClaim(...)`, `credit(uint256,uint64,RewardAsset)` | Per-position `(through, unpaidScaled, paid)` credit records |

---

So now your value has a vault where no executive can print, pause, or borrow behind your back—and where every single wei from the Reserve to your wallet is governed by immutable on-chain equations.

Next comes Dispatch #02: **Why paying someone `20 USDC` from a normal Ethereum wallet exposes your entire net worth in four clicks—and how CryptoSLIP V3.2's 1:1 isolated Slips stop it cold without touching a mixer.**
