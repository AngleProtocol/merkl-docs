# Token Holding Campaigns

Merkl enables the creation of token holding campaigns (formerly known as ERC20 campaigns), where **users earn incentives based on their token balance over time**.

At their simplest, these campaigns function like traditional `StakingRewards` contracts. But thanks to Merkl’s **high customizability** and **advanced forwarding mechanisms**, token holding campaigns can support far more flexible and complex use cases — going well beyond the limitations of standard staking systems.

## Campaign design

### Distribution type

In Token Holding Campaigns, users who hold an incentivized ERC20 token in their wallet earn rewards based on their balance. How those rewards are allocated depends on the [**distribution type**](../distributions.md) chosen by the campaign creator.

For example, in a [variable reward rate campaign](../distributions.md#variable-reward-rate-campaigns), rewards are distributed proportionally to each user’s share of the total token supply. So if a user holds 1% of the total supply over a given period, they’re eligible to receive 1% of the rewards distributed during that time.

### Customizability options

Merkl allows incentive providers to fully customize their incentive campaigns with [**advanced options (aka hooks)**](erc20-mechanisms.md#customizability-options) such as:

* **Whitelisting or blacklisting** specific addresses for reward eligibility.
* [**Forwarding rewards**](../../glossary.md#forwarder) to users whose tokens are staked in external contracts (e.g. LP tokens or lending positions).

## Applications

Token Holding Campaigns are highly versatile, supporting a wide range of DeFi use cases.

Any protocol that issues receipt tokens when users interact with it is automatically supported by Merkl. This notably applies to:

* **constant product AMMs** like Uniswap V2, Sushiswap, or similar liquidity pools.
* **lending and borrowing protocols** that emit receipt and debt tokens (i.e., ERC20 tokens) such as Aave. Lenders can be rewarded based on how much they lend over time, and borrowers based on how much they borrow.

{% hint style="danger" %}
Not all lending and borrowing protocols issue ERC20 receipt or debt tokens when you borrow from them. These protocols require a custom integration within Merkl. You can find more information about these specialized campaigns on our dedicated [lending & borrowing page](lending-borrowing.md).
{% endhint %}

### Full integration

For constant product AMMs and lending & borrowing protocols that issue receipt tokens, we strongly recommend **full Merkl integration**. Without it, Merkl may be unable to accurately compute APR and TVL for incentivized pools. A full integration ensures seamless calculation and visibility.

{% hint style="info" %}
If you want your Constant Product AMM, or lending and borrowing protocol to be fully integrated and supported by Merkl, or to create tailored protocol-campaigns (as we did for Silo and Radiant), please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process. **Integration allows APRs and TVL calculations**.
{% endhint %}

Additionally, some lending/borrowing protocols may want to be able to offer more tailored incentives (e.g., incentivize lenders while blacklisting those who fold their liquidity). In this case as well, we recommend a dedicated integration, as we did for Silo and Radiant.
