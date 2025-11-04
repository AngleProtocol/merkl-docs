---
description: Manage your campaigns once they’re live
---

# Campaign Management

Once your campaign is created, you will be able to see it in the [Opportunities page](https://app.merkl.xyz/). Note that it can take up to one hour to appear in our app due to cache.

You now have the full ownership of your campaign. You’ll be able to:

* Monitor its performance using our API (refer [here](https://docs.merkl.xyz/integrate-merkl/app) for more details) or directly in the studio’s dashboard (coming soon)
* See the campaign leaderboard in the opportunity page to track addresses’ participation
* Integrate campaign data into your own app using our API. You’ll find the procedure [here](https://docs.merkl.xyz/integrate-merkl/app)
* **Request address remapping**: For ongoing campaigns, you can contact the Merkl team to set up automatic address remapping, where rewards from specific addresses are automatically redirected to other addresses throughout the campaign duration. See the [address remapping section](#address-remapping) below.
* **Reallocate unclaimed rewards**: As a campaign creator, you can redirect rewards from any recipient to another address at your discretion. See the [reward reallocation section](#reward-reallocation) below for instructions.
* Cancel a campaign: go in the [studio](https://studio.merkl.xyz/users/) using the creator address, select the campaign you want to cancel and click on the button on the right.
*   (If needed) edit some parameters of your campaigns.

    ✅ You can:

    * Change the start date & end dates if they are not in the past
    * Edit blacklists or whitelists to include or exclude certain addresses
    * Enable additional features as needed

    ❌ You cannot:

    * Change the reward token
    * Change the total reward amount (create a new campaign for that — and shift the start/end date by at least 1 hour to avoid duplication bugs in the Merkl engine)

**Note**: If you want to add extra rewards or add another new reward token to your existing campaign, you’ll need to create another campaign on top of the current one. The new campaign must have a slightly modified start or end date (by a minimum of one hour) to prevent campaign duplication in our engine.

If you need assistance renewing your campaign, please notify us at least 3 days before it ends to ensure a smooth transition.

## Address Remapping

Campaign creators can request **address remapping** from the Merkl team to automatically redirect rewards from a source address to a destination address throughout the campaign duration. The Merkl engine handles this redirection automatically—no manual intervention is required.

**Remapping vs. Forwarding:**

While both mechanisms redirect rewards, they serve different purposes:

- **Forwarding** is an automatic Merkl feature that distributes rewards to users who hold the incentivized asset indirectly (e.g., through staking contracts or LP tokens). Forwarding works at the protocol level and is integrated directly into Merkl's reward distribution logic. See the [Forwarders section](../merkl-mechanisms/features.md#-forwarders) for more details.
- **Address remapping** is a manual configuration set up by the Merkl team that redirects rewards from one specific address to another specific address. It's used when a contract receives rewards but cannot claim them, and you want those rewards sent to a claimable address for the entire campaign duration.

**When to use address remapping:**

- You have a smart contract that receives rewards but cannot claim them, and you want these rewards automatically redirected to a claimable address
- You need ongoing, automatic redirection during a campaign (rather than one-time adjustments)

**How to request address remapping:**

Contact the Merkl team directly with:
- Your campaign ID(s)
- The source address(es) from which rewards should be redirected
- The destination address(es) where rewards should be sent

{% hint style="info" %}
Address remapping is particularly useful for long-running campaigns where you expect regular reward accumulation on addresses that cannot claim. Rather than performing frequent manual reallocations, remapping provides a seamless, automated solution.
{% endhint %}

## Reward Reallocation

As a campaign creator, you can reallocate unclaimed rewards from any recipient address to another address at your discretion. This gives you full control over reward distribution, especially when certain addresses cannot claim their rewards.

{% hint style="info" %}
**Remapping vs. Reallocation**: Address remapping handles ongoing, automatic redirection during a campaign. Reallocation is better suited for one-time or occasional adjustments, especially after a campaign has ended or when you need immediate manual control.
{% endhint %}

**Common use cases:**

- Smart contracts without claim functions that need rewards redirected to claimable addresses
- Lost wallets or inaccessible addresses
- One-time partner facilitation (redirecting from contract addresses to partner wallets)

**How to reallocate rewards:**

Call the `reallocateCampaignRewards` function on the Campaign Creator contract:

```solidity
function reallocateCampaignRewards(
    bytes32 _campaignId,
    address[] memory froms,
    address to
)
```

**Parameters:**
- `_campaignId`: The campaign ID for reallocation
- `froms`: Array of addresses to reallocate from (use `["0x0000000000000000000000000000000000000000"]` to reallocate all unclaimed rewards)
- `to`: The destination address

**Example transaction payload** (for use with Safe Transaction Builder):

When reallocating from the same address across multiple campaigns, create multiple transactions in your Safe batch—one for each campaign ID:

```json
{
  "version": "1.0",
  "chainId": "1",
  "createdAt": 1234567890000,
  "meta": {
    "name": "Campaign Rewards Reallocation",
    "description": "",
    "txBuilderVersion": "1.16.5",
    "checksum": "0x0000000000000000000000000000000000000000000000000000000000000000"
  },
  "transactions": [
    {
      "to": "0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd",
      "value": "0",
      "data": null,
      "contractMethod": {
        "inputs": [
          {
            "internalType": "bytes32",
            "name": "_campaignId",
            "type": "bytes32"
          },
          {
            "internalType": "address[]",
            "name": "froms",
            "type": "address[]"
          },
          {
            "internalType": "address",
            "name": "to",
            "type": "address"
          }
        ],
        "name": "reallocateCampaignRewards",
        "payable": false
      },
      "contractInputsValues": {
        "_campaignId": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "froms": "[\"0x0000000000000000000000000000000000000000\"]",
        "to": "0x0000000000000000000000000000000000000000"
      }
    },
    {
      "to": "0x8BB4C975Ff3c250e0ceEA271728547f3802B36Fd",
      "value": "0",
      "data": null,
      "contractMethod": {
        "inputs": [
          {
            "internalType": "bytes32",
            "name": "_campaignId",
            "type": "bytes32"
          },
          {
            "internalType": "address[]",
            "name": "froms",
            "type": "address[]"
          },
          {
            "internalType": "address",
            "name": "to",
            "type": "address"
          }
        ],
        "name": "reallocateCampaignRewards",
        "payable": false
      },
      "contractInputsValues": {
        "_campaignId": "0x0000000000000000000000000000000000000000000000000000000000000000",
        "froms": "[\"0x0000000000000000000000000000000000000000\"]",
        "to": "0x0000000000000000000000000000000000000000"
      }
    }
  ]
}
```

This example shows how to batch reallocations from the same source address to the same destination address across multiple campaigns. Simply add more transaction objects to the `transactions` array for additional campaigns.

**Important considerations:**

- Reallocation can take up to 24 hours to complete due to security checks and processing
- Reallocation operations are free—no fees are charged
- Each campaign must be reallocated separately (but can be batched in a single Safe transaction)

For more technical details, see the [Campaign Reallocation section](../merkl-mechanisms/features.md#-campaign-reallocation) in the features documentation.

To fetch important data regarding your campaigns, you can use our API to get detailed information. More information [here](https://docs.merkl.xyz/integrate-merkl/app). Here are the most commonly used endpoints:

- Getting the leaderboard for your campaign: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/)
- Getting opportunities and their included campaigns: [https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/campaigns](https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/campaigns)
- Checking rewards amount at user level: [https://api.merkl.xyz/docs#tag/users/GET/v4/users/%7Baddress%7D/rewards](https://api.merkl.xyz/docs#tag/users/GET/v4/users/%7Baddress%7D/rewards)
- Checking rewards amount at campaign level: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total)
- Checking rewards amount at token level: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/)
- Checking how many rewards are unclaimed: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/)
- Integrating APRs data in your front-end: [https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/](https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/)
    - For this route, match the incentivized asset's address with the "explorerAddress" field.

{% hint style="warning" %}
**Test campaigns integration**

You may want to start testing the flow and integrating our data before your point program starts. To read more on test campaigns, please refer to the [Before you start](https://docs.merkl.xyz/distribute-with-merkl/before-you-start#test-campaigns) section.
{% endhint %}

Note: You can find each campaign or opportunity ID directly on the opportunities page [here](https://app.merkl.xyz/).