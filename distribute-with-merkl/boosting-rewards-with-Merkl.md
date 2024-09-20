# Boosting Rewards with Merkl
Merkl allows you to boost rewards for users holding a specific token or NFT. The formula used for boosting is similar to Curve Finance’s but offers greater flexibility, as there is no 2.5x limit on boosting rewards for a user.

## Curve Formula
$$
B = \min \left( 2.5, 1.5 \times \frac{D \times v}{V \times d} + 1 \right)
$$

Where
- **B** is the boost a user receives.
- **d** is the value a user deposits, in USD.
- **D** is the total value deposited to the pool/vault, in USD.
- **v** is the amount of veCRV a user has (vote weight).
- **V** is the total veCRV in the system (total vote weight).

## Merkl Boost
There is no minimum value, and you can boost users holding any token as much as you want!
### Merkl Formula for Boost
$$
B = b \times \left( \frac{R \times v}{V \times r} )+ 1 \right
$$

Where:
- **B** is the boost a user receives.
- **b** is the boost multiplier selected by the Incentive Provider when creating a campaign (visible in the front end in the campaign creation page).
- **R** is the total amount of rewards distributed per epoch.
- **r** is the user’s rewards per epoch.
- **V** is the total supply of the token used for the boost (for NFTs, it’s the number of NFTs in a collection).
- **v** is the amount of the token held by the user during an epoch (or the number of NFTs held if applicable).

Applicable boost:
- For CLAMM campaigns.
- For any ERC20 tokens, including:
    - Constant Product AMM.
    - Holding a token campaign.
    - Lending/borrowing campaigns (with debt and receipt tokens).
    - For any campaigns on protocols that utilize ERC20 tokens
- For custom integrations: This feature is not automatically enabled. If you are interested, please reach out by opening a BD ticket in our Discord or directly contacting us via Telegram

### Boost for NFT Collections
We offer the ability to boost rewards based on the number of NFT a user holds from an NFT collection. If you'd like to set up a campaign with an NFT boost, please contact us to ensure the setup is configured accurately.
