# 🔍 Architecture and Technical Overview

Merkl is a platform designed to streamline and optimize reward distribution in the DeFi space. It operates on an offchain engine that analyzes both onchain and offchain data to measure user behavior and distribute rewards among eligible users based on the rules set by the campaign creator (the incentive provider). The Merkl engine aggregates reward distribution data into a merkle tree, compresses it into a merkle root, and pushes it onchain, allowing users to claim their rewards.

## Platform Overview

Merkl works around campaigns created by incentive providers. A campaign on Merkl is a time-bounded incentive program during which the Merkl engine will track on and offchain activity following some specific rules set by an incentive provider and either post onchain rewards or update its endpoints to keep track of how many tokens/points were accumulated by a given address.

The global flow is the following:

1. **Campaign Creation**: An incentive provider creates a campaign using a Merkl smart contract called the Merkl Distribution Creator contract. This includes specifying the rules and conditions under which rewards will be distributed (or not in the case of points programs). The campaign details are then pushed onchain, ready to start tracking eligible user activity.
2. **User Participation**: Users interact with the protocol in various ways (e.g., providing liquidity, lending, borrowing, etc.) to become eligible for rewards/points as defined by the campaign rules set by the incentive provider.
3. **Reward Computation**: The Merkl Engine, an offchain component hosted by Angle Labs, regularly collects onchain and offchain data to measure user behavior. At fixed intervals, the Merkl Engine processes the collected data to update reward distributions. This ensures that users can claim new rewards (or accrue points) multiple times a day without needing to stake tokens or perform additional onchain actions. Based on the data, the engine calculates the reward distribution, aggregates it into a merkle tree, and compresses it into a merkle root. The computed merkle root is then pushed onchain. Following the merkle root update, a dispute period allows anyone to verify the reward distribution and contest it if necessary. Note that some campaigns are just used for tracking purposes, and rewards are not posted onchain: this is the case of points programs used by some protocols.
4. **Reward Distribution**: Once the dispute period is over and the merkle root is validated, users can claim their rewards. The rewards from potentially multiple campaigns are aggregated, enabling users to claim all their rewards in a single transaction. Users can track their active positions and rewards through the [Merkl user dashboard](https://app.merkl.xyz/user), providing a transparent view of their earnings and investments.

![Merkl Engine applied to concentrated liquidity pools](../.gitbook/assets/docs-merkl-script.png)

## Key Features of the Merkl System

- **Single Merkl Root per Chain:** The Merkl system relies on **a single merkle root per chain**. This allows Merkl users to claim all their token rewards from various campaigns in just one transaction on Merkl.
- **Aggregated Campaigns:** Although campaigns on Merkl are treated independently, the Merkl engine usually aggregates the outcomes of multiple campaigns at once when updating a merkle root onchain. This ensures that rewards for multiple campaigns are included in a single update whenever possible.
- **Multiple Incentive Providers:** The engine is compatible with multiple incentive providers incentivizing the same type of campaigns (e.g., the same pool on Uniswap V3) with potentially different parameters. If you are eligible for a campaign on Merkl, you will claim rewards from all incentive providers who have incentivized your behavior when claiming your rewards. This means that many teams can incentivize a specific behavior at the same time with different tokens.

However, it is possible for a given merkle root to include rewards for some campaigns but not others that are live on the same chain. Regardless, **the Merkl engine ensures that all users involved in a campaign during its live period receive their rewards**. Any rewards that were not distributed as they should have been will be distributed in an upcoming engine run. As the Merkl engine operates regularly for every campaign on a chain - examining only the data specific to the period between its current and previous executions - if distributions were missed, the engine will collect the necessary data from where it last left off, ensuring accurate and complete reward allocation.

## Distribution Epochs

The engine operates in time periods, known as epochs, which range from 3 to 12 hours, depending on the chain. At the start of each epoch, the engine processes data from where it last left off, ensuring continuous and up-to-date reward calculations.

This epoch length also determines the interval between reward distributions in a campaign. For example, with a 8-hour epoch, users eligible for campaigns on Merkl can claim new rewards up to three times a day.

It is important to note that **users do not need to claim their Merkl rewards at every epoch**. Each merkle tree update takes the previous merkle tree state and simply adds the new rewards, which are then reflected in the published merkle root.

## Dispute Periods

The engine responsible for computing rewards and updating the reward's merkle root onchain is operated by Angle Labs. Merkle roots pushed onchain are based on offchain computations from both onchain and offchain data. Anyone can access the data required to run the script and verify the results.

To allow anyone to permissionlessly verify and ensure that the system functions properly, while minimizing the risk of hacking or failures, each new merkle root update is followed by a dispute period. A new Merkle root that aggregates reward distribution data for a chain will not take effect until after this dispute period.

Anyone can contest the result of a distribution during the dispute period. To initiate a dispute, `disputeToken` must be sent to the reward distribution contract. This requires sending either 100 EURA (stablecoin developed by Angle Labs pegged to the EURO) or, if EURA is not supported on the campaign's chain, the equivalent amount in the gas token of the campaign's chain (i.e., the token used for paying gas fees on that chain). If a dispute is triggered, the merkle root of the distribution contract is frozen to its last valid version. Disputes are then evaluated for validity: if a dispute is considered valid, the disputer is refunded, and the disputed merkle root is revoked; if invalid, the disputer forfeits their funds, and the dispute period restarts, leaving the disputed tree still unvalidated.

Information about `disputeToken`, `disputeAmount`, `disputePeriod` can be obtained by directly querying the contract handling reward distribution (_Distributor_) on the relevant chain. You can find all Merkl's smart contracts on this [page](https://app.merkl.xyz/status).

Angle labs has developed [an open-source bot](https://github.com/AngleProtocol/merkl-dispute) for everyone to check the rewards sent on Merkl and potentially dispute them. This is a crucial piece of infrastructure as it prevents invalid rewards from being claimed by users.

{% hint style="info" %}
The more active dispute bots, the better. If you encounter any issues deploying this bot, please feel free to reach out to the Merkl team—we're more than happy to assist!
{% endhint %}

## Merkl components

Let's break down the different components involved in the Merkl system and how they work with each other:

- [Smart contracts](#merkl-smart-contracts) deployed on each chain supported by Merkl.
- Offchain components hosted by Angle Labs, including:
  - [The Merkl Engine](#merkl-engine) (proprietary): Computes rewards and pushes new merkle roots onchain.
  - [Merkl Public API](../integrate-merkl/README.md): Provides easy access to Merkl-related data such as current rewards, APRs, merkle proofs for claims, and analytics. Merkl's public API allows the seamless integration of Merkl data into any frontend.
  - [Merkl Public frontend](https://app.merkl.xyz): Built on the public API, Merkl's public frontend allows users to easily view all Merkl campaigns, associated APRs, and of course claim their rewards. Any team can set up a dedicated Merkl frontend, as it fetches all its data from the fully public Merkl API.
  - Merkl [rewards bucket](#merkl-rewards-bucket): Stores all historical reward files, enabling anyone to rebuild the merkle root from these records.
- The Merkl Dispute bot: Presented above, this bot can be hosted by anyone to ensure the integrity of the reward distribution process.

![Merkl Components](../.gitbook/assets/merkl-components.png)

The components interact in the following way:

1. **Campaign Creation:** An incentive provider creates a campaign on the **Merkl Distribution Creator** contract.
2. **Token Forwarding to the Merkl Distributor contract:** When the campaign is created, the incentive tokens (what users will receive) are forwarded to the **Merkl Distributor** contract.
3. **Merkl Engine Campaigns Processing:** At fixed intervals, the **Merkl Engine** fetches the campaigns from the **Merkl Distribution Creator** contract, processes the campaigns to compute the rewards, computes a new merkle root from the result of the previous process, and pushes the new merkle root to the **Merkl Distributor** contract. It also pushes a reward file to the **Merkl Rewards bucket**.
4. **Dispute Period:** When the root is pushed to the contract, the dispute period starts. This period lasts between 1 and 2 hours, during which the newly pushed rewards cannot be claimed.
5. **Dispute Verification:** The **Merkl Dispute bots** fetch the reward file, re-compute the merkle root from it, and verify the validity of the reward allocation. If the merkle root cannot be re-computed or if the rewards are invalid, the bots can dispute the merkle root. No rewards can be claimed until the dispute is resolved.
6. **Reward Availability:** Once the dispute period is over and if no disputes are valid, users can view their rewards on the **Merkl Frontend** or any other frontend integrated with the **Merkl API**.
7. **Claiming Rewards:** Users claim their rewards through the Merkl Frontend (or any other frontend integrated with the Merkl API). The merkle proofs needed to claim the rewards are provided by the Merkl API, which can be used by any other frontend. These proofs can also be computed using the reward files from the **Merkl Rewards bucket.**

## Merkl smart contracts

The Merkl smart contracts, developed by Angle Labs, form the backbone of the Merkl system. The source code is available [here](https://github.com/AngleProtocol/merkl-contracts).

Merkl smart contracts have been audited by Code4rena, and you can find the audit report [here](https://code4rena.com/reports/2023-06-angle).

The system relies on two main contracts:

- The `DistributionCreator` contract: This contract is used by incentive providers to create campaigns. This contract holds no funds and is only used to store the campaigns and all their rules. When calculating rewards, the Merkl Engine fetches the campaigns and their configurations directly from this contract.
- The `Distributor` contract: This contract is used by users to claim their rewards, and holds all the tokens that will be distributed to the users. Tokens can only be claimed by providing merkle proofs that match the current merkle root. A new merkle root is pushed every time the engine computes new rewards.

Both contracts are managed through an `AccessControlManager` contract managed by a multisig. This contract has the power to settle disputes, change dispute parameters, modify fees and their recipients, and whitelist new addresses allowed to modify merkle roots in the `Distributor` contract. It has no ability to alter distributions.

## Merkl Engine

This is the most critical component of the whole system and as such is fully isolated and cannot be accessed from the internet. In short, at regular time intervals (between 3 and 12 hours depending on the chain) the Merkl engine performs the following:

1. **Fetches Campaigns:** Retrieves campaigns from the Merkl `DistributionCreator` contracts.
2. **Parallel Processing:** Creates independent processes for each campaign, all running in parallel. Each process executes the following actions:

- **Configuration Validation:** Ensures the campaign configuration is correct.
- **Fetches Forwarders:** Retrieves all forwarders applicable to the campaign (see [Merkl Forwarders](./#merkl-forwarders) for more information on forwarders).
- **Rewards Check:** Checks the amount of rewards already distributed for the campaign and the last distribution time.
- **Data Retrieval:** Fetches onchain data from RPC nodes, subgraphs, or other solutions to measure user activity and reward users based on these metrics.
- **Sanity Checks:** Runs several sanity checks to avoid disputes. If an issue is detected, the script will retry to compute the rewards up to three times. If the issue persists, the new rewards for the campaign will be omitted in the reward file to avoid blocking the entire system for a single campaign. These rewards will be recomputed at a later time.
- **Partial Reward File:** Creates a partial reward file for that campaign.

3. **Aggregation and Computation:** Aggregates all partial reward files into one and computes a new merkle root.
4. **Pushing Merkle Root:** Pushes the new merkle root onchain.

### Merkl Forwarders

Merkl Engine can smartly enable users to accrue rewards even if the incentivized asset isn't directly present in their wallet. Take the case of a campaign incentivizing holders of the token, but many of these tokens are staked in a staking contract: if the Merkl engine has integrated the staking contract, it can reward users which have provided liquidity on the staking contract based on how much they have staked.

This functionality to track user activity across various protocols and contracts is called Merkl `forwarders`. [Each campaign type](types-of-campaign.md) comes with its own forwarders.

Forwarders must be integrated with Merkl. Simple forwarders, such as staking contracts for ERC20 tokens are integrated by default. However, most complex forwarders, such as Active Liquidity Management (ALM) protocols on concentrated liquidity, require an integration process that may incur integration fees. To discuss integration, open a [BD ticket on our Discord](https://discord.com/channels/1209830388726243369/1210212731047776357).

Precisely speaking, the reward forwarding mechanism works as follows when calculating rewards:

1. **Initial Run:** The engine performs an initial run, distributing rewards to all eligible addresses without considering forwarders. It is important to note that whitelists and blacklists are applied during this first run. This means that if an address eligible for forwarding (e.g., a staking contract) is blacklisted, all liquidity provided by users at this address will be excluded from the reward calculation (e.g., all tokens staked in a blacklisted contract will be disregarded).
2. **Forwarder Check:** The engine identifies all addresses that received rewards and checks for addresses eligible for forwarding (e.g., a staking contract address).
3. **Forwarder Script Execution:** For each forwarder found, the engine runs scripts specific to the forwarder type to distribute the rewards to the end users (e.g., by looking at the balance of staked tokens of the users).

This mechanism ensures that users are fairly rewarded for their activity across different protocols, even when the incentivized assets are not directly in their wallets.

## Merkl rewards bucket

This bucket contains all the reward files generated by the Merkl Engine. It has 2 main purposes:

1. It is used by the dispute bots to validate that the rewards were distributed correctly.
2. It allows anyone to have a closer look at the raw output of the Merkl Engine.

If the Merkl Engine pushes a new root but does not push a reward file to this bucket within the first 30 minutes after the upload of the root, the merkle root will be disputed.

{% hint style="info" %}
The reward reports stored in the bucket for a given chain can be accessed from [the status page](https://app.merkl.xyz/status) on the Merkl app.
{% endhint %}
