---
description: Manage your campaigns once they’re live
---

# Campaign Management

Once your campaign is created, you will be able to see it in the [Opportunities page](https://app.merkl.xyz/). Note that it can take up to one hour to appear in our app due to cache.

You now have the full ownership of your campaign. You’ll be able to:

* Monitor its performance using our API (refer [here](https://docs.merkl.xyz/integrate-merkl/app) for more details) or directly in the studio’s dashboard (coming soon)
* See the campaign leaderboard in the opportunity page to track addresses’ participation
* Integrate campaign data into your own app using our API. You’ll find the procedure [here](https://docs.merkl.xyz/integrate-merkl/app)
* **Request address remapping**: For ongoing campaigns, you can contact the Merkl team to set up automatic address remapping, where rewards from specific addresses are automatically redirected to other addresses throughout the campaign duration. See the [address remapping section](#address-remapping-alternative-to-manual-reallocation) below.
* **Reallocate unclaimed rewards**: As a campaign creator, you have full control over reward reallocation. You can redirect rewards from any recipient to another address at your discretion. This is particularly useful when smart contracts receive pending rewards but lack a claim function, or when forwarding is not yet implemented. See the detailed section on [reward reallocation](#reward-reallocation) below for complete instructions.
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

## Address Remapping (Alternative to Manual Reallocation)

Instead of performing regular manual reallocations or waiting until the end of an incentive program to reallocate all rewards, campaign creators can request **address remapping** directly from the Merkl team.

When you request address remapping, the Merkl engine will automatically redirect rewards from a source address (e.g., a smart contract A) to a destination address (e.g., address B) throughout the campaign duration. This is handled automatically by Merkl's infrastructure—you don't need to perform manual reallocations.

**When to use address remapping:**

- You have a smart contract that receives rewards but cannot claim them, and you want these rewards automatically redirected to a claimable address for the entire campaign duration
- You need to redirect rewards from multiple addresses on a recurring basis, making manual reallocations impractical
- You want a proactive solution that handles reward redirection automatically rather than requiring periodic manual intervention

**How to request address remapping:**

Contact the Merkl team directly to request address remapping for your campaign. Provide:
- Your campaign ID(s)
- The source address(es) from which rewards should be redirected
- The destination address(es) where rewards should be sent

The Merkl team will configure the remapping, and the Merkl engine will handle the automatic redirection for the duration of your campaign(s).

{% hint style="info" %}
Address remapping is particularly useful for long-running campaigns where you expect regular reward accumulation on addresses that cannot claim. Rather than waiting until campaign end or performing frequent manual reallocations, remapping provides a seamless, automated solution.
{% endhint %}

## Reward Reallocation

As a campaign creator, you have the power to reallocate unclaimed rewards from any recipient address to another address at your discretion. This is a powerful feature that gives you full control over reward distribution, especially in scenarios where automatic forwarding is not yet implemented or when certain addresses cannot claim their rewards.

{% hint style="info" %}
**Remapping vs. Reallocation**: If you need ongoing, automatic redirection of rewards during a campaign, consider [address remapping](#address-remapping-alternative-to-manual-reallocation) instead. Reallocation is better suited for one-time or occasional adjustments, especially after a campaign has ended or when you need immediate manual control.
{% endhint %}

### When to Use Reward Reallocation

Reward reallocation is particularly useful in the following situations:

**Smart contracts without claim functions**: Some smart contracts may receive pending rewards but lack a `claim()` function to retrieve them. In such cases, these rewards remain unclaimable. By reallocating them to a wallet address that can claim (such as an EOA, Safe, or other contract with claim functionality), you enable the partner or stakeholder to easily claim their rewards.

**Forwarding not yet implemented**: When forwarding is not yet available, some contracts may accumulate rewards that need to be manually redirected to appropriate addresses.

**Lost wallets or inaccessible addresses**: If rewards are allocated to addresses that are no longer accessible, you can reallocate them to active addresses.

**Partner facilitation**: Reallocate rewards from a contract address to a partner's wallet address (EOA, Safe, etc.) to simplify the claiming process and improve user experience.

### How to Reallocate Rewards

You can reallocate rewards in two ways:

#### Via the Studio (Recommended for single campaign reallocations)

The easiest way to reallocate rewards is through the [Merkl Studio](https://studio.merkl.xyz/users/):

1. Connect to the studio using your creator address
2. Navigate to your opportunity's management page
3. Locate the button with three dots next to the "Renew" button
4. Click on it and select "Reallocate unclaimed funds"

The reallocation interface will guide you through the process:
- **Reallocate rewards from**: Enter the address of the participant you want to reallocate from
- The system will display the unclaimed rewards amount for that address
- **Enter a wallet address to reallocate rewards to**: Enter the destination address (a wallet that can claim, such as an EOA, Safe, or other contract with claim functionality)

{% hint style="warning" %}
**Important**: Reallocation is done per campaign ID. If your opportunity contains multiple campaigns that need reallocation, you must repeat this process for each campaign separately when using the Studio interface. For batch reallocations across multiple campaigns, see the [programmatic approach](#programmatically-for-advanced-users-and-batch-reallocations) below using Safe multisig.
{% endhint %}

#### Programmatically (For advanced users and batch reallocations)

If you need to reallocate rewards from multiple campaigns (when an opportunity contains several campaigns) or prefer a programmatic approach, you can call the `reallocateCampaignRewards` function directly on the Campaign Creator contract. This is particularly useful when batching multiple reallocations into a single transaction using a Safe multisig.

**Function signature:**
```solidity
function reallocateCampaignRewards(
    bytes32 _campaignId,
    address[] memory froms,
    address to
)
```

**Parameters:**
- `_campaignId`: The campaign ID you want to reallocate unclaimed rewards for
- `froms`: An array of addresses that you want to reallocate from
- `to`: The address that should receive the reallocated rewards

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

To reallocate all unclaimed rewards from a campaign at once, set `froms` to `["0x0000000000000000000000000000000000000000"]`.

### Important Considerations

**Reallocation timing**: When you trigger a reallocation, the process can take up to 24 hours to complete due to required security checks and the processing needed to update reward distributions. This applies whether you're:
- Reallocating all unclaimed rewards at the end of a campaign
- Reallocating rewards from specific addresses (partial reallocation)

**No fees**: Reallocation operations are free—no fees are charged.

**Multiple campaigns**: If an opportunity contains multiple campaigns that need reallocation, you must perform reallocation for each campaign ID separately. When using the Studio interface, this means repeating the reallocation process for each campaign. However, when using the programmatic approach with a Safe multisig, you can batch all reallocation transactions into a single Safe transaction by creating multiple calls to `reallocateCampaignRewards`—one for each campaign ID. This is more efficient and cost-effective when dealing with multiple campaigns.

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
**Using `&test=true` with aglaMerkl or other test tokens**

When using test tokens like aglaMerkl in your campaigns (both regular reward campaigns and point campaigns with mock tokens), these tokens are automatically hidden from the Merkl frontend. To retrieve campaign data via the API for these test tokens, you must include the `&test=true` parameter in your API requests. This ensures that campaigns using test tokens (which are filtered out of the frontend) will be included in the API response.
{% endhint %}

Note: You can find each campaign or opportunity ID directly on the opportunities page [here](https://app.merkl.xyz/).