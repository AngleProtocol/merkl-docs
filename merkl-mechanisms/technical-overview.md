---
description: Technical overview of the Merkl system
---

# Technical Overview

Merkl operates through an offchain engine that analyzes both onchain and offchain data to track user activity and allocate rewards based on campaign rules set by campaign creators. The Merkl engine processes reward data into a merkle tree, compresses it into a merkle root, and pushes it onchain, enabling users to claim rewards transparently and efficiently.

## 🗺️ Platform Overview

Merkl operates through campaigns created by campaign creators. A campaign is a time-bound incentive program where Merkl tracks onchain and offchain activity based on predefined rules. Rewards or points are either posted onchain or updated offchain via Merkl’s endpoints, allowing users to monitor their accumulated rewards.

### How it works

1. **Campaign Creation**: Campaign creators set up campaigns using a smart contract called the **Merkl Distribution Creator**. This includes defining eligibility criteria, reward structures, and distribution methods. Campaign details are then pushed onchain, along with the incentive tokens that need to be distributed
2. **User Participation**: Users engage with the protocol (e.g., providing liquidity, lending, borrowing) in order to earn the rewards/points as specified in the campaign.
3. **Reward Computation:** At fixed intervals, a system called the **Merkl Engine** fetches campaigns from the **Merkl Distribution Creator** contract and processes reward calculations based on available onchain and offchain data and on the rules set by the campaign creator. After this step, users can see that they have earned rewards which are only pending as they cannot claim them yet (awaiting reward update).
4. **Reward Update:** At regular intervals as well (potentially different from the reward computation intervals), the Merkl Engine generates a merkle tree from processed campaigns. This tree is then compressed as a merkle root that is pushed onchain to a contract called the **Merkl Distributor** contract. A reward file is also made available [here](https://app.merkl.xyz/status), in order to ensure transparency and enable anyone to manually audit past reward distributions.
5. **Dispute Period:** After this step, a 1-2 hour dispute period begins, during which newly computed rewards cannot be claimed yet. However, rewards from the previous Merkle root remain claimable. During this period, dispute bots verify the published reward file. If an inconsistency is found, a dispute can be raised to halt incorrect distributions before they become effective.
6. **Reward Availability:** Once the dispute period ends, users can see their rewards on any frontend integrated with the Merkl API. Users can claim rewards through a Merkl-powered frontend. Merkle proofs, required for claiming, are provided by the Merkl API or can be computed from reward files.

### Key Features

* **Single Merkl Root per Chain:** Merkl consolidates rewards from multiple campaigns into one merkle root per chain, enabling efficient batch claiming rewards from multiple different campaigns in a single transaction.
* **Aggregated Campaigns:** While campaigns operate independently, Merkl batches multiple campaign updates into a single onchain transaction
* **Automatic Catch-Up Mechanism:** If any rewards are not included in an update, they are automatically distributed in the next cycle. The Merkl engine ensures no missed rewards, processing only the data from the previous execution point.
* **Unclaimed Rewards Roll Over**: Users can claim their rewards at anytime they want: each merkle tree update takes the previous merkle tree state and simply adds the new rewards, which are then reflected in the published merkle root.

## 🔎 App Overview

The Opportunities page in the Merkl app allows you to browse and compare all available earning opportunities.

For each one, you can view key metrics such as APR, TVL, daily rewards, and reward tokens. By opening an opportunity, additional tabs give access to more details:

- Overview: global information (e.g, dates, chain, APR, blacklist ...)
- Advanced: detailed information regarding distrbiution's progress, last snapshot, creator address and `campaignId`.
- Leaderboard: the list of addresses participating in the opportunity
- Linked Opportunities: related opportunities. More info on this in the [glossary](https://docs.merkl.xyz/glossary#linked-opportunities)

![Opportunities page](image.png)

When using our app, either as a user or a campaign creator, make sure you understand the difference between an opportunity and a campaign:

A campaign on Merkl is a specific incentive program with defined parameters: one single reward token, duration, eligibility rules, etc. It targets a given user-action or behavior (e.g. providing liquidity, holding a token, lending/borrowing).

An opportunity is a grouping of campaigns that target the same user base or protocol behavior (e.g, the same liquidity pool, the same token, or similar user action). Multiple campaigns can run concurrently on the same opportunity, while aggregating APR, TVL & rewards at the opportunity level. 

On the example in the screenshot above, the opportunity targets one single SushiSwap pool, while incentivizing it with 3 different reward tokens, thus containing 3 different campaigns.

## 🤿 Deep-Dive

### Parallel Campaign Processing

Campaigns in Merkl are processed in parallel on each chain. Because campaigns are processed independently, rewards from some campaigns may be available onchain (after a merkle root update) while others are still being computed or have not been fully processed yet.

**How it works**:

* When a campaign is processed, the Merkl Engine resumes from where it last left off, ensuring no reward gaps even if updates occur at different times.
* Because onchain Merkle root updates occur separately, it is possible that:
  * Rewards for one campaign have been finalized and pushed onchain.
  * Rewards for another campaign are still pending because they weren’t processed in time for the last update.
* Merkl App/API Visibility: The Merkl app displays the last time each campaign was processed. However, note that processed rewards are not immediately claimable—they only become available after the next Merkle root update. In this case, they appear as Pending rewards in the dashboard of a user.

### Merkl root update

Merkle root updates — aka reward updates — occur on average every 8 hours (ranging from 4 to 12 hours), depending on the chain.

* [Reward computation](../glossary.md#reward-computation) and [reward update](../glossary.md#reward-update) are independent
* For example, between two reward updates (8 hours apart on average), rewards are computed up to four times
* Unclaimed rewards roll over into the next reward update.

Last reward update for each chain can be viewed on the Merkl app [here](https://app.merkl.xyz/status).

### Dispute Process

If an issue is detected during the dispute period, a dispute can be raised by sending a `disputeToken` to the Merkl Distributor contract.

If the dispute is valid, the merkle root is revoked, and the disputer is refunded. If invalid, the disputer forfeits their funds, and the dispute period restarts.

Dispute conditions (`disputeToken`, `disputeAmount`, `disputePeriod`) can be retrieved from the Distributor contract on the relevant chain.

#### Open-source dispute bot

Angle Labs has developed [an open-source bot](https://github.com/AngleProtocol/merkl-dispute) to verify Merkl reward distributions. Running a dispute bot helps maintain system integrity by detecting and preventing incorrect reward allocations.

{% hint style="info" %}
More active dispute bots mean a stronger system. Need help setting one up? Reach out to the Merkl team—we’re happy to assist!
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

### Merkl Frontends (Multiple Implementations)

* [Merkl Public frontend](https://app.merkl.xyz): Built on the Merkl API, this interface allows users to explore campaigns, check APRs, and claim rewards.
* Custom Frontends: Any team can build their own frontend leveraging the Merkl API, and Merkl can also host for large users branded white-label frontends, providing a seamless experience within their own ecosystem while benefiting from Merkl’s incentive distribution system.
