# System Wiring Post Deployment

**One‑time wiring checklist**

1. **Vault ⟷ Harvester**

   * `Vault.setRewardsCollector(<Harvester>)` → lets Harvester call `sweepERC20/ETH` (and auto‑authorizes it).
   * `Vault.setAuthorizedTarget(<AerodromeVoter>, true)` → allows veNFT actions via `executeNFTAction` to the Voter.
   * *(Optional if not already authorized via `setRewardsCollector`):* `Vault.setAuthorized(<Harvester>, true)`.

2. **Vault ⟷ VotingManager**

   * `Vault.setVotingManager(<VotingManagerOptimised>)` → authorizes manager.
   * `Vault.setAuthorizedTarget(<AerodromeVoter>, true)` → **required** for voting resets/votes.
   * Ensure Vault **owns** a valid veAERO NFT and it’s marked as `primaryNFT`.

3. **Harvester ⟷ Distributor**

   * `Harvester.setStakingDistributor(<EpochStakingDistributor>)`.
   * *(Optional)* `Harvester.setTreasuryDistributor(<TreasuryDistributor>)` (if you use protocol split helper).

4. **VotingManager Oracles & Tokens**

   * Configure Chainlink (or adapter) feeds:

     * `setOracle(address(0), <ETH/USD feed>, <maxStale>, true)` (for ETH bribes).
     * `setOracle(<token>, <feed>, <maxStale>, true)` for each bribe token.
   * Allow bribe tokens: `setAllowedBribeToken(<token>, true)` (or use `batchConfigureOracles(..., alsoSetAllowed=true)`).

5. **Keepers / Operators**

   * `Harvester.setKeeper(<bot>, true)`
   * `VotingManager.setKeeper(<bot>, true)`

> **Note:** Configure these on **Base** with correct addresses (Aero Voter, feeds). Vault’s immutables (AERO, iAERO, veAERO, LIQ, treasury) are set at deploy.

---
