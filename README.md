---
description: Your official source of information for everything regarding the Merkl platform.
cover: ./.gitbook/assets/merkl-cover.jpg
coverY: 0
---

# 🥨 Overview

Merkl is the largest liquidity marketplace in defi with over $1M of assets being distributed by incentive providers every week to liquidity providers accross 17+ chains.

Thanks to Merkl, users can benefit from boosted APRs on liquidity pools, lending protocols, yield optimizers and more!

Sounds too good to be true? Check out our [stats](https://app.merkl.xyz/stats)!

## You are a liquidity provider looking for competitive APRs

Merkl is a one stop shop to find the best investment opportunities in Defi and it only takes a few clicks to get started!

1. Visit the [Merkl App](https://app.merkl.xyz/)
2. Explore the various opportunities and find the one which suits you best
3. Invest your liquidity directly on the protocol's app by following the links
4. That's it! You're now earning rewards, no staking or other on chain action is needed

You can track your active positions and your rewards by visiting your [user dashboard](https://beta.merkl.xyz/user/)

Once you've earned rewards, you can claim all your earned tokens with a single click.

Happy farming!

{% hint style="info" %}
Merkl is a **non custodial solution**. Earning rewards on Merkl incurs **no risk of funds** and requires no specific smart contract interactions. Liquidity Providers can retain the custody of their liquidity while receiving rewards. They only need to interact with Merkl contracts to claim their rewards.
{% endhint %}

## You are a protocol looking to grow your liquidity

With Merkl, you can start you incentives campaigns in a matter of minutes, you are responsible for configuring how you want to reward liquidity providers, Merkl handles the rest!

Want to get started with Merkl? Follow these steps:

1. Visit the [Merkl campaign creation page](https://app.merkl.xyz/create)
2. Select the type of campaign you would like to run (not finding what you're looking for? Please reach out [here]())
3. Select the token you would like to distribute (not finding your token? Fill this [form](https://tally.so/forms/3y2bqx))
4. Configure your campaign
5. Execute the campaign creation transaction from our frontend by generating the Gnosis Safe transaction batch provided
6. That's it! Your campaign will take up to 30min to show up in the Merkl app and be visible to the Liquidity Providers

Merkl provides an API which you can use to integrate all Merkl related data into your frontend, everything is documented [here](distribute/integrate/integration-guide.md)

Check out the complete guide for creating campaigns [here](./distribute/README.md)

## How Merkl works

Merkl was built to incentivize any complex onchain and offchain behavior. It can not only replicate any type of staking contract, it can also do much more and reward in a tailored manner any onchain or offchain action, or combination of actions.

Merkl's custom engine is conceived as an agnostic powerhouse enabling everyone to unlock liquidity based on their exact needs without overspending or allocating tokens to reward behaviors/users/actions that are not beneficial to the incentive providers. Merkl is for instance much more expressive and efficient at shaping liquidity on concentrated liquidity pools than other solutions available on the market.

### Merkl building blocks

Merkl is a hybrid solution with both onchain and offchain components to benefit from the security of onchain data and from the flexibility of offchain scripts. It can be broken down into 3 main components:

- **Merkl campaign creator contract:** contract to create Merkl campaigns, this is the contract incentive providers interact with when creating campaigns.
- **Merkl distributor contract:** contract which holds all the rewards, this is the contract liquidity providers interact with when claiming their rewards.
- **Merkl Engine:** Off chain infrastructure which runs every 8 hours to allocate rewards to liquidity providers and push them on chain

### Merkl campaigns

Incentives are distributed by deploying **Merkl Campaigns** in which incentive providers select a token to distribute, an amount of that same token and a timeframe over which the token should be distributed. Once a campaign is created, rewards are then regularly streamed to eligible liquidity providers through onchain merkle root updates.

While the set of behaviors that can be incentivized with Merkl is infinite, **Merkl provides the ability to add custom rules to every campaign such as:**

- only rewarding users which matched the eligibility criterias for a set amount of time
- blacklist or whitelist some addresses for every campaigns
- boosting rewards of the users which hold a specific token

Merkl supports many more custom rules which are specific to the type of onchain behavior which is incentivized, it can currently reward:

- liquidity providers on concentrated liquidity protocols (like Uniswap V3)
- liquidity providers on lending protocols
- liquidity providers of any protocol which issues ERC20 tokens such as yield aggregators

{% hint style="info" %}
Merkl offchain logic allows it to easily support new and complex use cases. If there is a specific use case that you are interested in incentivizing efficiently (e.g. users of a perp DEX protocol, borrowers of a lending protocol that are not folding their positions, ...), contact us on [Merkl Discord](https://discord.gg/jnYfrGxDbe).
{% endhint %}

A key feature of Merkl is its ability to trace user activity across protocols. This enables users to accrue rewards even if the incentivized asset isn't directly present in their wallet, the rewards which would have been accrued by a smart contract holding the incentivized asset are forwarded the user which deposited tokens in it (see [Merkl Forwarders](merkl-mechanism#merkl-forwarders))

{% hint style="info" %}
For example, when an ERC20 holder stakes the ERC20 token in a staking contract, the address of the user no longer holds the incentivized ERC20 token. Merkl will detect ERC20 tokens held by the staking contract and forward the rewards to the original user.
{% endhint %}

## ⚱️ Fee Structure

Excluding gas, Merkl is free to use for liquidity providers claiming rewards. There is a maintenance fee applied to incentives that are sent by incentive providers. This fee is different for each type of campaign (between 0.5% and 3%) and can be decreased for large incentive providers.

It can also be waived for incentives which contain some specific approved tokens. In particular, there are no fees for incentives sent to pools which have Angle tokens in it.

{% hint style="info" %}
Merkl is built and maintained by Angle Labs, but is separate from the Angle Protocol.
{% endhint %}

## Resources

### 📖 Guides

The system for launching campaigns and claiming rewards **can be easily integrated on any dApp**. [This guide](./distribute/integrate/integration-guide.md) explains among other things how to integrate some Merkl campaigns in your frontend and how to build claim transactions for your users.

If you want to use Merk to farm, check out these guides to [make the best of Merkl as a liquidity provider](./earn/README.md) or to [distribute incentives](./distribute/README.md) with the system.

### Audits

Merkl smart contracts have been audited by Code4rena. Find the audit report [here](https://code4rena.com/reports/2023-06-angle).

### 🔗 Links

- [Merkl App](https://app.merkl.xyz)
- [Terms & Conditions](./distribute/incentivizor-tc.md)
- [Smart contracts addresses](./addresses.md)
- [Smart contracts code](https://github.com/AngleProtocol/merkl-contracts)
- [Track all rewards distributed through Merkl](https://rewards.merkl.xyz/)
- [Open-source dispute bot](https://github.com/AngleProtocol/merkl-dispute)
