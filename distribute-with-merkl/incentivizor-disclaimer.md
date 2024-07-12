---
description: Please be aware of the following before depositing an incentive on Merkl
---

# Incentivizor Disclaimer

1. Merkl is experimental software provided as is, use it at your own discretion. There may notably be delays in the onchain merkle root updates and there may be flaws in the script or in the infrastructure used to update results onchain. In that regard, everyone can permissionlessly dispute the rewards which are posted onchain, and when creating a campaign, you are responsible for checking the results and eventually dispute them.
2. If you are specifying an invalid address to incentivize or an address that is not marked as supported, your rewards will not be taken into account, instead, all the rewards will be distributed to the address which created the campaign so you can recover the funds.
3. If you do not blacklist smart contracts which could be eligible for rewards (e.g. liquidity position managers or smart contract addresses holding LP tokens that are not natively supported by the Merkl system), then the script will not be able to take the specifities of these addresses into account, and it will reward them like a normal externally owned account would be. If these are smart contracts that do not support external rewards, then rewards that should be accruing to it will be lost.
4. If rewards sent through Merkl remain unclaimed for a period of more than 1 year (because they are meant for instance for smart contract addresses that cannot claim or deal with them), then we reserve the right to recover these rewards and redistribute part of it.
5. Fees apply to incentives deposited on Merkl, unless the address incentivized is or contains a whitelisted token (e.g an Angle Protocol stablecoin).
6. By interacting with the Merkl smart contract to deposit an incentive for a pool, you are exposed to smart contract risk and to the offchain mechanism used to compute reward distribution.
7. If the rewards you are sending are too small in value, or if you are sending rewards using a token that is not approved for it, your rewards will not be handled by the script, and they will be lost.
8. If you mistakenly send too much rewards compared with what you wanted to send, you will not be able to call them back. You will also not be able to prematurely end a reward distribution once created.
9. The script handling reward distribution for an address may not look at all the onchain actions (e.g. swaps on a pool) during the time for which you are incentivizing, but just at a subset of it to gain in efficiency. Overall, if you distribute incentives using Merkl, it means that you are aware of how the script works, of the approximations it makes and of the behaviors it may trigger (e.g. just in time liquidity).
10. Rewards corresponding to incentives distributed through Merkl do not compound block by block, but are regularly made available (through a merkle root update) at a frequency which depends on the chain.
