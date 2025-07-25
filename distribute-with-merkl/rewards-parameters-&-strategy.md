description: This page helps you define your rewards parameters & the associated strategy when creating your campaigns.
---

# Rewards Parameters & Strategy

## APR, TVL and rewards amount:

To estimate your reward amount, we recommend targeting an APR and back-calculating based on your desired TVL:
 1) daily rewards = (APR * TVL) / 365 

 2) total reward amount = daily rewards x campaign duration (in days)

For more details on this, refer to the documentation [here](https://docs.merkl.xyz/earn-with-merkl/faq-earn#how-are-aprs-calculated)

Let’s take an example: If you want your stable pool to reach $2M TVL, with a 10% APR, for a 14 days campaign, the rewards amount will be: 

daily rewards = (0.1 x $2M)/365 = 547,94$ 

total rewards =  547,94 x 14 = 7671,16$

## Weight fees parameters (for Concentrated Liquidity campaigns only):

Here are some suggestions depending on your needs and pool type:

- For stable pools/pegged pools (e.g., USDC/USDT or WBTC/BTC): Liquidity contribution = 80%, token0 = 10%, token1 = 10%
- For volatile pools (e.g., WETH/USDC): Liquidity contribution = 20%, token0 = 40%, token1 = 40% (the higher the volatility, the higher the weights for token0 and token1 percentages should be)
- You can also refer to this [blog](https://blog.merkl.xyz/merkl-insights-how-can-incentives-prevent-your-token-from-dumping) about liquidity walls (to prevent a token from dumping, or a stablecoin from losing its peg)

**Important notes:** 

- Merkl is not responsible for defining your strategy and targets. Nevertheless, you can refer to live campaigns to look for typical APRs and/or weight feed parameters on similar campaigns.
- For airdrops, distribution fees will be deducted automatically from the reward amount once the transaction is executed. Be sure to account for this when estimating your total budget.
- Each campaign must meet the minimum hourly token distribution (typically ≥ ~$1/hour). If a campaign in the payload falls below this threshold, the payload will not be generated and will return an error message.