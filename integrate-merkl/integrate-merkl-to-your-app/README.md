---
description: Guide to integrate Merkl in your app
---

# Integrate Merkl to your App

**You can integrate Merkl data in your app, but you don't have to**. The [Merkl App](https://app.merkl.xyz/) will show your users everything they need to use Merkl, and they can also claim their tokens directly from there. This page will guide you through the different routes provided by the Merkl API.

<figure><img src="https://docs.merkl.xyz/~gitbook/image?url=https%3A%2F%2F3295124503-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FKRQdTiHhBGLRKeCCOqdc%252Fuploads%252Fgit-blob-76648012287e5c388aa2eb6608436bcf50779576%252Fdocs-merkl-front-integration.jpg%3Falt%3Dmedia&#x26;width=768&#x26;dpr=4&#x26;quality=100&#x26;sign=eba021123df16a566eb133fc8bdb2f9fc2b5d0503b1c8e2cede760790b15dbd7" alt=""><figcaption><p>Merkl front integration diagram</p></figcaption></figure>

**The Merkl API** is fully maintained by Angle Labs, and **contains all the information needed to integrate Merkl into your frontend. Some parameters are optional, but you should kep in mind that specifying more parameters can speed up the result.**

## V3 Campaigns APIs

### /V3/campaigns <a href="#campaigns-information" id="campaigns-information"></a>

Returns all the campaigns on a given `chainId`.

**Parameters**

* `chainId` (optional): You can specify one or several Merkl supported chain Ids. If none are provided, the API will return campaigns for all chains by default.&#x20;

**Example requests**

* **Query all V3 campaigns for all chains.** [https://api.merkl.xyz/v3/campaigns](https://api.merkl.xyz/v3/campaigns)
* **Query all V3 campaigns on Ethereum Mainnet (**`chainId=1`). [https://api.merkl.xyz/v3/campaigns?chainIds=1](https://api.merkl.xyz/v3/campaigns?chainIds=1)
* **Query all V3 campaigns on Ethereum Mainnet (**`chainId=1`) **and Arbitrum** **(**`chainId=42161`). [https://api.merkl.xyz/v3/campaigns?chainIds=1\&chainIds=42161](https://api.merkl.xyz/v3/campaigns?chainIds=1\&chainIds=42161)

### /v3/campaignsForMainParameter

Returns all the campaigns for a given `mainParameter`.

**Parameters**

* `chainId`: Merkl supported chain Id. In this instance, `chainId` is mandatory.
* `mainParameter`: Address of the incentivized asset (pool address for concentrated liquidity campaigns, ERC20 address for ERC20 campaigns, etc.).

**Example requests**

* **Query campaigns on the wstETH/USDT Univ3 pool (**`mainParameter=`**`0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA`) on Ethereum  Mainnet (**`chainId=1`). [https://api.merkl.xyz/v3/campaignsForMainParameter?chainId=1\&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA](https://api.merkl.xyz/v3/campaignsForMainParameter?chainId=1\&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA)

**Response Template**

```
{
  campaignId: string;
  campaignType: number;
  rewardToken: string;
  rewardTokenSymbol: string;
  amountDecimal: string;
  startTimestamp: number;
  endTimestamp: number;
}
[];
```

### /v3/recipients

Returns all the rewarded addresses of a given campaign.

**Parameters**

* `chainId`: Merkl supported chain Id. In this instance, `chainId` is mandatory.
* `campaignId`: Campaign Id of the campaign.

You can find the campaignId of the campaign by calling the [/v3/campaigns](./#campaigns-information-1) or [/v3/campaignsForMainParameter](./#v3-campaignsformainparameter)

**Example requests**

* **Query the campaign on Arbitrum (chain** `chainId=42161`**) with** `campaignId=`0x2bba7ce636dcd4ddd2ea70f790729cdc87510327074aa3f5df8a3aeb3f54f7d0 [https://api.merkl.xyz/v3/recipients?chainId=42161\&campaignId=0x2bba7ce636dcd4ddd2ea70f790729cdc87510327074aa3f5df8a3aeb3f54f7d0](https://api.merkl.xyz/v3/recipients?chainId=42161\&campaignId=0x2bba7ce636dcd4ddd2ea70f790729cdc87510327074aa3f5df8a3aeb3f54f7d0)

**Response**

```
{
  recipient: string;
  reason: string;
  rewardToken: string;
  amount: string;
}
[];
```

### /v3/userRewards

Returns all rewards linked to a user on a given chain. Data can be filtered by providing parameters.

**Parameters**

* `user`: Address of the user.
* `chainId`: Merkl supported chain Id. In this instance, `chainId` is mandatory.
* `rewardToken` (optional): Address of the token rewarded to the users.
* `mainParameter` (optional): Address of the incentivized asset (pool address for concentrated liquidity campaigns, ERC20 address for ERC20 campaigns, etc.)
* `proof` (optional): Defaults to false. Allows you to choose whether or not to include proof of rewards. This cannot be set to true if `mainParameter` is specified, as the proof could be invalid if the user has unclaimed rewards of the same reward token earned over different `mainParameters`.

**Example requests**

* **Query all rewards eanred on Ethereum (chain** `chainId=1`**) by a user (**`user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185`**).**  [https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1](https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1)
* **Query all ANGLE (**`rewardToken=0x31429d1856aD1377A8A0079410B297e1a9e214c2`**) rewards earned on Ethereum Mainnet (chain** `chainId=1`**) by a user (**`user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185`**).** [https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&rewardToken=0x31429d1856aD1377A8A0079410B297e1a9e214c2](https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&rewardToken=0x31429d1856aD1377A8A0079410B297e1a9e214c2)
* **Query all rewards earned on the wstETH/USDT Univ3 pool (**`mainParameter=`**`0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA`) on Ethereum Mainnet (chain** `chainId=1`**) by a user (**`user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185`**).** [https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA](https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA)
* **Query all rewards earned by a user (**`user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185`**) to build a claim transaction (**`proof=true` **) on Ethereum Mainnet (**`chainId=1`)  [https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&proof=true](https://api.merkl.xyz/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185\&chainId=1\&proof=true)

**Response (orifginal)**

```
{
    [rewardTokenAddress: string]: {
        accumulated: string;
        unclaimed: string;
        decimals: number;
        symbol: string;
        reasons: {
          [reason: string]: {
            accumulated: string;
            unclaimed: string;
          }
        }
        proof?: string[]
    }
}
```

**Response (for me this is the new structure, need to be checked)**

```
{
    [rewardTokenAddress: string]: {
        accumulated: string;
        decimals: number;
        symbol: string;
        reasons: {
          [reason: string]: {
            accumulated: string;
            unclaimed: string;
          }
        }
        symbol: string;
        unclaimed: string;
        proof?: string[]
    }
}
```

To submit a [claim transaction](https://etherscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae#writeProxyContract#F1) you should map the data returned by this route as follows:

* Users: `["<Address used for the request>"]`
* Tokens: `["<Address of the token you want to claim>"]`
* Amounts: `[data["<chain on which you want to claim>"].tokenData["<Address of the token you want to claim>"].accumulated]`
* Proofs: `[data["<chain on which you want to claim>"].tokenData["<Address of the token you want to claim>"].proofs]`

You can get the Merkl Distributor contract abi [here](https://etherscan.io/address/0x22b0ac22d5d58f05873e470bca5db7ceb5c47f5e#code) (same ABI on all chains)

**Claiming user rewards**

Rewards are claimable per token: meaning that if you have accumulated rewards of several tokens, you may choose to only claim your rewards of one token type, but you may also choose to claim all your token rewards at once.

The contract on which rewards should be claimed is the `Distributor` contract which address can be found on at this [page](../../general-information/smart-contract-address.md) or on the [Angle SDK](https://github.com/AngleProtocol/sdk).

You have two options to claim rewards:

* **Rely on the data provided by this route (recommended):** We build the claim transaction payload and the associated proof, for you. Simply call our API. This is the example shown below.
* **Build the proof yourself:**  Built the proof yourself and join it to the transaction data from the API. You can find a Github repository below showing how to do that.

In any case, if a call is made to the correct `Distributor` contract and the `token` or `amount` doesn't match the `proof`, the transaction will revert.

Here is a script you may use to claim all the token rewards for a user on a chain.

```
import {
  Distributor__factory,
  MerklAPIData,
  registry,
} from "@angleprotocol/sdk";
import { JsonRpcSigner } from "@ethersproject/providers";
import axios from "axios";

export const claim = async (chainId: number, signer: JsonRpcSigner) => {
  let data: MerklAPIData["transactionData"];
  try {
    data = (
      await axios.get(
        `https://api.merkl.xyz/v3/userRewards?chainId=${chainId}&user=${signer._address}&proof=true`,
        {
          timeout: 5000,
        }
      )
    ).data;
  } catch {
    throw "Angle API not responding";
  }
  const tokens = Object.keys(data).filter(
    (k) => data[k].proof !== undefined || data[k].proof !== []
  );
  const claims = tokens.map((t) => data[t].accumulated);
  const proofs = tokens.map((t) => data[t].proof);

  if (tokens.length === 0) throw "No tokens to claim";

  const contractAddress = registry(chainId)?.Merkl?.Distributor;
  if (!contractAddress) throw "Chain not supported";
  const contract = Distributor__factory.connect(contractAddress, signer);
  await (
    await contract.claim(
      tokens.map((t) => signer._address),
      tokens,
      claims,
      proofs as string[][]
    )
  ).wait();
};
```

If you want to build the proof yourself please reach out to us on Telegram, we'll be glad to help!

### /v3/rewards

This route was made for the Angle Labs Merkl frontend. To get user rewards, we recommend using `/v3/userRewards` as it is faster and returns more specific information.

`/v3/rewards`returns all the rewards accumulated by a user, the merkl proofs needed for the claim transaction, and the campaign information.

**Parameters**

* `user`: Address of the user.
* `chainIds` (optional): One or several Merkl supported chain Ids. If none are provided, the API will return rewards for all chains.

**Response**

Copy

```
{
  [chainId: number]: {
    tokenData: {
      [token: string]: {
        accumulated: string;
        unclaimed: string;
        decimals: number;
        symbol: string;
        proof: string[];
      };
    };
    campaignData: {
      [campaignId: string]: {
        [reason: string]: {
          accumulated: string;
          unclaimed: string;
          token: string;
          decimals: number;
          symbol: string;
          mainParameter: string;
          auxiliaryData1: string;
          auxiliaryData2: string;
        };
      };
    };
  };
}
```

### Listing incentivized pools <a href="#listing-incentivized-pools" id="listing-incentivized-pools"></a>

When called for a specific chain, the API returns in a `pools` object a mapping between pool addresses of this chain (like Uniswap V3 pool addresses) and details on the APRs for providing liquidity.

**Note:** Due to the nature of concentrated liquidity AMMs and the script's workings, two LPs with different positions may earn drastically different returns. Therefore, APRs displayed here are average measures of the returns LPs can earn. If you integrate Merkl into your app, you may want to build APR indicators tailored to your use cases.

The `pools` object also includes details about past distributions for each incentivized pool, such as start and end dates, parameters set, and the amount of incentives for each distribution. You can filter the `pools` object to display only the pools of your choice (e.g., pools with tokens in a subset and with an active distribution).

### Displaying user rewards <a href="#displaying-user-rewards" id="displaying-user-rewards"></a>

The `rewardsPerToken` object allows you to see how many rewards of a specific token can be claimed by a user for a specific pool. This object maps each reward token to the total amount of rewards accumulated on the pool for a user. It includes both the rewards that have already been claimed and the amount of unclaimed tokens on the pool. It also details where these tokens have been earned (e.g., Uniswap V3, Gamma).
