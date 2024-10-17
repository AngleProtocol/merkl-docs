# 🪝 Customazibility Hooks

Beyond the different types of pools of opportunities incentivizers can choose to track, incentive providers with Merkl have the possibility to customize their campaigns with some hooks to change the default behavior of their campaigns.

While some hooks are campaign specific (e.g prevent out of range liquidity from being incentivized), others are global and their behavior is roughly the same from one campaign type to another.

Here are some of the most common hooks supported by Merkl.

{% hint style="info" %}
We are constantly adding new forms of hooks and customization tools to incentive providers on Merkl. This list may therefore not be fully up to date with what's currently supported by the Merkl engine and frontend.
{% endhint %}

## Forwarders

For some campaign types, forwarders will be automatically detected and there is no need to specify them. This applies typically to [concentrated liquidity campaigns](../campaigns/concentrated-liquidity-mechanisms.md#automated-liquidity-management-solutions) where once whitelisted automated liquidity management solutions are automatically detected and supported.

For other ERC20 campaigns, incentive providers may choose to specify forwarders (provided that these are also ERC20 tokens) likely to hold the tokens to be incentivized.

Let's illustrate this forwarding feature using an ERC20 campaign incentivizing USDA holders. With forwarding enabled, users who staked their USDA and received stUSD in exchange can still be eligible to earn rewards even if they don't hold USDA in their wallets.

Here's how it works:

- **Staking Example:** Users stake their USDA and receive stUSD tokens in return.
- **Forwarding Mechanism:** Although the staked tokens (USDA) are not in the user's wallet (as they are held in the stUSD smart contracts), users are still rewarded based on their stUSD holdings.
- **Reward Eligibility:** Merkl's forwarding mechanism can recognize their stake in the original tokens, ensuring that users with stUSD in their wallets can earn rewards.

**As a result, if the token you are incentivizing can be staked in another contract (such as staking USDA in the stUSD contract), Merkl can trace back the liquidity in the staking contract to the original user.**

For this to work, when creating a campaign you need to provide the staking contract addresses. The contract where users stake their tokens is the **recipient of the initial rewards**. The token issued when staking the token is the **token to forward rewards to**, and this contract needs to be an ERC20 token.

{% hint style="info" %}
Most of the time, these are the same contracts, so you should enter the same address twice when creating a campaign.
{% endhint %}

## Whitelisting

Incentive providers may whitelist some addresses (usually forwarders) so only the whitelisted addresses and the addresses that are associated to whitelisted addresses can be eligible to rewards.

Let's take the case of a campaign on a UniswapV3 pool with 3 supported ALMs. If an incentive provider chooses to whitelist the addresses corresponding to 2 of the 3 ALMs and to one individual user address, then only the addresses providing liquidity through the 2 ALMs and the whitelisted user providing liquidity directly on the pool will be eligible to rewards.
The reward allocation rules that apply between whitelisted addresses are the same that would apply if all addresses were eligible.

For an ERC20 Campaign incentivizing holders of a given ERC20 token on a chain, let's say two staking contract addresses are whitelisted by the incentive providers, then only the addresses which staked within one of the 2 staking contracts will be eligible to rewards and the amount of rewards they get is dependent on how much they've staked over time with respect to the total amount that has been staked over the two staking contracts.

Whitelisting always prevails over blacklisting, in the sense that if an address is whitelisted and another one is blacklisted, all other non whitelisted addresses will by default be blacklisted and not eligible to rewards.

There may be the case where some campaigns run in parallel on the same opportunity (e.g the same pool), and there is a campaign with a whitelist and a campaign without. If you're a user providing liquidity, if there is any doubt about this, you need to check the card corresponding to each campaign on the opportunity page to make sure that you understand what the APRs displayed are referring to, and what you need to do to be eligible to the whitelisted campaign in case this is of interest.

## Blacklisting

Incentive providers may choose to blacklist addresses from being eligible to rewards. If a forwarder address is blacklisted, then all the balance from the addresses associated to this forwarder will not be eligible to rewards.

Back to the case of an ERC20 campaign incentivizing holders of a given ERC20 token on a chain, if a staking contract address is blacklisted, and a user has staked 10 tokens in the contract, but holds 5 tokens in its wallet, then only its 5 tokens will be eligible to the rewards of the campaign.

More globally, if some addresses are blacklisted in a campaign, the share of all non-blacklisted users in the reward pool increases proportionally. In this case, users receive the same percentage of the rewards that the blacklisted addresses would have collected if they were not blacklisted. This ensures that the total rewards are distributed fairly among the eligible participants.

## Incentivized bridged liquidity

Merkl has partnered with Jumper to enable incentivized bridged liquidity. This feature allows chains or other incentive providers to only incentivize users to bridge liquidity from another chain, rather than moving it between protocols on the same chain. This results in a real chain-wide increase in liquidity.

In fact, it's a way to only whitelist the set of addresses which used the chosen bridge.

## Cross-chain Incentives

With Merkl, you can choose to incentivize activity on one chain while distributing rewards on another. If you don't want to deal with the hurdle of bridging your reward tokens to a given chain, this feature enables you to typically incentivize activity on a pool on Arbitrum but let people claim on Uniswap.

In this case, you need to be wary of the fact that some smart contract addresses on Arbitrum may be providing liquidity on the chain but are not deployed at the same address on Ethereum (or not deployed at all on Eth). In this case, these addresses which provided liquidity will be unable to access their rewards. It's up to you to blacklist addresses which might be concerned here.

## Boosting Rewards with Merkl

Merkl allows you to boost rewards for users holding a specific token or NFT. The formula used for boosting is similar to Curve Finance’s but offers greater flexibility, as there is no 2.5x limit on boosting rewards for a user.

{% hint style="info" %}
This boosting feature is not automatically supported for custom integrations (beyond Concentrated Liquidity campaigns and ERC20 campaigns). If you are interested, please reach out by opening a BD ticket in our Discord.
{% endhint %}

### Curve Formula

$$
B = \min \left( 2.5, 1.5 \times \frac{D \times v}{V \times d} + 1 \right)
$$

Where

- **B** is the boost a user receives.
- **d** is the value a user deposits, in USD.
- **D** is the total value deposited to the pool/vault, in USD.
- **v** is the amount of veCRV a user has (vote weight).
- **V** is the total veCRV in the system (total vote weight).

### Merkl Boost

There is no minimum value, and you can boost users holding any token as much as you want!

$$
B = b \times \frac{R \times v}{V \times r} + 1
$$

Where:

- **B** is the boost a user receives.
- **b** is the boost multiplier selected by the Incentive Provider when creating a campaign (visible in the front end in the campaign creation page).
- **R** is the total amount of rewards distributed per epoch.
- **r** is the user’s rewards per epoch.
- **V** is the total supply of the token used for the boost (for NFTs, it’s the number of NFTs in a collection).
- **v** is the amount of the token held by the user during an epoch (or the number of NFTs held if applicable).

### Boost for NFT Collections

We offer the ability to boost rewards based on the number of NFT a user holds from an NFT collection. If you'd like to set up a campaign with an NFT boost, please contact us to ensure the setup is configured accurately.
