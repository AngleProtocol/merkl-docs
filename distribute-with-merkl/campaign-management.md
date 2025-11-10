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

You now have the full ownership of your campaign. You’ll be able to:

* Monitor its performance using our API (refer [here](https://docs.merkl.xyz/integrate-merkl/app) for more details) or directly in the studio’s dashboard (coming soon)
* See the campaign leaderboard in the opportunity page to track addresses’ participation
* Integrate campaign data into your own app using our API. You’ll find the procedure [here](https://docs.merkl.xyz/integrate-merkl/app)
* **Request address remapping**: For ongoing campaigns, you can contact the Merkl team to set up automatic address remapping, where rewards from specific addresses are automatically redirected to other addresses throughout the campaign duration. See the [address remapping section](campaign-management.md#address-remapping) below.
* **Reallocate unclaimed rewards**: As a campaign creator, you can redirect rewards from any recipient to another address at your discretion. See the [Campaign Reallocation section](../merkl-mechanisms/features.md#-campaign-reallocation) in Additional Features for instructions.
* Cancel a campaign: go in the [studio](https://studio.merkl.xyz/users/) using the creator address, select the campaign you want to cancel and click on the button on the right.
*   (If needed) edit some parameters of your campaigns.

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

## Address Remapping

Campaign creators can request **address remapping** from the Merkl team to automatically redirect rewards from a source address to a destination address throughout the campaign duration. The Merkl engine handles this redirection automatically—no manual intervention is required.

**Remapping vs. Forwarding:**

While both mechanisms redirect rewards, they serve different purposes:

* **Forwarding** is an automatic Merkl feature that distributes rewards to users who hold the incentivized asset indirectly (e.g., through staking contracts or LP tokens). Forwarding works at the protocol level and is integrated directly into Merkl's reward distribution logic. See the [Forwarders section](../merkl-mechanisms/features.md#-forwarders) for more details.
* **Address remapping** is a manual configuration set up by the Merkl team that redirects rewards from one specific address to another specific address. It's used when a contract receives rewards but cannot claim them, and you want those rewards sent to a claimable address for the entire campaign duration.

**When to use address remapping:**

* You have a smart contract that receives rewards but cannot claim them, and you want these rewards automatically redirected to a claimable address
* You need ongoing, automatic redirection during a campaign (rather than one-time adjustments)

**How to request address remapping:**

Contact the Merkl team directly with:

* Your campaign ID(s)
* The source address(es) from which rewards should be redirected
* The destination address(es) where rewards should be sent

{% hint style="info" %}
Address remapping is particularly useful for long-running campaigns where you expect regular reward accumulation on addresses that cannot claim. Rather than performing frequent manual reallocations, remapping provides a seamless, automated solution.
{% endhint %}

{% hint style="info" %}
**Remapping vs. Reallocation**: Address remapping handles ongoing, automatic redirection during a campaign. Reallocation is better suited for one-time or occasional adjustments, especially after a campaign has ended or when you need immediate manual control. For complete details on reallocation, see the [Campaign Reallocation section](../merkl-mechanisms/features.md#-campaign-reallocation) in Additional Features.
{% endhint %}

## Fetching Campaign Information

The Merkl studio and app provide comprehensive campaign statistics and analytics. For programmatic access to campaign data, leaderboards, and reward information, refer to the [API integration guide](https://docs.merkl.xyz/integrate-merkl/app).