---
description: Answers to common questions from Merkl users
---

# Merkl User FAQ

## What’s the frequency of rewards distribution?

The rewards distribution frequency varies by chain. On average, rewards are distributed approximately every 8 hours.

Note that the reward update schedule differs from the reward computation schedule. New rewards based on activity are computed and appear as pending approximately every 2 hours on each chain, while distribution updates occur less frequently. When pending rewards (updated every 2 hours) are pushed onchain (every 8 hours), they effectively become claimable.

You can find the last reward updates for each chain on the [Status page](https://app.merkl.xyz/status).

To learn more about reward updates vs. reward computation, refer to [this page](../merkl-mechanisms/technical-overview.md#reward-updates).

## I participated in a Merkl campaign but I can't claim rewards on Merkl. Why?

If you're unable to claim rewards on Merkl after participating in a campaign, here are a few possible reasons:

**Campaign Status**: The campaign may not have started yet. Check its status — if it's marked as "Upcoming," it hasn't started. Only "Live" campaigns are currently active, while completed ones are labeled "Past."

**Processing Delay**: There may be a delay in the campaign computation process. To check if there's a delay, refer to the status page or navigate to the campaign section on the opportunity page. In the advanced section, you can see when the campaign was last processed.

If there's a delay in the Merkl engine run for a campaign, you'll receive any missed rewards during the next engine run once the issue is resolved.

Since the Merkl engine runs multiple times per day, you won't have to wait long for missed rewards. For example, if a campaign didn't distribute rewards today but runs the following day after the issue is fixed, you'll receive both the missed and current rewards.

One common reason for delays is the absence of swaps in the pool. Swaps are necessary to calculate certain rewards, so if no swap occurs, the campaign will be delayed until the next engine run after a swap takes place.

Learn more about rewards delays within Merkl [on this page](../merkl-mechanisms/technical-overview.md#reward-computation).

## I performed an action (providing liquidity, lending, borrowing, …) on one chain, but rewards are distributed on another, is that normal?

Yes, the chain where you perform an action may differ from the one where rewards are distributed. For example, you might provide liquidity in a pool on the Base network and receive rewards on Optimism. This setup is defined by the incentivizers when they create the reward campaign.

You can find the chain where rewards are distributed in the campaign details on the opportunity page.

Learn more about cross-chain campaigns [on this page](../merkl-mechanisms/features#-cross-chain-campaigns).

## My wallet address has been blacklisted. Why?

Merkl automatically blacklists addresses that display suspicious behavior, such as wash trading to artificially inflate rewards. This typically involves addresses trading in pools where they hold positions specifically to maximize rewards.

If you believe you were blacklisted by mistake, please open a support ticket with your wallet address and an explanation of your activity. Our team will review your case.

Learn more about blacklists in [the case of concentrated liquidity campaigns](../merkl-mechanisms/campaign-types/concentrated-liquidity-mechanisms.md#-fake-volume-attacks-detection)

## An opportunity I'm interested in is marked as linked to other opportunities in the Merkl App. What does this mean?

When an opportunity is marked as linked to other opportunities, it means your deposits flow through multiple layers of contracts.

For instance, a vault in which you supply assets (child opportunity) may allocate part of its funds to a liquidity pool (parent opportunity) that earns rewards from Merkl campaigns. Thanks to Merkl forwarders, you receive rewards from the parent opportunity (in which you indirectly participate), in addition to the rewards from the child opportunity itself.

This setup can sometimes boost your returns, as participating in a child opportunity may combine rewards from both parent and child opportunities.

Learn more about reward forwarding and linked opportunities [on this page](../merkl-mechanisms/reward-forwarding.md#linked-opportunities).

## How are APRs calculated?

The main APR displayed is calculated as:

$$
\frac{\text{Daily Rewards} \times 365}{\text{Adjusted TVL*}}
$$

* When there are no specific conditions or eligibility restrictions, **adjusted TVL = TVL**
* When there are some (e.g, net lending, whitelist and/or blacklist hooks, or eligibility thresholds), **adjusted TVL = Net TVL**

[Learn more about Merkl APRs, TVLs and daily rewards](../merkl-mechanisms/technical-overview.md#key-merkl-metrics)

## What do "Amount", "Pending", and "Claimed" mean in the UI?

* **Amount**: The total number of tokens already credited to the user onchain
* **Pending**: Rewards that are queued and will be pushed onchain in the next update
* **Claimable**: Rewards that have been computed and are ready to be claimed by users
* **Claimed**: The amount the user has already claimed

Dive deeper into the different fields in the UI by referring to [the API docs](../integrate-merkl/app.md#integrating-user-rewards).
