# 🧑‍🌾 ERC20 Campaigns and Applications

With Merkl, you can create what we call ERC20 campaigns that is to say campaigns where users are rewarded based on their time-weighted balance of a token.

These campaigns, in their simplest form, are equivalent to classical `StakingRewards` contracts and can serve a wide range of use cases: providing liquidity in constant product formula decentralized exchanges (like UniswapV2). With Merkl forwarding technology, this simple mechanism can be extended to seamlessly support rewarding tokens staked in periphery contracts or blacklisting some addresses.

## ERC20 Campaign Design

In Merkl ERC20 Campaigns, users holding an ERC20 token that is rewarded by an incentivizer in their wallets are rewarded based on their token balance. Reward distribution is proportional to the percentage of the total token supply that a user holds.

For example, if a user holds 1% of the total token supply for a period of time, they are eligible for 1% of the reward pool for this period of time.

### Blacklisting

When creating a campaign, Merkl incentive providers have the possibility to blacklist or whitelist some specific addresses which should not or should be the only ones receiving rewards.

In the case of blacklisting, if some addresses are blacklisted, the user's share of the reward pool will increase proportionally. In this case, the user will receive the same percentage of the rewards that the blacklisted addresses would have collected if they were not blacklisted. This ensures that the total rewards are distributed fairly among the eligible participants.

### ERC20 Campaign Forwarders: Staking Mechanism

With ERC20 Campaigns, Merkl supports a staking mechanism where users can earn rewards even if the incentivized asset is not directly present in their wallet.

For example, for a USDA incentivization campaign, users who staked their USDA and received stUSD in exchange can still be eligible to earn rewards. The forwarder includes users who have staked their USDA, ensuring they receive rewards despite not having the original tokens in their wallets since their USDA are locked in the stUSD smart contracts.

Here's how it works:

- **Staking Example:** Users stake their USDA and receive stUSD tokens in return.
- **Forwarding Mechanism:** Although the staked tokens (USDA) are not in the user's wallet (as they are held in the stUSD smart contracts), users are still rewarded based on their stUSD holdings.
- **Reward Eligibility:** Merkl's forwarding mechanism ensures that users with stUSD in their wallets can earn rewards, recognizing their stake in the original tokens.

**As a result, if the token you are incentivizing can be staked in another contract (such as staking USDA in the stUSD contract), Merkl can trace back the liquidity in the staking contract to the original user.** For this to work, when creating a campaign you need to provide the staking contract addresses. The contract where users stake their tokens is the **recipient of the initial rewards**. The token issued when staking the token is the **token to forward rewards to**, and this contract needs to be an ERC20 token.

{% hint style="info" %}
Most of the time, these are the same contracts, so you should enter the same address twice when creating a campaign
{% endhint %}

## Applications

The advantage of ERC20 campaigns is that they can cover a wide range of different use cases. Technically, all the protocols which give their users a receipt token when they deposit into it or take action with it are by default supported within Merkl, ensuring that users participating in different activities can earn rewards.

This notably applies to:

- constant product AMMs (like Uniswap V2): any incentivizer can use Merkl to incentivize Liquidity Providers on a UniswapV2 or Sushiswap pool based on how much liquidity they provide with respect to others
- lending and borrowing protocols that emit receipt and debt tokens (i.e., ERC20 tokens) when users lend and borrow: lenders can be rewarded based on how much they lend over time or borrowers based on how much they borrow relatively to others

{% hint style="info" %}
Not all lending and borrowing protocols issue ERC20 receipt or debt tokens when you borrow from them. These protocols require a custom integration within Merkl. You can find more details on these dedicated lending and borrowing campaigns [here](./lending-borrowing.md).
{% endhint %}

It is important to note that for the constant product AMMs and lending and borrowing protocols that issue receipt tokens, **we still strongly recommend you to be fully supported by Merkl**.
There are some cases otherwise where the Merkl API may not be able to correctly compute the APR and TVL of the pools being incentivizing, and with a full Merkl integration this can all be solved seamlessly.

{% hint style="info" %}
If you want your Constant Product AMM, or lending and borrowing protocol to be fully integrated and supported by Merkl, or to create tailored protocol-campaigns (as we did for Silo and Radiant), please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process. **Integration allows APRs and TVL calculations**.
{% endhint %}

Some of the fully supported lending and borrowing protocols on Merkl include Compound, Gearbox, Sturdy, Aave, Ionic. For the constant product AMMs, Uniswap V2, Balancer V2, Velodrome V2, Aerodrome V2 and Quickswap V2 are all fully supported.

{% hint style="info" %}
Additionally, some lending/borrowing protocols may want to be able to offer more tailored incentives (e.g incentivize lenders but while blacklisting those who fold their liquidity). In this case as well, we recommend a dedicated integration, like we did typically for Silo and Radiant.
{% endhint %}
