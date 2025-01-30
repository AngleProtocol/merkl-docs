# 🧑‍🌾 ERC20 Campaigns and Applications

Merkl enables the creation of ERC20 campaigns—reward programs where users earn incentives based on their balance of a token over time.

At their core, these campaigns function similarly to traditional `StakingRewards` contracts. However, with Merkl’s forwarding technology, they can be extended to support more advanced use cases, such as:

- Rewarding liquidity providers in constant product AMMs (e.g., Uniswap V2).
- Supporting tokens staked in periphery contracts (through Merkl forwarder technology)
- Blacklisting specific addresses from receiving rewards.

With Merkl, you can create what we call ERC20 campaigns - campaigns where users are rewarded based on their time-weighted balance of a token.

These campaigns, in their simplest form, are equivalent to classical `StakingRewards` contracts and can serve a wide range of use cases: providing liquidity in constant product formula decentralized exchanges (like Uniswap V2). With Merkl forwarding technology, this simple mechanism can be extended to seamlessly support rewarding tokens staked in periphery contracts or blacklisting some addresses.

## ERC20 Campaign Design

In Merkl ERC20 Campaigns, users holding an incentivized ERC20 token in their wallets are rewarded based on their balance. The way they earn rewards based on their balance over time then depends on the distribution type chosen by the campaign creator.

Typically, in a variable APR campaign, reward distribution is proportional to the percentage of the total token supply a user holds. For example, if a user holds 1% of the total token supply for a period of time, they are eligible for 1% of the reward pool for this period of time.

### Customizability Hooks

Merkl allows incentive providers to customize campaign behavior through hooks, enabling:

- Blacklisting or whitelisting specific addresses for reward eligibility.
- Forwarding rewards to users whose tokens are staked in external contracts (e.g., LP tokens or lending deposits).

These hooks significantly enhance the flexibility of ERC20 campaigns beyond the limitations of traditional onchain `StakingRewards` contracts.

{% hint style="info" %} For more details on customizability hooks, refer to [this page](../hooks/README.md). {% endhint %}

## Applications

ERC20 campaigns are highly versatile, supporting a wide range of DeFi use cases. Any protocol that issues receipt tokens (e.g., LP tokens, debt tokens) when users interact with it is automatically supported by Merkl.

This notably applies to:

- constant product AMMs (like Uniswap V2): any incentivizer can use Merkl to incentivize Liquidity Providers on a Uniswap V2 or Sushiswap pool
- lending and borrowing protocols that emit receipt and debt tokens (i.e., ERC20 tokens) when users lend and borrow: lenders can be rewarded based on how much they lend over time, or borrowers based on how much they borrow

{% hint style="info" %}
Not all lending and borrowing protocols issue ERC20 receipt or debt tokens when you borrow from them. These protocols require a custom integration within Merkl. You can find more details on these dedicated lending and borrowing campaigns [here](./lending-borrowing.md).
{% endhint %}

For constant product AMMs and lending/borrowing protocols that issue receipt tokens, we strongly recommend full Merkl integration. Without it, Merkl may be unable to accurately compute APR and TVL for incentivized pools. A full integration ensures seamless calculation and visibility.

{% hint style="info" %}
If you want your Constant Product AMM, or lending and borrowing protocol to be fully integrated and supported by Merkl, or to create tailored protocol-campaigns (as we did for Silo and Radiant), please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process. **Integration allows APRs and TVL calculations**.
{% endhint %}

{% hint style="info" %}
Additionally, some lending/borrowing protocols may want to be able to offer more tailored incentives (e.g., incentivize lenders while blacklisting those who fold their liquidity). In this case as well, we recommend a dedicated integration, as we did for Silo and Radiant.
{% endhint %}
