---
description: Integrate Merkl in your app using Merkl API V4
---

# 🧑‍💻 Integrate Merkl to your app

While the [Merkl App](https://app.merkl.xyz/) offers a comprehensive interface that provides users and Liquidity Providers with everything they need to take advantage of Merkl opportunities, Merkl is also designed so anyone can integrate Merkl data into its own application and use it as a white-label solution.

This guide aims at walking you through the integration of Merkl in your app via the Merkl API.

{% hint style="info" %}
If you use Merkl as a white-label solution in your frontend, you must integrate [our logo](./branding-and-integration.md) with a clickable link that redirects to our app.
{% endhint %}

Merkl provides an API, available [here](https://api.merkl.xyz) that contains all the information you need to track campaigns and selectively display the ones you want on your frontend.
All the data from the Merkl app is served by this API.

The documentation for can be found [here](https://api.merkl.xyz/v4/docs). We encourage you to use this documentation for reference after having read the paragraphs below.

{% hint style="info" %}
As you browse through the documentation, you'll notice that some API parameters are optional. Keep in mind though that specifying more parameters can speed up the result of your API queries.
{% endhint %}

## Displaying campaigns, opportunities and rewards

To show Merkl data in your front-end, you have 2 types of data you can use:

- `Campaigns`: these are programs running on a given pool, ERC20 token, etc, over a period of time
- `Opportunities`: these are groups of campaigns targeting the same user base (the same pool, ERC20 token, ...). Typically, multiple campaigns can run in parallel for liquidity providers on a pool, and these all make up an opportunity.

### Campaigns

Assuming you've have created a campaign using PYTH as a reward token targeting the Euler Vault `0x82D2CE1f71cbe391c05E21132811e5172d51A6EE`, you can find this campaign's data using the following endpoint: [https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH](https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH).

Here, you will find data related to this campaign start and end date, the amount of PYTH streamed, its ID, ...

{% hint style="info" %}
Each campaign on Merkl is identified on the Merkl API by a unique ID.
{% endhint %}

There are different filters available to find the campaigns that are relevant for you. You may browse the available filters [here](https://api.merkl.xyz/v4/docs#tag/campaigns/GET/v4/campaigns/).

### Opportunities

Now, you may want to display data about all the campaigns targeting this pool.

This will enable you to display aggregated data about all these campaigns, like the resulting APR of these all.

Taking the example from above, you may get the opportunity corresponding to the Euler vault by using: [https://api.merkl.xyz/v4/opportunities?name=Euler](https://api.merkl.xyz/v4/opportunities?name=Euler).

This will give you aggregated data related to the opportunity like daily rewards, APRs, TVLs, etc.

Once again, there are different filters available to find the opportunities of your choice. You may browse the different filters for opportunities [here](https://api.merkl.xyz/v4/docs#tag/opportunities/GET/v4/opportunities/).

### Rewards

The API docs explains [here](https://api.merkl.xyz/v4/docs#tag/users/GET/v4/users/{address}/rewards) how to find the rewards earned by a user through Merkl.

Essentially, you need to specify the address you want to find rewards for as well as the chainIds for which you want to compute rewards.

Here is an example: [https://api.merkl.xyz/v4/users/0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185/rewards?chainId=1,10](https://api.merkl.xyz/v4/users/0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185/rewards?chainId=1,10).

## Claiming user rewards

Rewards on Merkl are claimable per token: meaning that if a user has accumulated rewards of several tokens, they may choose to only claim their rewards of one token type, just like they can choose to claim all their token rewards at once.

The contract on which rewards should be claimed is the `Distributor` contract which address across different chains can be found on this [page](https://app.merkl.xyz/status).

There are different options with which you can help your users claim their rewards:

- **Rely on the data provided by the `Rewards` API route mentioned above (recommended):** In this case the Merkl API builds entirely the claim transaction payload and the associated proof. All you need to do is call the Merkl API. This is the example shown below.
- **Build the proof yourself:** In this setting, once the proof is built on your end, you may join it to the transaction data from the Merkl API. You can find a Github repository below showing how to do that. If you need further help, please reach out to us on Discord or Telegram.

{% hint style="info" %}
In any case, if a call is made to the correct `Distributor` contract and the `token` or `amount` do not match the `proof`, the transaction will revert.
{% endhint %}

Here is a script you may use to claim all the token rewards for a user on a chain.

```javascript
import type { JsonRpcSigner } from '@ethersproject/providers'
import { MerklApi } from '@merkl/api'
import { Distributor__factory } from '@sdk' // ABI can be fetched on etherscan

const DISTRIBUTOR_ADDRESS = '0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae'

export const claim = async (chainId: number, signer: JsonRpcSigner) => {
  const { status, data } = await MerklApi('https://api.merkl.xyz')
    .v4.users({ address: signer._address })
    .rewards.get({ query: { chainId: [chainId] } })
  if (status !== 200) throw 'Failed to fetch rewards'

  const users = []
  const tokens = []
  const amounts = []
  const proofs = []

  for (const rewards of data) {
    if (rewards.chain.id !== chainId) continue
    for (const reward of rewards.rewards) {
      users.push(signer._address)
      tokens.push(reward.token.address)
      amounts.push(reward.amount)
      proofs.push(reward.proofs)
    }
  }

  if (tokens.length === 0) throw 'No tokens to claim'

  const contract = Distributor__factory.connect(DISTRIBUTOR_ADDRESS, signer)
  await (await contract.claim(users, tokens, amounts, proofs)).wait()
}
```

## Benefit from Typescript typings when using the Merkl API

Interacting safely with Merkl API is possible using our NPM package. See our guide [here](./merkl-api-package.md).
