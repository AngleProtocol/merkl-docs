---
description: Manage your campaigns once they’re live
---

# Campaign Management

Once your campaign is created, you will be able to see it in the [Opportunities page](https://app.merkl.xyz/). Note that it can take up to 1 hour to be visible on our app due to cache.

## Campaign Management Actions

You now have the full ownership of your campaign. You’ll be able to:

* **Monitor its performance** using our API (refer [here](https://docs.merkl.xyz/integrate-merkl/app) for more details) or directly in the studio’s dashboard (coming soon)
* **Track any delay** related to the campaign or the chain on which the campaign is running using [Merkl status page](https://app.merkl.xyz/status)
* **See the campaign leaderboard** in the opportunity page to track addresses’ participation
* **Integrate campaign data** into your own app using our API. You’ll find the procedure [here](https://docs.merkl.xyz/integrate-merkl/app)
* **Renew your campaigns** Campaigns cannot be extended after they end, but you can easily create a new one using the same parameters
* **Reallocate unclaimed rewards** from any recipient to another address at your discretion. You’ll find the procedure [here](https://docs.merkl.xyz/merkl-mechanisms/features#campaign-cancellation)
* **Cancel a campaign** by opening the [Studio](https://studio.merkl.xyz/users/) and selecting the campaign you want to cancel. You can find a full guide [here](https://docs.merkl.xyz/merkl-mechanisms/features)

*   (If needed) **edit some parameters** of your campaigns.

    ✅ You can:

    * Change the start date & end dates if they are not in the past
    * Edit blacklists or whitelists to include or exclude certain addresses
    * Enable additional features as needed

    ❌ You cannot:

    * Change the reward token
    * Change the total reward amount. 
        - To **reduce** the total amount: cancel and recreate a new campaign.  
        If you want identical dates, **offset** either the start or end by **≥ 15 min** to avoid campaign duplication in the merkl engine.
        - To **increase** the total amount: create an **additional campaign** running in parallel. In the Merkl app, look for opportunities with **2+ lightning icons** to see examples of multiple active campaigns on an opportunity.
    
**Note**: If you want to add extra rewards or add another new reward token to your existing campaign, you’ll need to create another campaign on top of the current one. The new campaign must have a slightly modified start or end date (by a minimum of one hour) to prevent campaign duplication in our engine.

## Merkl API Good to know

To fetch important data regarding your campaigns, you can use our API to get detailed information. More information [here](https://docs.merkl.xyz/integrate-merkl/app). Here are the most commonly used endpoints:

- Getting the leaderboard for your campaign: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/)
- Getting opportunities and their included campaigns: [https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/campaigns](https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/campaigns)
- Checking rewards amount at user level: [https://api.merkl.xyz/docs#tag/users/GET/v4/users/%7Baddress%7D/rewards](https://api.merkl.xyz/docs#tag/users/GET/v4/users/%7Baddress%7D/rewards)
- Checking rewards amount at campaign level: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/total)
- Checking rewards amount at token level: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/token/)
- Checking how many rewards are unclaimed: [https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/](https://api.merkl.xyz/docs#tag/rewards/get/v4/rewards/unclaim/)
- Integrating APRs data in your front-end: [https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/](https://api.merkl.xyz/docs#tag/opportunities/get/v4/opportunities/)
    - For this route, match the incentivized asset’s address with the “explorerAddress” field.

Note: You can find each campaign ID directly on the opportunities page [here](https://app.merkl.xyz/).