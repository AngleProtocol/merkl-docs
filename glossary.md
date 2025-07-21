---
description: Here you'll find the key terms used across Merkl's products and experience.
---

# Glossary

## APR

Annual Percentage Rate (APR) represents the yearly return from participating in an opportunity, expressed as a percentage. The APR can be fixed and stay constant throughout the campaign, or it can be variable and fluctuate based on factors such as the number of participants.

## Blacklist

A list of addresses excluded from receiving rewards via Merkl for a given campaign.

## Boost

An optional feature that allows campaign creators to increase rewards for users who hold a specific token or NFT in their wallet. Different boost formulas are available.

## Bridged liquidity

Liquidity that has been bridged from one chain to another before being deposited into a protocol. Bridged liquidity can be used as an optional eligibility criteria for receiving rewards, to ensure that the provided liquidity actually increases the destination chain’s TVL — rather than simply being moved from one protocol to another on the same chain.

## Campaign

A distribution of tokens or points over a defined period, launched by an incentivizer to reward participants of a specific opportunity. For example, an incentive campaign distributing 50k tokens over 3 days to users who provide liquidity in a particular pool. Note that multiple campaigns can exist for the same opportunity—for instance, Protocol A and Protocol B may both launch campaigns to reward participants of the opportunity “Deposit tokens in the AAA-BBB liquidity pool.” In such cases, users can earn rewards from both campaigns with just one deposit.

## Capped reward rate campaigns

A capped reward rate campaign is similar to a [variable reward rate campaign](glossary.md#variable-reward-rate-campaign) — where APR adjusts based on TVL — but the APR is capped and cannot be exceeded. This prevents users from capturing excessive rewards when TVL is low, especially at the beginning of the campaign.

## Cross-chain rewards

Distributing rewards to users on a different blockchain than where the incentivized asset is located. For example, incentivizing a WETH-USDC pool on Ethereum by distributing WBTC rewards on Base.

## Daily rewards

The total amount of tokens or points distributed each day, shared among all users eligible for the campaign.

## Dashboard

The page in the Merkl App where users can claim their earned rewards in a single transaction and without any gas fees.

## Deposit URL

A link shown on an opportunity’s page that directs users to the protocol where the incentivized action must be performed to become eligible for rewards. For example, the page of a lending market where liquidity must be deposited to earn rewards via Merkl. Note that Merkl is non-custodial, meaning liquidity is never held by Merkl but deposited directly into the protocols.

## Dispute

A objection submitted by anyone to signal an inconsistency in the reward distribution. A dispute can be raised by sending a disputeToken to the Merkl Distributor contract.

## Eligibility

An optional feature that allows a campaign creator to restrict rewards so that only users who hold a specific token in a minimum amount for a certain number of days receive rewards.

## Encompassing campaign

A campaign type that enables campaign creators to distribute rewards without requiring Merkl’s engine to compute the results. In this model, partners provide API endpoints containing opportunity data and rewards, which are distributed based on the provided output.

## Fixed reward rate campaign

A predefined amount of rewards is distributed per unit of liquidity provided by users at a constant rate. As more users join, the campaign’s budget is consumed more quickly, but individual earnings remain stable. Campaigns end when funds are depleted or the scheduled duration expires.\
There are two types of fixed reward rate campaigns depending on whether the distributed amount is pegged to a fixed token amount ([fixed token rewards](glossary.md#fixed-token-rewards)) or to a fixed dollar value ([fixed dollar rewards](glossary.md#fixed-dollar-rewards)).

## Fixed dollar rewards

Fixed dollar rewards are a type of [fixed reward rate campaign](glossary.md#fixed-reward-rate-campaigns). With fixed dollar rewards, users earn a fixed amount of rewards in USD terms, even though the reward is distributed in tokens. The APR in dollar value remains stable for users, but the number of tokens they receive may vary depending on the token’s market price.

## Fixed token rewards

Fixed token rewards are a type of [fixed reward rate campaign](glossary.md#fixed-reward-rate-campaigns). With fixed token rewards, users earn a predetermined number of tokens, regardless of fluctuations in the token’s market price. As a result, users’ APR is directly affected by changes in the token's value: if the token price increases, the effective return grows, and if it decreases, the return diminishes.

## Forwarder

A contract (such as a staking, an ALM, or a vault) that holds the incentivized asset on behalf of users. Merkl detects the underlying users and distributes rewards to them, even if the asset isn’t held directly in their wallet. For example, a campaign distributing rewards to USDA holders. Users who staked USDA and receive stUSD in exchange would normally be ineligible because they don't hold the USDA directly in their wallet. With forwarders, Merkl recognizes stUSD holders as USDA holders and distributes rewards accordingly. Reward forwarding is enabled by default in Merkl.

## Hook

Optional features that can be added to a campaign to make it unique, such as boosts, blacklists, forwarders, bridged liquidity, and more. A non exhaustive list of hooks is available here.

## Merkl Studio

The command center for incentive campaign creators. It allows anyone to launch incentive campaigns via Merkl, independently and in just a few minutes, and provides powerful tools to manage them effectively.

## Opportunity

An asset (liquidity pool, lending market, ...) and its associated action that is incentivized via Merkl. For example, providing liquidity to a specific liquidity pool, holding a particular token, or lending a token on a given protocol. Not to be confused with a campaign.

## Pre-TGE

In pre-TGE campaigns, participants receive reward tokens representing a future allocation of tokens that have not yet been launched. These pre-TGE tokens are non-transferable, non-tradable, and their displayed value is an estimate subject to change at the time of the Token Generation Event (TGE).

## Referral

A system that lets a Merkl user share a link to an opportunity with another person. If that person deposits liquidity into the opportunity, both the new participant and the original user receive a boost in rewards. Referral helps campaigns grow virally.

## Reward Computation

The process by which the Merkl Engine calculates reward allocations based on onchain and offchain data, as well as the rules defined by each campaign (e.g., blacklists). Reward computations run **on average every 2 hours**. After this step, users can view the rewards they’ve earned, which are marked as pending until the [rewards are updated](glossary.md#reward-update) — at which point they become claimable.

## Reward Update

The process by which the Merkl Engine compresses computed campaign data into a Merkle root and pushes it onchain to the Merkl Distributor contract, enabling users to claim their rewards. Updates run **on average every 8 hours (between 4 to 12 hours)**, and each includes a [reward file](https://app.merkl.xyz/status) for transparency and auditability.

[Reward computation](glossary.md#reward-computation) and reward updates are independent processes. For example, rewards are computed every 2 hours, but updates may happen just once a day — so users see pending rewards accruing every 2 hours, but can only claim them once daily.

## TVL

Total Value Locked (TVL) measures the total amount of assets deposited in an opportunity. It reflects the combined value of all tokens provided by liquidity providers and serves as a key indicator of the market/pool’s size and liquidity depth. TVL directly impacts an opportunity’s APR and daily rewards.

## Variable reward rate campaign

A set amount of daily rewards is shared among all participants proportionally to their contribution. Each user earns according to his campaign’s share. The APR fluctuates depending on participation and TVL. This is the most common type of campaign. Variable reward rate campaigns contrast with [fixed reward rate campaigns](glossary.md#fixed-reward-rate-campaign), where the reward is predefined and where each user gets the same APR.

## Whitelist

A list of addresses eligible to receive rewards for a given campaign. Any address not included in this list will be excluded from the reward distribution.
