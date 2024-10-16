# 🏕️ Types of Campaign

Merkl is designed to be versatile and compatible with a wide range of DeFi protocols. It notably offers a variety of incentivization campaigns to cater to different DeFi activities. Here are some of the most used campaign types:

1. Campaigns for liquidity providers in Concentrated Liquidity Pools (CLAMM) like on Uniswap V3
2. Campaigns to incentivize Lending and Borrowing on protocols like Morpho, Silo or Radiant, or to incentivize more tailored behavior on lending and borrowing protocols
3. Campaigns to airdrop tokens to a wide range of users based on a json file or on a snapshotted token balance
4. Campaigns to incentivize holders of an ERC20 token over time (ERC20 Campaigns). This campaign type can be used for a wide range of different purposes including:

- Incentivizing liquidity on any non-Silo or non-Radiant lending and borrowing protocol that uses receipt and debt tokens
- Incentivizing liquidity in a Constant Product Liquidity Pool based on the LP token balance: this works to incentivize UniswapV2 liquidity
- Or, simply incentivizing users holding a token (e.g. wBTC, ETH, USDA, EURA, etc.) over time based on how much they're holding

In the following pages, we explain some of the technicalities and specifities associated with each campaign type on Merkl

{% hint style="info" %}
We are constantly working on adding support for new campaign types. This list as well as the following pages may therefore not be fully up to date with what's currently supported by the Merkl engine and frontend. If you're interested in a new incentivization use case, don't hesitate to contact us by opening a BD ticket on Merkl Discord.
{% endhint %}
