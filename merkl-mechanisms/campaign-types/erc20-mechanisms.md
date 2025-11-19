# Token Holding Campaigns

Merkl enables the creation of token holding campaigns (also known as ERC20 campaigns), where **users earn rewards based on their token balance over time**.

At their simplest, these campaigns function like traditional `StakingRewards` contracts. However, Merkl's **high customizability** and **advanced forwarding mechanisms** enable token holding campaigns to support far more flexible and complex use cases—going well beyond the limitations of standard staking systems.

## Campaign Design

### Distribution Type

In Token Holding Campaigns, users holding an incentivized ERC20 token in their wallet earn rewards based on their balance. How these rewards are allocated depends on the [distribution and scoring types](../distributions.md) and the [customization options](../customization-options.md) chosen by the campaign creator.

For example, in a [variable reward rate campaign](../distributions.md#variable-reward-rate-campaigns), rewards are distributed proportionally to each user's share of the total token supply. If a user holds 1% of the total supply over a given period, they are eligible to receive 1% of the rewards distributed during that time.

## Applications

Token Holding Campaigns are highly versatile, supporting a wide range of DeFi use cases.

Any protocol that issues receipt tokens when users interact with it is automatically supported by Merkl. This includes:

* **Constant product AMMs**: Uniswap V2, SushiSwap, and similar liquidity pools
* **Lending and borrowing protocols**: Platforms that emit receipt and debt tokens (ERC20 tokens) such as Aave—lenders can be rewarded based on their lending amount over time, and borrowers based on their borrowing amount
* **Perpetual DEXes**: Perp protocols that issue receipt tokens to depositors in trading vaults

{% hint style="danger" %}
Not all lending and borrowing protocols issue ERC20 receipt or debt tokens when users borrow from them. These protocols require a custom integration within Merkl. You can find more information about these specialized campaigns on the dedicated [lending & borrowing page](lending-borrowing.md).

Additionally, some lending/borrowing protocols may require more tailored incentives (e.g., incentivizing lenders while blacklisting users who fold their liquidity). In such cases, a dedicated integration is recommended, similar to the custom integration implemented for Aave.
{% endhint %}

### Merkl API Integration

The logic for reward computation is independent from the API logic used to compute APRs and TVLs for the associated opportunity.

For the protocol it integrates, the Merkl system can automatically recognize whether a given ERC20 token originates from a specific protocol, and then compute the associated TVL and APR if a campaign is running on it.

If you are unsure whether the Merkl API will properly recognize your token as coming from your protocol before creating a campaign, please [contact us on the Merkl Discord by opening a BD ticket](https://discord.com/invite/jnYfrGxDbe) to discuss the integration process.