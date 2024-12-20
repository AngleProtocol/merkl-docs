---
description: Guide to integrate Merkl in your app
---

You can integrate Merkl data directly into your application and use Merkl as a white-label solution. However, this is entirely up to you. Regardless of your choice, the [Merkl App](https://app.merkl.xyz/) offers a comprehensive interface that provides your users with everything they need to utilize Merkl, including the ability to claim their tokens directly. This guide will walk you through the various routes available via the Merkl API.

<figure><img src="../../.gitbook/assets/merkl-front-integration.png" alt=""><figcaption></figcaption></figure>

If you choose to use Merkl as a white-label solution, you will need to integrate our logo with a clickable link that redirects to our app.

## Merkl Front Integration: Options and Implementation

**The Merkl API** is fully maintained by Angle Labs, and **contains all the information needed to integrate Merkl into your frontend. Some parameters are optional, but you should kep in mind that specifying more parameters can speed up the result.**

The Merkl API is available here [https://api.merkl.xyz](https://api.merkl.xyz) and contains all the data you need to track campaigns. All the data from the Merkl app is served by this API.

The documentation for can be found [here](https://api.merkl.xyz/v4/docs). We encourage you to use this documentation after having read the paragraphs below.

### Displaying campaigns and opportunities

To show Merkl data in your front-end, you have 2 types of data you can use:
 - campaigns are programs running on a given pool / ERC20, etc over a certain time
 - opportunities are groups of campaigns targeting the same user base

#### Campaigns

So assuming you're have created a campaign using PYTH targeting the Euler Vault 0x82D2CE1f71cbe391c05E21132811e5172d51A6EE, you can find this campaign's data using [https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH](https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH).

Here, you'll find data related to this campaign start and end date, the amount of PYTH streamed, its id, etc.

Fill free to browse through the available filters at [https://api.merkl.xyz/v4/docs#tag/campaigns/GET/v4/campaigns/](https://api.merkl.xyz/v4/docs#tag/campaigns/GET/v4/campaigns/).

#### Opportunities

Now, you may want to display data about all the campaigns targeting this pool. This way, you'll be able to have the combined APR of all and them, and will be able to display the aggregated data. You could this by getting this opportunity using [https://api.merkl.xyz/v4/opportunities?name=Euler](https://api.merkl.xyz/v4/opportunities?name=Euler).

Here, you'll find aggregated data related to the opportunity like daily rewards, APRs, TVLs, etc.

Fill free to browse through the available filters at [https://api.merkl.xyz/v4/docs#tag/opportunities/GET/v4/opportunities/](https://api.merkl.xyz/v4/docs#tag/opportunities/GET/v4/opportunities/).


#### Rewards

You can find rewards earned by a user through Merkl using [https://api.merkl.xyz/v4/docs#tag/users/GET/v4/users/{address}/rewards](https://api.merkl.xyz/v4/docs#tag/users/GET/v4/users/{address}/rewards). You'll need to specify the address and chainIds.

Here is an example: [https://api.merkl.xyz/v4/users/0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185/rewards?chainId=1,10](https://api.merkl.xyz/v4/users/0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185/rewards?chainId=1,10).


### Benefit from typescript typings when using the API

Interacting safely with Merkl API is possible using our NPM package. See our guide [here](./merkl-api-package.md).

### Claiming user rewards

Rewards are claimable per token: meaning that if you have accumulated rewards of several tokens, you may choose to only claim your rewards of one token type, but you may also choose to claim all your token rewards at once.

The contract on which rewards should be claimed is the `Distributor` contract which address can be found on at this [page](https://app.merkl.xyz/status).

You have two options to claim rewards:

- **Rely on the data provided by this route (recommended):** We build the claim transaction payload and the associated proof, for you. Simply call our API. This is the example shown below.
- **Build the proof yourself:** Built the proof yourself and join it to the transaction data from the API. You can find a Github repository below showing how to do that.

In any case, if a call is made to the correct `Distributor` contract and the `token` or `amount` doesn't match the `proof`, the transaction will revert.

Here is a script you may use to claim all the token rewards for a user on a chain.

```
import type { JsonRpcSigner } from "@ethersproject/providers";
import { MerklApi } from "@merkl/api";
import { Distributor__factory } from "@sdk"; // ABI can be fetched on etherscan

const DISTRIBUTOR_ADDRESS = "0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae";

export const claim = async (chainId: number, signer: JsonRpcSigner) => {
  const { status, data } = await MerklApi("https://api.merkl.xyz")
    .v4.users({ address: signer._address })
    .rewards.get({ query: { chainId: [chainId] } });
  if (status !== 200) throw "Failed to fetch rewards";

  const users = [];
  const tokens = [];
  const amounts = [];
  const proofs = [];

  for (const rewards of data) {
    if (rewards.chain.id !== chainId) continue;
    for (const reward of rewards.rewards) {
      users.push(signer._address);
      tokens.push(reward.token.address);
      amounts.push(reward.amount);
      proofs.push(reward.proofs);
    }
  }

  if (tokens.length === 0) throw "No tokens to claim";

  const contract = Distributor__factory.connect(DISTRIBUTOR_ADDRESS, signer);
  await (await contract.claim(users, tokens, amounts, proofs)).wait();
};

```

If you want to build the proof yourself please reach out to us on Telegram, we'll be glad to help!

---

You're now ready to interact with the Merkl API! 🎉

