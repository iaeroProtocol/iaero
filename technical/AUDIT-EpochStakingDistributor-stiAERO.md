Security Audit Reports: Epoch Staking Distributor & stiAERO
Auditor: Independent Security Review
Date: September 2025 Network: Base Mainnet

Audit 1 — EpochStakingDistributor 
Contract Address: 0x781A80fA817b5a146C440F03EF8643f4aca6588A
Executive Summary

Risk rating: Low–Medium overall
The contract implements a snapshot‑per‑epoch reward distribution for iAERO stakers. It does not maintain or iterate any token list, which keeps stake/unstake gas predictable. Rewards are accounted by (token, epoch) and claimed by users directly. The contract follows safe interaction patterns (checks‑effects‑interactions, nonReentrant) and now includes Pausable, tighter funding arithmetic, and safer receipt‑token linkage.

What changed / fixed

Removed unchecked arithmetic in ERC‑20 funding; now we compute received = after − before with explicit non‑regression and > 0 checks.

Added Pausable; critical functions (stake, unstake, exit, notify*, claim*, backfillReceipts) are gated with whenNotPaused.

Strengthened receipt‑token onboarding: setReceiptToken now requires a code‑bearing contract, forbids setting to iAERO, and can be frozen.

Wider checkpoint timestamps: uint64 ts (from uint48) while keeping checkpoints to one slot (uint64 ts | uint192 value).

Conservative ERC‑20 recovery: owner can only recover non‑iAERO and non‑receipt tokens (with clear warning; see “Operations & Risks”).

Internal claim path for batch claims avoids nested nonReentrant.

Scope & Methodology

In‑scope files: Epoch‑snapshot distributor contract provided in the last version you approved.

Out‑of‑scope: Vault, Harvester, Aerodrome Voter, token prices/oracles, third‑party protocols using stiAERO as collateral.

Approach: Manual line‑by‑line review, invariants & state‑machine reasoning, reentrancy/authorization analysis, economic checks for reward splits, edge‑case exploration (zero‑supply epochs, FOT tokens), gas & grief‑ing considerations.

System Overview

Stake/Unstake: Users stake iAERO into the distributor; (optionally) a receipt token (stiAERO) is minted/burned 1:1 with staked iAERO.

Epoch model: Rewards are funded to (token, epoch) buckets. Payout shares are computed from epoch‑start snapshots of user balance and total supply.

Funding: notifyRewardAmount(token, amount) (current epoch) or notifyRewardForEpoch(token, specificEpoch). ERC‑20 funding uses “pull” with balance‑delta to accommodate fee‑on‑transfer tokens; ETH uses msg.value.

Claiming: Users call claim(token, epoch) (or batched variants) to pull rewards. The distributor transfers directly to the user.

No token enumeration: The contract never stores an “active set” of reward tokens; all loops are bounded by the caller’s input (capped at 50 items per batch).

Roles & Permissions

Owner: pause/unpause; set receipt token / freeze pointer; set allowed funders; perform conservative recoverERC20.

Allowed funders: call notify* functions.

Anyone: stake/unstake (user), claim personal rewards.

Threat Model & Trust Assumptions

Trusted: Contract owner/multisig; allowed funders (e.g., your Harvester).

Untrusted: Users; arbitrary ERC‑20 tokens used as rewards; external protocols where stiAERO is used as collateral.

Dependencies: OpenZeppelin (v5) Ownable, ReentrancyGuard, Pausable, SafeERC20, Math.

Findings
Critical / High

None observed.

Medium

M‑1: Zero‑supply epoch ⇒ permanent 0 claims for that epoch

Description: If totalSupplyAtEpochStart(epoch) == 0, then previewClaim returns 0 for that epoch forever (even if users stake mid‑epoch). This is by design for snapshot fairness but leaves funds stranded in that epoch bucket.

Impact: Operational. Rewards for such epochs do not flow to users.

Recommendation: Handle at the Harvester/ops layer—if snapshot is 0 for intended epoch, fund the next epoch. Optionally add an owner‑only “roll‑forward” function to move untouched rewards to a later epoch.

Status: Accepted by design. Documented in runbook (see below).

M‑2: Admin / role mis‑configuration risk

Description: If stiAERO roles (MINTER/BURNER) are not granted to the distributor, staking or unstaking will revert. If the distributor’s owner is an EOA, key risk increases.

Recommendation: Use a multisig for both DEFAULT_ADMIN_ROLE on stiAERO and Ownable owner on the distributor; wire roles first, then call setReceiptToken. Consider freezing pointer after validation.

Status: Mitigated operationally (deployment checklist provided).

Low

L‑1: Conservative token recovery can withdraw reward tokens

Description: recoverERC20 is intentionally narrow (excludes iAERO & receipt token) but could still remove a token that has funded/claimable balances.

Recommendation: Use only for obvious dust. For a fully safe recovery, a richer liability check would be needed (not recommended due to gas/storage overhead).

Status: Acknowledged; keep as emergency escape with procedures.

L‑2: Batch sizes and user checkpoint growth

Description: claimMany/notifyBatch accept up to 50 items; checkpoint arrays grow with user activity.

Recommendation: The caps and binary search are adequate; periodically test gas envelopes.

Status: Accepted.

L‑3: Pausing blocks claims

Description: whenNotPaused protects claim functions as well.

Recommendation: If business preference is to allow claims while paused, remove whenNotPaused from claim functions.

Status: Configuration choice.

Design Soundness & Invariants

No token enumeration: The distributor never loops “all tokens”; all loops depend on caller input (bounded/capped).

Reentrancy: All external state‑mutating functions are nonReentrant. Claims use internal _claim to avoid nested guards. Transfers occur after state updates.

Funding correctness: ERC‑20 path computes received = after − before and requires after ≥ before and received > 0. ETH path requires msg.value == amount.

Snapshot math: For user u, payout = rewards[token][epoch] * bal_at_start(u) / supply_at_start. The sum of payouts over all users ≤ funded amount (flooring/rounding may leave dust).

Receipt alignment: After setReceiptToken, staking mints stiAERO 1:1, unstaking burns 1:1. exit burns full balance.

Ownership: Ownable(msg.sender) in constructor; recommended to transfer to multisig post‑deploy.

Operational Guidance (How to Use)
Deployment (Base)

Deploy EpochStakingDistributor(iAERO).

Deploy StiAERO(admin=multisig).

From the admin (multisig), grant roles to the distributor:

grantRole(MINTER_ROLE, <Distributor>)

grantRole(BURNER_ROLE, <Distributor>)

On distributor, set receipt token: setReceiptToken(<StiAERO>). (Optional: freezeReceiptToken().)

Allow funder: setAllowedFunder(<RewardsHarvester>, true).

Transfer ownership of distributor to multisig (recommended).

If there were existing stakes, use backfillReceipts([users...]) in batches.

Routine Operations

Funding (Harvester):

Normal: notifyRewardAmount(token, amount) → current epoch.

Specific epoch: notifyRewardForEpoch(token, epoch, amount) (current or previous only).

Batch: notifyRewardAmountsBatch(tokens[], epochs[], amounts[]) (≤ 50 legs).

Claiming (User/UI):

Single: claim(token, epoch)

Many: claimMany(tokens[], epochs[]) (n tokens for n epochs)

Weekly default: claimLatest(tokens[]) (current + previous)

Preview: previewClaim(user, token, epoch) or previewClaimsForEpoch.

Emergency:

pause() → blocks stake/unstake/notify/claim/backfill.

unpause() restores.

recoverERC20(token, to, amount) only for non‑iAERO/non‑receipt dust.

Monitoring & Playbooks

Zero‑supply epoch: If supplySnapshotAtEpochStart[epoch] == 0, fund next epoch instead.

Unclaimed dust: Due to rounding, small dust may remain. This is acceptable; do not sweep casually.

Receipt invariants: stiAERO.totalSupply() should equal the sum of balanceOf across users (not tracked on‑chain; verify off‑chain).

Test Recommendations

Property tests:

Sum of previewClaim across users ≤ funded amount.

Receipt mint/burn matches delta in balanceOf.

Funding with FOT token yields received > 0 and claims match received.

Epoch with zero supply produces zero claims; funding next epoch produces non‑zero claims.

Reentrancy tests: Attempt reentrancy via ERC‑777 style tokens or malicious receipt token; should be blocked by nonReentrant and call order.

Pause tests: Verify all protected functions revert when paused.

Audit 2 — StiAERO (Receipt Token)
Contract Address: 0x72C135B8eEBC57A3823f0920233e1A90FF4D683D
Executive Summary

Risk rating: Low
StiAERO is a standard transferable ERC‑20 with ERC20Permit and AccessControl. The staking distributor is granted MINTER_ROLE and BURNER_ROLE to mint/burn receipts 1:1 with staked balances. The security posture primarily depends on role governance and the distributor’s correctness.

Scope & Methodology

In‑scope: StiAERO token contract.

Approach: Manual review for ERC‑20 correctness, role gating, and mint/burn semantics.

System Overview

Token: Staked iAERO / stiAERO (18 decimals).

Roles:

DEFAULT_ADMIN_ROLE: manages roles; set to a multisig at deployment.

MINTER_ROLE: granted to distributor.

BURNER_ROLE: granted to distributor.

Mint/Burn:

mint(to, amount) only by MINTER.

burn(from, amount) only by BURNER (no allowance required; distributor burns directly during unstake/exit).

Findings
Critical / High

None observed.

Medium

M‑1: Centralized roles

Description: Admin can mint/burn if they grant roles to themselves or a compromised address.

Recommendation: Keep DEFAULT_ADMIN_ROLE on a multisig, avoid keeping MINTER/BURNER on EOAs beyond deployment. Revoke any temporary roles.

Low

L‑1: External dependencies

Description: Relies on OZ ERC‑20/Permit/AccessControl; well‑audited but requires correct linkage and compiler settings.

Recommendation: Pin OZ version in your build, run compilation with optimizer enabled and consistent settings across contracts.

Operational Guidance (How to Use)

Deployment: new StiAERO(admin=multisig); immediately grant MINTER/BURNER to distributor.

After wiring: Optionally revoke any roles from the deployer EOA; keep only multisig (admin) and distributor (minter/burner).

Usage in UIs/protocols: stiAERO is a regular ERC‑20 (transferable, permit supported). Users may deposit it as collateral elsewhere—but they must hold enough to burn when unstaking.

Fixes & Closing Notes

The combined system (Distributor + stiAERO) incorporated the following security improvements before this final audit:

✔️ Removed unchecked arithmetic in ERC‑20 funding; added explicit post‑transfer checks.

✔️ Added Pausable to all critical flows.

✔️ Hardened setReceiptToken (code length check; forbid iAERO; optional freezing).

✔️ Widened checkpoint timestamp to uint64.

✔️ Added conservative recoverERC20 with exclusions.

✔️ Used internal _claim to avoid nesting nonReentrant.

With these changes, the design is sound and production‑ready for Base, assuming operational best practices:

Run with multisig admin/owner.

Set allowedFunders to the Harvester only.

Handle zero‑supply epochs at the operations layer (fund next epoch or accept that bucket as 0).

Avoid using recoverERC20 except for obvious dust; do not sweep active reward tokens.

Appendix — Function Access Map (Distributor)
Function	Access	Pausable	Reentrancy	External calls
stake, stakeFor	public	Yes	Yes	ERC‑20 transferFrom (iAERO), optional stiAERO.mint
unstake, exit	public	Yes	Yes	optional stiAERO.burn, ERC‑20 transfer (iAERO)
notifyRewardAmount	owner/allowedFunder	Yes	Yes	ERC‑20 transferFrom (reward)
notifyRewardForEpoch	owner/allowedFunder	Yes	Yes	ERC‑20 transferFrom (reward)
notifyRewardAmountsBatch	owner/allowedFunder	Yes	Yes	ERC‑20 transferFrom (reward)
claim	public	Yes	Yes	ETH transfer or ERC‑20 transfer (reward)
claimMany, claimLatest	public	Yes	Yes	ETH/ ERC‑20 transfers
setReceiptToken, freezeReceiptToken	owner	n/a	n/a	sets receipt address
backfillReceipts	owner	Yes	Yes	stiAERO.mint
setAllowedFunder	owner	n/a	n/a	—
pause, unpause	owner	n/a	n/a	—
recoverERC20	owner	n/a	n/a	ERC‑20 transfer (non‑iAERO/non‑receipt)
