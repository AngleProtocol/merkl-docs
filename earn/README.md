---
description: Everything you need to know to earn on Merkl
---

# Earn with Merkl

All yield opportunities using Merkl are listed at [app.merkl.xyz](https://app.merkl.xyz).

This interface displays the different campaigns and their types. For every opportunity, you may find a link for where to deposit your funds.

Once you deposit your funds or perform the actions for an opportunity incentivized with Merkl, no additional steps are required to start receiving your rewards. You will be able to claim them directly from the Merkl page or from any other app which integrates Merkl. In particular, **you will not need to stake your tokens anywhere else**.

Rewards on Merkl do not increase block by block, but can be claimed at [a frequency](./helpers.md#🔗-live-amms-and-chains) which depends on the chain.

Merkl comes with an anti DoS filter which means that positions with less than \$20 of liquidity are not eligible for incentives.

{% hint style="info" %}
Note that on Merkl, rewards can be claimed by any address on behalf of any other address. If you are integrating Merkl as a smart contract and don't want rewards to be claimed on your behalf, you can call the `toggleOnlyOperatorCanClaim` function on the `Distributor` contract (address [here](./helpers.md#🧑‍💻-smart-contracts)).
{% endhint %}

In the following pages, we explain in more details how to take advantage of the different campaign types on Merkl to earn rewards.
