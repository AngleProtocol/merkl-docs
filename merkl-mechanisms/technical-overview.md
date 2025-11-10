---
description: Technical overview of the Merkl system
---

# Technical Overview

Merkl operates through an offchain engine that analyzes both onchain and offchain data to track user activity and allocate rewards based on campaign rules set by campaign creators. The Merkl engine processes reward data into a merkle tree, compresses it into a merkle root, and pushes it onchain, enabling users to claim rewards transparently and efficiently.

## 🗺️ Platform Overview

Merkl operates through campaigns created by campaign creators. A campaign is a time-bound incentive program where Merkl tracks onchain and offchain activity based on predefined rules. Rewards or points are either posted onchain or updated offchain via Merkl’s endpoints, allowing users to monitor their accumulated rewards.

### How it works

1. **Campaign Creation**: Campaign creators set up campaigns using a smart contract called the **Merkl Distribution Creator**. This includes defining eligibility criteria, reward structures, and distribution methods. Campaign details are pushed onchain, along with the incentive tokens that need to be distributed
2. **User Participation**: Users engage with the protocol (e.g., providing liquidity, lending, borrowing) in order to earn the rewards/points as specified in the campaign.
3. [**Reward Computation:**](#reward-computation) At fixed intervals, a system called the **Merkl Engine** fetches campaigns from the **Merkl Distribution Creator** contract and processes reward calculations based on available onchain and offchain data and on the rules set by the campaign creator. After this step, users can see that they have earned rewards which are only pending as they cannot claim them yet (awaiting reward update).
4. [**Reward Update:**](#reward-updates) At regular intervals as well (potentially different from the reward computation intervals), the Merkl Engine generates a merkle tree from processed campaigns. This tree is then compressed as a merkle root that is pushed onchain to a contract called the **Merkl Distributor** contract. A reward file is also made available [on the Merkl status page](https://app.merkl.xyz/status), in order to ensure transparency and enable anyone to manually audit past reward distributions.
5. [**Dispute Period:**](#dispute-process) After this step, a 1-2 hour dispute period begins, during which newly computed rewards cannot be claimed yet. However, rewards from the previous merkle root remain claimable. During this period, dispute bots verify the published reward file. If an inconsistency is found, a dispute can be raised to halt incorrect distributions before they become effective.
6. **Reward Availability:** Once the dispute period ends, users can see their rewards on any frontend integrated with the Merkl API. Users can claim rewards through a Merkl-powered frontend. Merkle proofs, required for claiming, are provided by the Merkl API or can be computed from reward files.

### Key Features

* **Single Merkl Root per Chain:** Merkl consolidates rewards from multiple campaigns into one merkle root per chain, enabling efficient batch claiming rewards from multiple different campaigns in a single transaction.
* **Aggregated Campaigns:** While campaigns operate independently, Merkl batches multiple campaign updates into a single onchain transaction
* **Automatic Catch-Up Mechanism:** If any rewards are not included in an update, they are automatically distributed in the next cycle. The Merkl engine ensures no missed rewards, processing only the data from the previous execution point.
* **Unclaimed Rewards Roll Over**: Users can claim their rewards at anytime they want: each merkle tree update takes the previous merkle tree state and simply adds the new rewards, which are then reflected in the published merkle root.

## 🤿 Deep-Dive

### Reward Computation

**Reward computation** (or "compute") is the process by which the Merkl Engine calculates reward allocations based on campaign rules.

**Computation frequency**: The Merkl engine processes rewards for each campaign approximately every 2 hours on average, analyzing all relevant onchain events affecting the incentivized asset.

**After computation**: Once a campaign completes its computation cycle, rewards are not immediately claimable onchain but appear as **pending rewards** in user dashboards.

**Parallel processing**: Campaigns on a chain are processed independently and in parallel. This means rewards from some campaigns may be available onchain while others are still computing or awaiting updates.

**Delayed computations**: Occasionally, campaign computations may be delayed due to processing constraints. Track delayed campaigns on the [Merkl status page](https://app.merkl.xyz/status).

**Continuous processing**: When a campaign is processed, the Merkl Engine resumes from its last checkpoint, ensuring complete reward coverage even when updates occur at different intervals.

{% hint style="info" %}
**Reward delays & retroactive distribution**

Occasional delays of a few hours may occur in reward distribution. If an invariant is triggered or a third-party data feed experiences a temporary issue, calculations are paused until all safety checks pass at the next scheduled computation.

When this happens, the Merkl system is designed so that **all rewards are distributed retroactively**, with no rewards undistributed.
{% endhint %}

### Reward Updates

**Reward updates** are the process of posting computed rewards onchain, making them claimable by users.

**Update frequency**: Reward updates occur approximately every 8 hours (ranging from 4 to 12 hours), depending on the chain.

**Delayed updates**: Like computations, updates may occasionally be delayed. Monitor update status on the [Merkl status page](https://app.merkl.xyz/status).

**Transparency**: Each reward update includes a publicly available reward file on the status page for transparency and auditability.

**Independent cycles**: Reward computation and reward updates operate on independent schedules. Updates push the results of completed computations onchain, but the two processes follow separate lifecycles.

**Example**: Between two reward updates (8 hours apart on average), a campaign's rewards may be recomputed up to four times. Each computation refines the pending rewards, but they only become claimable when the next update occurs.

**Rollover mechanism**: Unclaimed rewards automatically roll over into subsequent reward updates, allowing users to claim at their convenience.

**Tracking updates**: View the last reward update for each chain on the [Merkl status page](https://app.merkl.xyz/status).

### Dispute Period & Process

The **dispute period** is a security window of 1-2 hours following each reward update during which anyone can contest reward allocations made by the Merkl engine. This safeguard ensures the integrity of the reward infrastructure and allows campaign creators to oversee the reward computations before they become final and claimable.

**Raising a dispute**: If an issue is detected during the dispute period or if you want to contest reward allocations for a campaign, raise a dispute by sending a `disputeToken` to the Merkl Distributor contract.

**Dispute outcomes**:

* **Valid dispute**: The merkle root is revoked and the disputer is refunded
* **Invalid dispute**: The disputer forfeits their funds and the dispute period restarts

**Dispute parameters**: Retrieve dispute conditions (`disputeToken`, `disputeAmount`, `disputePeriod`) from the Distributor contract on the relevant chain.

#### Dispute Bots

**Security infrastructure**: The Merkl team operates several independent dispute bots on separate infrastructure to ensure redundancy and prevent malicious actors from compromising the reward update process.

**Open-source reference**: While the production bot code is closed source for security reasons, you can reference [this open-source repository](https://github.com/AngleProtocol/merkl-dispute) to understand how to build a dispute bot.

{% hint style="info" %}
**Strengthen the network**: More active dispute bots make the system more secure. Need help setting one up? Reach out to the Merkl team—we're happy to assist!
{% endhint %}

## 📌 Other Key Merkl components

Beyond the Merkl engine and dispute bots described above, Merkl consists of several core components that interact to facilitate reward distribution

### Merkl Smart Contracts

Deployed on each blockchain integrated by Merkl, Merkl's smart contracts store campaign data, hold incentive tokens, and process reward claims.

These contract are [publicly available](https://github.com/AngleProtocol/merkl-contracts) and have been audited by Code4rena ([Audit Report](https://code4rena.com/reports/2023-06-angle)).

Key contracts:

* `DistributionCreator`: Stores campaign details and configurations.
* `Distributor`: Holds tokens and processes reward claims based on merkle proofs.
* `AccessControlManager`: A multisig-controlled contract that manages disputes, fees, and access control but cannot alter distributions.

Smart contract addresses, categorized by chain are listed [here](https://app.merkl.xyz/status).

### Merkl API

[Merkl API](../integrate-merkl/app.md) provides real-time access to Merkl data, including rewards, APRs, merkle proofs, and analytics. This API enables any frontend to integrate Merkl seamlessly.

### Merkl App

[The Merkl App](https://app.merkl.xyz/), built on the Merkl API, enables users to explore reward opportunities, track APRs, and seamlessly claim their rewards.

<figure><img src="../.gitbook/assets/Capture d’écran 2025-09-17 à 10.51.55 1.png" alt="Homepage of the Merkl app"><figcaption><p>Home page of the Merkl App</p></figcaption></figure>

The interface is organized around several types of pages:

* **Home page (all opportunities)** – displays all available opportunities with filtering options
* **Opportunity page** – dedicated view for each opportunity
* **Protocol / Chain / Liquidity program page** – groups all opportunities related to a specific protocol, chain, or program
* **Dashboard** – where users can claim their rewards

{% hint style="info" %}
Among these, the Opportunity page is central, as this is where you’ll find the campaigns you’ve created!
{% endhint %}

#### Opportunity vs. Campaign

Before using Merkl, it’s important to understand the difference between an opportunity and a campaign.

* **Campaign**: a reward distribution set by a campaign creator, with specific parameters such as [distribution type](distributions.md), reward amount, [eligibility rules](customization-options.md#eligibility), [boosts](customization-options.md#boosts), and duration. A campaign always targets a specific onchain behavior (e.g. providing liquidity in a pool, holding a token, lending/borrowing), also referred to as an opportunity.
* **Opportunity**: an asset (e.g. pool, vault,…) and its associated action (e.g. depositing liquidity, borrowing assets) that are incentivized.\
  Example: _Provide liquidity to the SushiswapV3 liquidity pool._

<figure><img src="../.gitbook/assets/Group 23.png" alt=""><figcaption><p>An opportunity page with several incentive campaigns running on it</p></figcaption></figure>

{% hint style="success" %}
**Multiple campaigns can run simultaneously on the same opportunity**. This means that for a single onchain action, a user can earn rewards from several campaigns.
{% endhint %}

#### Focus - Opportunity page

On an Opportunity page, you’ll find key metrics that aggregate all active campaigns for that opportunity:

* APR
* TVL (Total Value Locked)
* Daily rewards (the amount of rewards shared each day among participants)

<figure><img src="../.gitbook/assets/Group 24.png" alt=""><figcaption><p>Campaign-specific info on the opportunity page</p></figcaption></figure>

Detailed info for each campaign running on the opportunity is available across several tabs:

* **Overview:** global details such as dates, APR, and eligibility rules,…
* **Advanced**: distribution progress, last snapshot, creator address, and campaign ID,…
* **Leaderboard**: list of addresses participating in the opportunity
* [**Linked opportunities**](glossary.md#linked-opportunities) _(optional):_ displays opportunities connected through shared liquidity and rewards
