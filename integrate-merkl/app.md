---
description: Integrate Merkl in your app using Merkl API V4
---

# Integrate Merkl API

Merkl provides an API, available [here](https://api.merkl.xyz/docs), that serves all the data shown in the Merkl app. You can use it to track campaigns, display APRs, and integrate user rewards directly into your frontend as a white-label solution.

This guide will walk you through the key integration patterns and best practices for using the Merkl API. We encourage you to reference the full API documentation for detailed endpoint specifications.

{% hint style="info" %}
If you use Merkl as a white-label solution in your frontend, you must integrate [our logo](branding-and-integration.md) with a clickable link that redirects to our app.
{% endhint %}

{% hint style="info" %}
The Merkl API provides two types of data for displaying incentive information: **campaigns** and **opportunities**. Before integrating the API, it's important to understand [the difference between a campaign and an opportunity](../merkl-mechanisms/technical-overview.md#opportunity-vs-campaign) within Merkl.
{% endhint %}

## Finding Relevant Opportunities

The opportunities endpoint provides key metrics like APR, TVL, and daily rewards that you can display in your frontend.

Multiple filters are available to query opportunities. For a complete list, see the [opportunities endpoint documentation](https://api.merkl.xyz/docs#tag/opportunities/GET/v4/opportunities/).

**Recommended filters:**

**By Protocol ID** - Find all opportunities for a specific protocol:

```
https://api.merkl.xyz/v4/opportunities?mainProtocolId={protocol_id}
```

Example for Euler: [`https://api.merkl.xyz/v4/opportunities?mainProtocolId=euler`](https://api.merkl.xyz/v4/opportunities?mainProtocolId=euler)

To find your protocol's ID, check existing opportunities on the API or try filtering by name first (`https://api.merkl.xyz/v4/opportunities?name={name}`)

**By Explorer Address** - Find opportunities for a specific pool or lending market:

```
https://api.merkl.xyz/v4/opportunities?explorerAddress={address}
```

Example for the Aave USDT0 market on Plasma: [`https://api.merkl.xyz/v4/opportunities?explorerAddress=0x5D72a9d9A9510Cd8cBdBA12aC62593A58930a948`](https://api.merkl.xyz/v4/opportunities?explorerAddress=0x5D72a9d9A9510Cd8cBdBA12aC62593A58930a948)

**By Chain ID** - Find all opportunities on a specific chain:

```
https://api.merkl.xyz/v4/opportunities?chainId={chain_id}
```

Example for Ethereum: [`https://api.merkl.xyz/v4/opportunities?chainId=1`](https://api.merkl.xyz/v4/opportunities?chainId=1)

**By Tags** - Find opportunities across multiple protocols and chains:

```
https://api.merkl.xyz/v4/opportunities?tags={tag}
```

Example for zkSync Ignite Program: [`https://api.merkl.xyz/v4/opportunities?tags=zksync`](https://api.merkl.xyz/v4/opportunities?tags=zksync)

If you need a custom tag for your opportunities, contact us and we'll assign it to your campaigns.

{% hint style="info" %}
Each opportunity has a permanent unique ID that remains unchanged even when multiple successive campaigns are created on the same pool or asset.
{% endhint %}

## Finding Relevant Campaigns

While the opportunities endpoint provides high-level metrics about what's being incentivized, the campaigns endpoint offers detailed campaign rules (duration, budget, campaign type, customization options, etc.). Use this endpoint to display in-depth campaign information to your users.

For a complete list of available filters, see the [campaigns endpoint documentation](https://api.merkl.xyz/docs#tag/campaigns/GET/v4/campaigns/).

**Recommended filters:**

**By Creator Address** - Find all campaigns created by a specific address:

```
https://api.merkl.xyz/v4/campaigns?creatorAddress={address}
```

Example: [`https://api.merkl.xyz/v4/campaigns?creatorAddress=0xdef1FA4CEfe67365ba046a7C630D6B885298E210`](https://api.merkl.xyz/v4/campaigns?creatorAddress=0xdef1FA4CEfe67365ba046a7C630D6B885298E210)

**By Token Symbol** - Find all campaigns distributing a specific reward token:

```
https://api.merkl.xyz/v4/campaigns?tokenSymbol={symbol}
```

Example for campaigns distributing $PYTH: [`https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH`](https://api.merkl.xyz/v4/campaigns?tokenSymbol=PYTH)

## Retrieving Both Campaign and Opportunity Data

The `campaigns` endpoint provides campaign-specific information but not upstream opportunity data. Similarly, querying for an opportunity doesn't include downstream campaign details. To retrieve all information in a single request:

**Search campaigns with related opportunities** - Use `/v4/campaigns` with `withOpportunity=true`:

```
https://api.merkl.xyz/v4/campaigns?withOpportunity=true
```

**Search opportunities with related campaigns** - Use `/v4/opportunities` with `campaigns=true`:

```
https://api.merkl.xyz/v4/opportunities?campaigns=true
```

{% hint style="info" %}
Learn more about [how TVLs, APRs, and daily rewards are computed](../merkl-mechanisms/technical-overview.md#main-metrics) and why they may differ between the campaign and opportunity levels.
{% endhint %}

### About Campaign IDs

Merkl uses two distinct identifier types for campaigns:

* **`campaignId`** (format: `0x...`): The onchain identifier of a campaign. Note that this is **not unique** across chains—multiple campaigns on different chains may share the same `campaignId`. You can find this in the [opportunities page](https://app.merkl.xyz/) by selecting an opportunity, viewing campaign details, and navigating to the "Advanced" tab.
* **`id`** (format: numeric, e.g., `13972358188887408622`): The unique database identifier used for most API routes. You can retrieve this by querying the [campaigns endpoint](https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/) with a `campaignId`. You may also find a campaign's database ID under the advanced tab of a campaign on the Merkl app.

<figure><img src="../.gitbook/assets/API-V4.png" alt=""><figcaption></figcaption></figure>

## Fetching Campaign and User Statistics

The Merkl API provides endpoints to retrieve user leaderboards and reward statistics for individual campaigns or across multiple campaigns distributing the same token.

**Recommended endpoints:**

* **Campaign leaderboard**: Get all users rewarded in a campaign - [`https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/`](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/)
* **Token-level rewards**: Check rewards across all campaigns for a token - [`https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/`](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/)
* **Campaign-level rewards**: Check total amount of rewards distributed in a specific campaign - [`https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total`](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total)
* **Unclaimed rewards**: Check unclaimed amounts for a campaign - [`https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/`](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/)
* **Historical metrics**: Get TVL and APR history for a campaign - [`https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/{id}/metrics`](https://api.merkl.xyz/docs#tag/campaigns/get/v4/campaigns/{id}/metrics)

<details>

<summary>Example Python script for fetching campaign metrics (unclaimed rewards, number of wallets and evolution of APR/TVL/Daily Rewards)</summary>

```python
import datetime
import requests

def timestamp_to_date(timestamp: int) -> str:
    dt = datetime.datetime.fromtimestamp(timestamp)
    return dt.strftime('%d/%m/%Y')

def get_unclaimed_rewards(campaign_id: int):
    campaign_info_url = f"https://api.merkl.xyz/v4/campaigns/{campaign_id}"
    campaign_response = requests.get(campaign_info_url)
    
    campaign_onchain_id = campaign_response.json().get("campaignId")
    distribution_chain_id = campaign_response.json().get("distributionChainId")
    reward_token_symbol = campaign_response.json().get("rewardToken", {}).get("symbol", "UNKNOWN")
    decimals = campaign_response.json().get("rewardToken", {}).get("decimals", 18)
    total_budget = int(campaign_response.json().get("amount", 0))/(10 ** decimals)

    unclaimed_url = f"https://api.merkl.xyz/v4/rewards/unclaim?chainId={distribution_chain_id}&campaignIds={campaign_onchain_id}"

    response_distributed = requests.get(f"https://api.merkl.xyz/v4/rewards/total?chainId={distribution_chain_id}&campaignId={campaign_onchain_id}")
    total_distributed = int(response_distributed.json().get("amount"))/(10 ** decimals)
    response = requests.get(unclaimed_url)
    unclaimed = int(response.json().get(campaign_onchain_id))/(10 ** decimals)
    
    print(f"Total budget: {total_budget:,.2f} {reward_token_symbol}")
    print(f"Distributed: {total_distributed:,.2f} {reward_token_symbol} ")
    print(f"Claimed: {total_distributed - unclaimed:,.2f} {reward_token_symbol}")
    print(f"Unclaimed: {unclaimed:,.2f} {reward_token_symbol}")

def get_campaign_metrics(campaignId: int):
    url = f"https://api.merkl.xyz/v4/campaigns/{campaignId}/metrics"

    response = requests.get(url)
    data = response.json()
    if response.status_code != 200:
        print(f"Failed to fetch metrics for campaign {campaignId}. Status code: {response.status_code}")
        return
    
    
    tvlsRecords = data["tvlRecords"]
    aprRecords = data["aprRecords"]
    dailyRewardsRecords = data["dailyRewardsRecords"]
    walletCountRecords = data["walletCount"]
    # Find the min and max wallet count
    min_wallets = min([item["walletCount"] for item in walletCountRecords])
    max_wallets = max([item["walletCount"] for item in walletCountRecords])
    
    print(f"Campaign {campaignId} metrics:")
    print("")
    get_unclaimed_rewards(campaignId)
    print("")
    print(f"Min wallets: {min_wallets}")
    print(f"Max wallets: {max_wallets}")
    print("")
    
    results = {}
    
    for item in tvlsRecords:
        if item["timestamp"] not in results:
            results[item["timestamp"]] = {}
        results[item["timestamp"]]["TVL"] = int(item["total"])
    for item in aprRecords:
        if item["timestamp"] not in results:
            results[item["timestamp"]] = {}
        results[item["timestamp"]]["APR"] = float(item["apr"])
    for item in dailyRewardsRecords:
        if item["timestamp"] not in results:
            results[item["timestamp"]] = {}
        results[item["timestamp"]]["Daily Rewards"] = float(item["total"])
    # Find the closest wallet count record for each timestamp and add it to results
    for timestamp in results.keys():
        closest_record = min(
            walletCountRecords,
            key=lambda record: abs(int(timestamp) - int(record.get('timestamp', 0)))
        )
        results[timestamp]["Wallets"] = closest_record.get("walletCount", 0)

    # Print the results in a table format
    print(f"{'Date':<20} {'TVL ($)':<20} {'APR (%)':<15} {'Daily Rewards ($)':<20} {'Wallets':<10}")
    for timestamp in sorted(results.keys()):
        date_str = timestamp_to_date(int(timestamp))
        tvl = results[timestamp].get("TVL", 0)
        apr = results[timestamp].get("APR", 0)  # Convert to percentage
        daily_rewards = results[timestamp].get("Daily Rewards", 0)
        wallets = results[timestamp].get("Wallets", 0)
        # Print TVL in human readable format
        tvl = f"{tvl:,.0f}"
        daily_rewards = f"{daily_rewards:,.2f}"
        print(f"{date_str:<20} {tvl:<20} {apr:<15.2f} {daily_rewards:<20} {wallets:<10}")
        
if __name__ == "__main__":
    # Example usage
    campaign_id = 12345  # Replace with your campaign ID
    get_campaign_metrics(campaign_id)
```

</details>

## Integrating User Rewards

To retrieve reward data for a specific user, use the following endpoint:

```
https://api.merkl.xyz/v4/users/{address}/rewards?chainId={chain_id}
```

Example - Checking a user's rewards on zkSync: [`https://api.merkl.xyz/v4/users/0x4F2BF7469Bc38d1aE779b1F4affC588f35E60973/rewards?chainId=324`](https://api.merkl.xyz/v4/users/0x4F2BF7469Bc38d1aE779b1F4affC588f35E60973/rewards?chainId=324)

**This endpoint returns:**

* **`amount`**: Total tokens credited to the user onchain
* **`pending`**: Pending rewards that will be credited on the next Merkle root update
* **`claimed`**: Tokens already claimed by the user
* **`proofs`**: Cryptographic proofs needed for claiming rewards (detailed in the next section)
* **`breakdowns`**: Campaign attribution for earned rewards (may be incomplete and not show all campaigns)

{% hint style="info" %}
The claimable amount equals `amount - claimed`.
{% endhint %}

{% hint style="warning" %}
`pending` rewards update more frequently (about every 2 hours) than the `amount`. Integrating `pending` rewards in your app lets you show more timely updates to your users. However, always display pending rewards separately to avoid confusion because `pending` rewards are not claimable. Also note that whenever tokens are credited onchain, the `pending` rewards reset to zero because they are added to the `amount`.
{% endhint %}

**Important notes about this endpoint:**

* **Historical data**: The API doesn't return time-series data. To build historical datasets, take daily snapshots of the API responses.
* **Caching behavior**: This route is cached. If called immediately after a user claims rewards, the data may be stale. Use the `reloadChainId` parameter to force a cache refresh:

    ```
    https://api.merkl.xyz/v4/users/{address}/rewards?chainId=324&reloadChainId=324
    ```

**Building claim transactions:**

Rewards are claimed through the `Distributor` contract. Find contract addresses for each chain on the [Merkl status page](https://app.merkl.xyz/status).

{% hint style="info" %}
If the `token` or `amount` doesn't match the `proof` when calling the `Distributor` contract, the transaction will revert.
{% endhint %}

Rewards on Merkl are claimable per token. Users can claim rewards for a single token or all tokens at once.

**Example claiming script:**

Here is a script to claim all token rewards for a user on a chain:

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

## Additional Tips

**Pagination** - All Merkl API endpoints are paginated. Iterate through pages or increase the page size parameter to retrieve more results (using `&page=<NUMBER>`)

**Query optimization** - While many API parameters are optional, specifying additional filters can significantly improve query performance.

**Test campaigns** - To retrieve data for test campaigns (using tokens like `aglaMerkl`), include the `&test=true` parameter in your requests. Learn more in the [Before you start guide](https://docs.merkl.xyz/distribute-with-merkl/before-you-start#test-campaigns).

## Benefit from TypeScript Typings

For type-safe API interactions, use the [@merkl/api NPM package](https://www.npmjs.com/package/@merkl/api?activeTab=readme).
