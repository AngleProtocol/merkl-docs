---
description: Manage your campaigns once they’re live
---

# Campaign Management

{% hint style="success" %}
**Campaign visibility**

Once your campaign is created, you will be able to see it in the [Opportunities page](https://app.merkl.xyz/). Note that it can take up to 1 hour to be visible on our app due to cache.
{% endhint %}

{% hint style="info" %}
**Reward delays & retroactive distribution**

Occasional delays of a few hours may occur in reward distribution. If an invariant is triggered or a third-party data feed experiences a temporary issue, calculations are paused until all safety checks pass at the next scheduled computation.

Rest assured that when this happens, **all rewards are distributed retroactively**. No rewards are lost.
{% endhint %}

## Campaign Management Actions

As the campaign creator, you have full control over your campaign. Available actions include:

* **Request address remapping**: For ongoing campaigns, you can contact the Merkl team to set up automatic address remapping, where rewards from specific addresses are automatically redirected to other addresses throughout the campaign duration. See the [address remapping section](campaign-management.md#address-remapping) below.
* **Reallocate unclaimed rewards**: As a campaign creator, you can redirect rewards from any recipient to another address at your discretion. See the [Campaign Reallocation section](../merkl-mechanisms/features.md#-campaign-reallocation) in Additional Features for instructions.
* Cancel a campaign: go in the [studio](https://studio.merkl.xyz/users/) using the creator address, select the campaign you want to cancel and click on the button on the right.
* (If needed) edit some parameters of your campaigns.

    ✅ You can:

    * Change the start date & end dates if they are not in the past
    * Edit blacklists or whitelists to include or exclude certain addresses
    * Enable additional features as needed

    ❌ You cannot:

    * Change the reward token
    * Change the total reward amount.
      * To **reduce** the total amount: cancel and recreate a new campaign.\
        If you want identical dates, **offset** either the start or end by **≥ 15 min** to avoid campaign duplication in the merkl engine.
      * To **increase** the total amount: create an **additional campaign** running in parallel. In the Merkl app, look for opportunities with **2+ lightning icons** to see examples of multiple active campaigns on an opportunity.

**Note**: If you want to add extra rewards or add another new reward token to your existing campaign, you’ll need to create another campaign on top of the current one. The new campaign must have a slightly modified start or end date (by a minimum of one hour) to prevent campaign duplication in our engine.

If you need assistance renewing your campaign, please notify us at least 3 days before it ends to ensure a smooth transition.

{% hint style="info" %}
**Monitoring campaign performance**

View leaderboards and key metrics like APR and TVL directly in the studio dashboard for your campaign. For advanced analytics and programmatic access, use the [Merkl API](https://docs.merkl.xyz/integrate-merkl/app).
{% endhint %}

## Address Remapping

Need rewards from a non-claimable contract to land in a claimable wallet? Ask the Merkl team to enable address remapping by pinging them with your campaign ID(s), the source address, and the destination address. The Merkl engine will then redirect the rewards automatically for the duration of the campaign.

For the full decision tree (when to prefer forwarding, remapping, or reallocation), head to the [Reward Forwarding guide](../merkl-mechanisms/reward-forwarding.md#address-remapping).
