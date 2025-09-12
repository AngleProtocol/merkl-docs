---
description: Answers to common questions from Merkl users
---

# Merkl User FAQ

### What’s the frequency of rewards distribution?

The rewards distribution frequency varies by chain. On average, rewards are distributed approximately every 9 hours, but some chains — like Arbitrum — have distributions as frequently as every 4 hours. You can find the specific distribution frequency for each chain on the [Status page](https://app.merkl.xyz/status).

### I participated in a Merkl campaign but I can’t claim rewards on Merkl. Why?

If you're unable to claim rewards on Merkl after supplying liquidity to a pool, here are a few possible reasons:

*   **Concentrated Liquidity Campaigns:** For campaigns based on concentrated liquidity, the Merkl engine requires at least one swap in the pool to calculate rewards accurately. If no swaps have occurred since the last engine run, the engine won’t process rewards yet, as one of the reward weights depends on swap fees.

    _Note:_ Reward allocation is retroactive. Once a swap occurs, the Merkl engine will distribute all rewards accrued from the last engine run. If no engine run has happened since you added liquidity, you will receive all rewards accumulated from the time you initially supplied liquidity.
* **Campaign Status:** The campaign may not have started yet. Check its Status — if it’s marked as "Upcoming," it hasn’t started. Only "Live" campaigns are currently active, while completed ones are labeled "Past."

### I performed an action (providing liquidity, lending, borrowing, …) on one chain, but rewards are distributed on another, is that normal?

Yes, the chain where you perform an action may differ from the one where rewards are distributed. For example, you might provide liquidity in a pool on the Base network and receive rewards on Optimism. This setup is defined by the incentivizers when they create the reward campaign.

You can find the chain where rewards are distributed in the campaign details on the opportunity page.

### My wallet address has been blacklisted. Why?

Merkl automatically blacklists addresses that display suspicious behavior, such as wash trading to artificially inflate rewards. This usually involves addresses trading in pools where they hold positions specifically to maximize rewards.

If you believe you were blacklisted by mistake, please open a support ticket and provide your wallet address along with an explanation of your activity. Our team will review your case.

### An opportunity I’m interested in is marked as linked to other opportunities in the Merkl App. What does this mean?

When a campaign is marked as linked to other opportunities, it means your deposits flow through multiple layers of contracts.

For instance, a vault in which you supply assets (child opportunity) may allocate part of the funds into a liquidity pool (parent opportunity) and earn rewards from Merkl campaigns distributing incentives to the pool’s liquidity providers. Thanks to Merkl forwarders, you receive any rewards from the parent opportunity (in which you indirectly participate), in addition to the rewards from the child opportunity itself.

This setup can sometimes boost your returns, as participating in a child opportunity may combine rewards from both the parent and child.

### The daily distribution of rewards didn't occur. Will I still receive my rewards?

Yes, rewards are retroactive. If there’s a delay in the Merkl engine run for a campaign, you’ll receive any missed rewards during the next engine run once the issue is resolved. Since the Merkl engine runs multiple times per day, you won’t have to wait long for missed rewards. For example, if a campaign didn’t distribute rewards today but the engine runs the following day after the issue is fixed, you’ll receive both the missed rewards and the current rewards.

One common reason for delays is the absence of a swap in the pool. Swaps are necessary to calculate certain rewards, so if no swap occurs, the campaign will be delayed until the next engine run after a swap takes place.

### How are APRs calculated?

The main APR displayed is calculated as:

$$
\frac{\text{Daily Rewards} \times 365}{\text{Adjusted TVL*}}
$$

* When there are no specific conditions or eligibility restrictions, **adjusted TVL = TVL**
* When there are some (e.g, net lending, whitelist and/or blacklist hooks, or eligibility thresholds), **adjusted TVL = Net TVL**

While Merkl campaigns generally use a standardized formula to compute TVLs and hence APRs, there are important nuances when it comes to how TVL is defined and used, especially for opportunities with non-trivial eligibility conditions (e.g., net lending, whitelist hooks, or eligibility thresholds).

For certain opportunity types, such as net lending, Merkl attempts — when possible — to compute APR based on the real, net opportunity TVL. As a result, we try to use the net supplied TVL (i.e., total supplied minus borrowed) as the basis for APR calculation instead of raw deposits.

For opportunities that include blacklisted or whitelisted addresses, or have specific eligibility criteria, the Net TVL will only include the TVL from addresses that meet all the defined conditions.

Overall, TVL within Merkl are adapted as much as possible to reflect the true opportunity size, but there are situations where this TVL might be over or underestimated, because they rely on approximations for simplicity in computations or because exact computing logic was not yet implemented.

If you’ve got a doubt about a true opportunity TVL, feel free to reach out to us to understand how APRs are being computed.

### What do "Amount", "Pending", and "Claimed" mean in the UI?

* **Amount** refers to the total number of tokens already credited to the user onchain.
* **Pending** refers to rewards that are queued and will be pushed onchain in the next update.
* **Claimable** refers to rewards that have been computed and are ready to be claimed by users.
* **Claimed** shows the amount the user has already claimed.
