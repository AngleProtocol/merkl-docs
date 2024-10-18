# 🏕️ Types of Campaign

Merkl is designed to be versatile and compatible with a wide range of DeFi protocols. It notably offers a variety of incentivization campaigns to cater to different DeFi activities. Here are some of the most used campaign types:

1. Campaigns for liquidity providers in Concentrated Liquidity Pools (CLAMM) such as on Uniswap V3
2. Campaigns to incentivize Lending and Borrowing on protocols like Morpho, Silo or Radiant, or to incentivize more tailored behavior on lending and borrowing protocols
3. Campaigns to airdrop tokens to a wide range of users based on a json file or on a snapshotted token balance
4. Campaigns to incentivize holders of an ERC20 token over time (ERC20 Campaigns). This campaign type can be used for a wide range of different purposes including:

- Incentivizing liquidity on any lending and borrowing protocol that uses receipt and debt tokens
- Incentivizing liquidity in a Constant Product Liquidity Pool based on the LP token balance: this works to incentivize Uniswap V2 liquidity
- Or, simply incentivizing users holding a token (e.g. wBTC, ETH, USDA, EURA, etc.) over time based on how much they're holding

In the following pages, we explain some of the technicalities and specifities associated with each campaign type on Merkl.

{% hint style="info" %}
We are constantly working on adding support for new campaign types. This list as well as the following pages may therefore not be fully up to date with what's currently supported by the Merkl engine and frontend. If you're interested in a new incentivization use case, don't hesitate to contact us by opening a [BD ticket on Merkl Discord](https://www.google.com/url?q=https://discord.gg/jnYfrGxDbe&sa=D&source=docs&ust=1714726869927696&usg=AOvVaw1loOKjqz9IGEdpNjWsvrmD).
{% endhint %}

## Common Patterns - Anti DoS

While Merkl campaigns are all different, there are some common patterns and rules that apply across all campaigns on Merkl.

Typically, the Merkl engine applies across most campaigns an anti-DoS filter. Precisely, except for airdrop campaigns, the Merkl engine cuts off rewards for users who receive less than 1/10,000,000th (or 0.00001 %) of the campaign rewards per engine run across all active campaigns.

This anti-DoS (Denial of Service) filter helps maintain the platform's integrity and therefore ensures the continuity of fair distribution of rewards.

Example on a campaign rewarding LPs on a Uniswap V2 pool:

- Campaign Duration: 14 days
- Total Value Locked of the Uniswap V2 Liquidity Pool: \$1,000,000 on Arbitrum
- Merkl Engine Script Frequency: Every 4 hours (6 times per day)

For this campaign:

- Each distribution will release 1/84th of the total rewards (1 / (14 days \* 6 times per day)).
- If a user has $10 of liquidity, it represents 1/100,000th (or 0.001%) of the total TVL ($10 / \$1,000,000 = 1/100,000 = 0.001%).
- Per script run, the user's potential reward would be 1/8,400,000th (approximatively 0.000012%) of the total rewards (1/100,000 \* 1/84), which is more than 1/10,000,000th (or 0.00001 %). Therefore, the reward would be distributed.

However, if the TVL were $2,000,000 and users had $10 of liquidity, the user's liquidity would represent 1/200,000th (or 0.0005%) of the total TVL. Per run, the potential reward would be 1/16,800,000th (1/200,000 \* 1/84, or approximatively 0.0000059%), which is in that case below the threshold (1/10,000,000th or 0.00001 %) and thus would not be distributed.
