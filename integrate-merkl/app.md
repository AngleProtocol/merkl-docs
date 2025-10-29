---
description: Integrate Merkl in your app using Merkl API V4
---

# Integrate Merkl to your App - Merkl API V4 Docs

While the [Merkl App](https://app.merkl.xyz/) offers a comprehensive interface that provides users and Liquidity Providers with everything they need to take advantage of Merkl opportunities, Merkl is also designed so anyone can integrate Merkl data into its own application and use it as a white-label solution.

This guide aims at walking you through the integration of Merkl in your app via the Merkl API.

{% hint style="info" %}
If you use Merkl as a white-label solution in your frontend, you must integrate [our logo](branding-and-integration.md) with a clickable link that redirects to our app.
{% endhint %}

Merkl provides an API, available [here](https://api.merkl.xyz/docs) that contains all the information you need to track campaigns and selectively display the ones you want on your frontend.\
All the data from the Merkl app is served by this API.
We encourage you to use the API documentation for reference after having read the paragraphs below.

{% hint style="info" %}
As you browse through the documentation, you'll notice that some API parameters are optional. Keep in mind though that specifying more parameters can speed up the result of your API queries.
{% endhint %}

## Understanding the difference between Opportunities and Campaigns

To show Merkl data in your front-end, you have 2 types of data you can use:

- `Campaigns`: these are programs running on a given pool, ERC20 token, etc, over a period of time
- `Opportunities`: these are groups of campaigns targeting the same user base (the same pool, ERC20 token, ...). Typically, multiple campaigns can run in parallel for liquidity providers on a pool, and these all make up an opportunity.

**Important note**: there are two types of `ids` used by merkl to identify campaigns & opportunities:

- `campaignId`: this refers to the onchain identity of a campaign (in 0x... format). This one is not unique: there can be multiple campaigns across different chains with the same `campaignId`. It can be easily found in the [opportunities page](https://app.merkl.xyz/) by looking into a speficic opportunity and going in the campaign details, then in the "advanced" tab.

- `id`: this refers to the campaign's code stored in our database (e.g 13972358188887408622). It is unique and is used to retrieve data for most of the routes. You can find it by using the 0x.. `campaignId` on this route [https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/](https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/)

You can find a list of the most frequently used endpoints in the [campaign management page](https://docs.merkl.xyz/distribute-with-merkl/campaign-management).

## Integrating APRs Data into your frontend

1. **Retrieve the Opportunity id**:

   - If you're a DeFi protocol integrating APRs for **your own opportunities**, query this API route with your protocol name:

     ```
     https://api.merkl.xyz/v4/opportunities?name={protocol_name}
     ```

     For example, for Izumi:

     ```
     https://api.merkl.xyz/v4/opportunities?name=Izumi
     ```

     Just below the `tags` of an opportunity, you’ll find the opportunity id. Copy it for later.

   - If your opportunities are across multiple protocols, we recommend using this route:

     ```
     https://api.merkl.xyz/v4/opportunities?tags={tag}
     ```

     Example: Display all campaigns associated with the Ignite Program on zkSync:

     ```
     https://api.merkl.xyz/v4/opportunities?tags=zksync
     ```

     If you want a tag added to an opportunity, feel free to reach out, and we will assign the campaign to your protocol’s tag. Similarly, below `tags`, you’ll find the opportunity id. Copy it for later.

2. **Retrieve APR Data**:

   - Once you have the opportunity id, call the following route, replacing `{id}` with your opportunity id:

     ```
     https://api.merkl.xyz/v4/opportunities/{id}
     ```

     Example for `SyncswapV3 USDC-wUSDM 0.3%`:

     ```
     https://api.merkl.xyz/v4/opportunities/17697067541467596262
     ```

     This route provides details such as APR, TVL, and daily rewards.

## Retrieving Campaign data

Assuming you've created a campaign using \$PYTH as a reward token targeting the Euler Vault `0x82D2CE1f71cbe391c05E21132811e5172d51A6EE`, you can find this campaign's data using the following endpoint: [https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH](https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH).

Here, you will find data related to this campaign start and end date, the amount of PYTH streamed, its id, etc.

{% hint style="info" %}
Each campaign on Merkl is identified on the Merkl API by a unique id. You can find it by using the 0x.. `campaignId` on this route https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/
{% endhint %}

After obtaining the `databaseId` of a campaign, you can use it to retrieve data related to this specific campaign using this route: [https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/{id}](https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/{id})

There are different filters available to find your campaigns. You may browse the available filters [here](https://api.merkl.xyz/docs#tag/campaigns/GET/v4/campaigns/).

## Retrieving Opportunity Data

Now, you may want to display data about all the campaigns targeting this pool.

This will enable you to display aggregated data about all these campaigns.

The two routes we recommend using are:

- `https://api.merkl.xyz/v4/opportunities?name={protocol_name}`
- `https://api.merkl.xyz/v4/opportunities?tags={tag}`

Taking the example from above, you may get the opportunity corresponding to the Euler vault by using: [https://api.merkl.xyz/v4/opportunities?name=Euler](https://api.merkl.xyz/v4/opportunities?name=Euler).

This will give you aggregated data related to the opportunity like daily rewards, APRs, TVLs, etc.

You can also retrieve data for a specific opportunity using the following route: [https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/{id}](https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/{id}) using the `databaseId`.

Once again, there are different filters available to find the opportunities of your choice. You may browse the different filters for opportunities [here](https://api.merkl.xyz/docs#tag/opportunities/GET/v4/opportunities/).

## Integrating User Rewards

For protocols integrating user rewards, they need to call the following API route to display the rewards earned by users:

```
https://api.merkl.xyz/v4/users/{address}/rewards?chainId={chain_id}
```

Example: Checking a user’s rewards on zkSync:

```
https://api.merkl.xyz/v4/users/0x4F2BF7469Bc38d1aE779b1F4affC588f35E60973/rewards?chainId=324
```

This route provides:

- Amount: the total amount of tokens to have been credited to the user onchain.
- Pending: the pending rewards that will be credited onchain on the next update.
- Claimed: the number of already claimed tokens.
- Proofs that will assist in integrating the claiming of rewards (details in the next section).


We do not yet return time-series data through our API. However, if you need historical data, you could take a daily snapshot of our API to build such dataset.

## Claiming user rewards

Rewards on Merkl are claimable per token: meaning that if a user has accumulated rewards of several tokens, they may choose to only claim their rewards of one token type, just like they can choose to claim all their token rewards at once.

The contract on which rewards should be claimed is the `Distributor` contract, which address across different chains can be found on this [page](https://app.merkl.xyz/status).

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

Interacting safely with Merkl API is possible using our NPM package. See our guide [here](https://www.npmjs.com/package/@merkl/api?activeTab=readme).
