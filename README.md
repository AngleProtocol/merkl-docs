---
description: >-
  Your official source of information for everything regarding the Merkl
  platform
---

# 🥨 Merkl Documentation Portal

## What is Merkl?

Merkl is a platform that bridges users in DeFi with liquidity, enabling them to deploy it to incentive providers aiming to boost activity and liquidity in their protocols.

Users can leverage Merkl to earn rewards or points by participating in various incentive campaigns, while incentive providers can utilize the platform to meet all their incentivization and growth needs - whether it is tracking points earned by their users, attracting more lenders, borrowers, and liquidity providers, executing retroactive token airdrops, or addressing other unique use cases specific to their protocols.

In short, Merkl allows users to effortlessly earn competitive returns and access attractive opportunities, while enabling incentive providers to launch complex growth and incentive campaigns swiftly and without the usual operational burdens.

Merkl is built on top of an offchain engine that leverages onchain and offchain data to compute reward and point allocations for campaigns created by incentive providers.

This system leaves the flexibility for Merkl to support tracking and rewarding virtually any type of offchain or onchain DeFi activity. Typically, Merkl currently supports incentives for providing liquidity in both concentrated (e.g., Uniswap V3) and constant product (e.g., UniswapV2) pools, lending and borrowing across a wide range of different protocols, providing liquidity through a yield aggregator, holding a token over a given period of time. Protocols also use Merkl to execute retroactive token airdrops or as a backend infrastructure to power their point systems.

## What sets Merkl apart?

### For Users

- **Competitive APRs:** With over \$60M distributed and multiple points program powered, Merkl stands as an all-in-one incentivization hub offering attractive returns and opportunities. Merkl offers the possibility to participate in multiple campaigns that incentivize the same asset or pool, allowing users to be eligible for rewards from various incentive providers and therefore maximize their earning potential.
- **No Staking Required:** Users don’t need to stake any assets to earn rewards or points with Merkl: simply participate in the eligible activities and start earning. This also means there's no extra cost or additional smart contract risk for users.
- **Gas Optimization:** Merkl’s system allows users to claim all their rewards in a single transaction, minimizing gas fees and simplifying the process - in addition to saving costs from not having to stake.
- **A Hub:** With over 12,800 campaigns and many of the most competitive opportunties in DeFi, users can see all the available campaigns in one unified front end. Merkl users can also view all their active positions and rewards on a single page.

{% hint style="info" %}
Merkl is a **non custodial solution**. Earning rewards on Merkl incurs **no additional risk of funds** and requires no specific smart contract interactions. Liquidity Providers can retain the custody of their liquidity while receiving rewards. They only need to interact with Merkl contracts to claim their rewards.
{% endhint %}

### For Protocols

- **Easy and Simple to Start Incentivizing:** In less than 3 minutes, anyone from your team can start a complex incentivization campaign. With Merkl, you can focus on your core product and reduce the time for launching growth/incentive campaigns from weeks to minutes.
- **Coverage:** Merkl offers a variety of incentivization methods (also known as campaign types) and supports incentives for a wide range of different protocols, chains and actions. This includes among other things:
  - **Liquidity Incentives**: Campaigns for liquidity providers in Concentrated Liquidity Pools (CLAMM) such as on Uniswap V3
  - **Airdrops**: Campaigns to airdrop tokens to a wide range of users based on a json file or based on their token holdings at a given moment in time
  - **Lending/Borrowing**: Campaigns for lenders and borrowers of various types of lending protocols including Aave, Compound, Morpho, Euler, Silo, Radiant, Dolomite, among the many supported by Merkl
  - **Token Balance**: Campaigns to reward simple holders of ERC20 tokens based on their relative balance over time. This can typically apply to holders of LP tokens on liquidity pools.
- **Flexibility:** Customize your campaigns to suit your specific needs and goals, Merkl provides the flexibility to incentivize various behaviors and actions. Merkl can handle multiple campaigns for the same asset, just like it allows for complex reward structures and broad participation incentives. You also don't necessarily need to give rewards and may use Merkl as a backend to run your points programs. On every Merkl campaign, you may choose among a set of options to incentivize the exact liquidity that you need. This includes notably:
  - **Crosschain Incentives**: Incentivize an asset on a chain and distribute rewards on another one.
  - **Reward Boosts**: Increase rewards for users holding specific ERC20 tokens in their wallets.
  - **Blacklist / Whitelist**: Blacklist some users from rewards (e.g sanctioned crypto wallets published by government associations such as the US’s OFAC and the UK’s FCDO), whitelist a set of wallets types that you want to be eligible (e.g., only users who provide liquidity through a concentrated liquidity pool through a given liquidity management solution)
- **Smart rewards forwarding:** In many cases, the Merkl engine knows how to recognize a smart contract from an externally owned account, just like it knows how to make the difference between a staking contract and an account abstraction wallet. When rewarding a given address, Merkl can smartly forward the rewards that were meant for a smart contract address to the addresses that provided liquidity to it (e.g to the addresses which staked into a staking contract). With this, Merkl can seamlessly integrate into any existing reward flow and people who previously integrated your systems do not need to update their contracts to fit with a new incentive method
- **Easy integration:** Integrate Merkl's data (APRs, user rewards, claiming data, analytics, ...) into your app effortlessly with the fully maintained Merkl API. However, you don't have to integrate it if you don't want to: the Merkl App will show your users everything they need, from reward APRs to TVL in the corresponding pools including the ability to claim their tokens directly from the app.
- **Advanced Analytics:** Incentive providers on Merkl get access to detailed analytics on their campaigns (addresses rewarded over time, liquidity sources breakdown, claiming data, ...)

By combining a user-centric approach with powerful tools for protocols, Merkl is setting a new standard for DeFi incentivization. Its comprehensive and innovative solutions make it the go-to platform for anyone looking to maximize their rewards or efficiently distribute incentives. With Merkl, both users and protocols can achieve their incentive goals more effectively and efficiently than ever before.

Sounds too good to be true? Check out our [stats](https://app.merkl.xyz/stats)!

## How to get started?

### Earning Rewards with Merkl

If you want to start earning incentives, check out the live campaigns [here](https://app.merkl.xyz/)

If you want learn more how to use Merkl, click [here](./earning-with-merkl/README.md).

### Distributing Rewards with Merkl

For more details on how Merkl works under the hood, you can check this [page](https://docs.merkl.xyz/merkl-mechanisms/technical-overview).

If you want to learn more about the different types of campaigns that Merkl supports, you may check our docs page [here](./mechanisms/types-of-campaign.md).

To get started and learn how to create campaigns using [Merkl's campaign creation page](https://app.merkl.xyz/create), you may use [our guides](./distribute-with-merkl/README.md).

## 🔗 Links

- [Merkl App](https://app.merkl.xyz/)
- [Merkl Website](https://merkl.xyz/)
- [Twitter](https://x.com/merkl_xyz)
- [Discord](https://discord.com/invite/Gs8MUrUVP3)
- [Audit](https://code4rena.com/reports/2023-06-angle)

## 📩 Contact us

Merkl is an ever-evolving project with new features and capabilities added every day.

If you want Merkl to support specific use cases (e.g., incentivizing users of a perpetual DEX protocol, incentivizing borrowers of a lending protocol who are not folding their positions, creating incentivization programs for yield aggregators, etc.) and leverage Merkl's capabilities, or if you have questions on what's optimal for your points program or incentive campaign, contact us on [Merkl Discord by opening a BD ticket.](https://www.google.com/url?q=https://discord.gg/jnYfrGxDbe&sa=D&source=docs&ust=1714726869927696&usg=AOvVaw1loOKjqz9IGEdpNjWsvrmD)
