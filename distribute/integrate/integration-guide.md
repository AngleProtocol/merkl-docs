---
description: Guide to integrate Merkl in your app
---

# 🔌 Integrate Merkl in your App with the Merkl API

You can integrate Merkl data in your app but you don't have to. The [Merkl App](https://merkl.xyz/) will show your users everything they need to use Merkl, they can also claim their tokens from there. This page will guide you through the different routes provided by the Merkl API.

![Merkl front integration diagram](/.gitbook/assets/docs-merkl-front-integration.jpg)

The Merkl API is fully maintained by Angle Labs, it contains all the information needed to integrate Merkl in your frontend.

Some parameters are optional but you should keep in mind that the more parameters you specify, the faster the result will be returned.

## Campaigns information

### /v3/campaigns

Returns all the campaigns on a given chainId

#### Parameters

- chainIds (optional): one or several Merkl supported chain Ids, if none are provided the api will return campaigns for all chains

#### Example requests

**Query all campaigns for all chains**
[https://api.angle.money/v3/campaigns](https://api.angle.money/v3/campaigns)

**Query all campaigns on Ethereum Mainnet**
[https://api.angle.money/v3/campaigns?chainIds=1](https://api.angle.money/v3/campaigns?chainIds=1)

**Query all campaigns on Ethereum Mainnet and Arbitrum**
[https://api.angle.money/v3/campaigns?chainIds=1&chainIds=42161](https://api.angle.money/v3/campaigns?chainIds=1&chainIds=42161)

#### Response

### /v3/campaignsForMainParameter

Returns all the campaigns for a given mainParameter

#### Parameters

- chainId: Merkl supported chain Id
- mainParameter: address of the incentivized asset (pool address for concentrated liquidity campaigns, ERC20 address for ERC20 campaigns etc)

#### Example requests

**Query campaigns on the wstETH/USDT Univ3 pool (`0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA`)**
[https://api.angle.money/v3/campaignsForMainParameter?chainId=1&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA](https://api.angle.money/v3/campaignsForMainParameter?chainId=1&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA)

#### Response

```typescript
{
  campaignId: string;
  campaignType: number;
  rewardToken: string;
  rewardTokenSymbol: string;
  amountDecimal: string;
  startTimestamp: number;
  endTimestamp: number;
}[]
```

### /v3/recipients

Returns all the rewarded addresses of a given campaign

#### Parameters

- chainId: chainId of the campaign
- campaignId: campaignId of the campaign

You can find the campaignId of the campaign by calling the [/v3/campaigns](#v3campaigns) or [v3/campaignsForMainParameter](#v3campaignsformainparameter)

#### Example requests

**Query all campaigns for all chains**
[https://api.angle.money/v3/recipients?chainId=1&campaignId=0xbb9d6e4b3f665d6f73ccab5f83ebcf20325fd517f4e5af1fe864caf6a2ba5563](https://api.angle.money/v3/recipients?chainId=1&campaignId=0xbb9d6e4b3f665d6f73ccab5f83ebcf20325fd517f4e5af1fe864caf6a2ba5563)

#### Response

```typescript
{
    recipient: string;
    reason: string;
    rewardToken: string;
    amount: string;
}[]
```

### User information

### /v3/userRewards

Return all rewards linked to a user on a given chain, data can be filtered by providing parameters.

#### Parameters

- user: address of the user
- chainId: Merkl supported chain Id
- rewardToken (optional): address of the token rewarded to the users
- mainParameter (optional): address of the incentivized asset (pool address for concentrated liquidity campaigns, ERC20 address for ERC20 campaigns etc)
- proofs (optional): defaults to false, wether or not to send proofs for the rewards. Cannot be set to true if mainParameter is specified because the proof could be invalid if the user has unclaimed rewards of the same reward token earned over different mainParameters.

#### Example requests

**Query all rewards for a user on Ethereum Mainnet**
[https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1](https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1)

**Query all ANGLE (`0x31429d1856aD1377A8A0079410B297e1a9e214c2`) rewards earned by a user on Ethereum Mainnet**
[https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&rewardToken=0x31429d1856aD1377A8A0079410B297e1a9e214c2](https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&rewardToken=0x31429d1856aD1377A8A0079410B297e1a9e214c2)

**Query all rewards earned on the wstETH/USDT Univ3 pool (`0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA`) by a user on Ethereum Mainnet**
[https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA](https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&mainParameter=0xeC5055067d60292Ab2c514A1090Bc8E014e4aBAA)

**Query all rewards earned by a user to build a claim transaction on Ethereum Mainnet**
[https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&proof=true](https://api.angle.money/v3/userRewards?user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185&chainId=1&proof=true)

#### Response

```typescript
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

To submit a [claim transaction](https://etherscan.io/address/0x3Ef3D8bA38EBe18DB133cEc108f4D14CE00Dd9Ae#writeProxyContract#F1) you should map the data returned by this route as follows:

- Users: `["<Address used for the request>"]`
- Tokens: `["<Address of the token you want to claim>"]`
- Amounts: `[data["<chain on which you want to claim>"].tokenData["<Address of the token you want to claim>"].unlclaimed]`
- Proofs: `[data["<chain on which you want to claim>"].tokenData["<Address of the token you want to claim>"].proofs]`

{% hint style="info" %}
You can get the Merkl Distributor contract abi [here](https://etherscan.io/address/0x22b0ac22d5d58f05873e470bca5db7ceb5c47f5e#code) (same ABI on all chains)
{% endhint %}

#### Claiming user rewards

Rewards are claimable per token: meaning that if you have accumulated rewards of several tokens, you may choose to only claim your rewards of one token type, but you may also choose to claim all your token rewards at once.

The contract on which rewards should be claimed is the `Distributor` contract which address can be found on [this doc](../../addresses.md), or on the [Angle SDK](https://github.com/AngleProtocol/sdk).

You have two options to do that:

- Rely on the data provided by this route (recommended): we build the claim transaction payload for you and the associated proof, and you just call our API. This is the example shown below.
- Build the proof yourself and join it to the transaction data from the API. You can find a Github repository below showing how to do that.

{% hint style="info" %}
In any case, if a call is made to the correct `Distributor` contract and the `token` or `amount` doesn't match the `proof`, the transaction will revert.
{% endhint %}

Here is a script you may use to claim all the token rewards for a user on a chain.

```typescript
import {
  Distributor__factory,
  MerklAPIData,
  registry,
} from '@angleprotocol/sdk'
import { JsonRpcSigner } from '@ethersproject/providers'
import axios from 'axios'

export const claim = async (chainId: number, signer: JsonRpcSigner) => {
  let data: MerklAPIData['transactionData']
  try {
    data = (
      await axios.get(
        `https://api.angle.money/v3/userRewards?chainId=${chainId}&user=${signer._address}&proof=true`,
        {
          timeout: 5000,
        },
      )
    ).data
  } catch {
    throw 'Angle API not responding'
  }
  const tokens = Object.keys(data).filter((k) => (data[k].proof !== undefined || data[k].proof !== []))
  const claims = tokens.map((t) => data[t].unclaimed)
  const proofs = tokens.map((t) => data[t].proof)

  if (tokens.length === 0) throw 'No tokens to claim'

  const contractAddress = registry(chainId)?.Merkl?.Distributor
  if (!contractAddress) throw 'Chain not supported'
  const contract = Distributor__factory.connect(contractAddress, signer)
  await (
    await contract.claim(
      tokens.map((t) => signer._address),
      tokens,
      claims,
      proofs as string[][],
    )
  ).wait()
}
```

{% hint style="info" %}
If you want to build the proof yourself please reach out to us on Telegram, we'll be glad to help!
{% endhint %}

### /v3/rewards

This route was made for the Angle Labs Merkl frontend, to get user rewards we recommend using /v3/userRewards as the route is faster and returns more specific information.

Returns all the rewards accumulated by a user, the merkl proofs needed for the claim transaction and the campaign information.

#### Parameters

- user: address of the user
- chainIds (optional): one or several Merkl supported chain Ids, if none are provided the api will return rewards for all chains

#### Response

```typescript
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

## All data

### /v2/merkl
Data about all Merkl incentivized assets across all supported chains, contains all Merkl data but is a bit slower than other endpoints. This route is subject to change in the near future.

#### Parameters

- chainIds: one or several Merkl supported chain Ids, if none are provided the api will return rewards for all chains
- user: address of the user

#### Example queries

**Fetch all Merkl data for a user on Optimism**
[`https://api.angle.money/v2/merkl?chainIds[]=10&user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185`](https://api.angle.money/v2/merkl?chainIds[]=10&user=0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185).

## Listing incentivized pools

When called for a specific chain, the API returns in a `pools` object a mapping between pool addresses of this chain (like Uniswap V3 pool addresses) and details on the APRs for providing liquidity.

Note that because how concentrated liquidity AMMs and the way the script works, two LPs with different positions may earn drastically different returns. As such, APRs displayed here are average measures of the returns LPs can earn. If you are to integrate Merkl in your app, you may want to build the APR indicators tailored to your use cases.

The `pools` object also has details about the past distributions for each incentivized pool: their start and end dates, the parameters set and the amount of incentives for each distribution.

You can filter from the `pools` object to only display the pools of your choice (for instance pools with tokens in a subset and with an active distribution).

## Displaying user rewards

The `rewardsPerToken` object lets you get how much rewards of a specific token can be claimed by a user for a specific pool.

This object maps each reward token to the total amount of rewards accumulated on the pool for a user. It includes the rewards that have already been claimed as well as to the amount of unclaimed tokens on the pool. It also details where these tokens have been earned (Uniswap V3, Gamma, ...).
