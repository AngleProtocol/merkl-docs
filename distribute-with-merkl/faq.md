# FAQ

Merkl is experimental software provided "as is." Please use it at your own discretion.

To learn how to deploy the type of campaign you want to launch, check the corresponding campaign type. Detailed instructions for your campaign will be provided there. Visit this [page](types-of-campaign/) to explore all the campaign types supported by Merkl.

### How do/can I whitelist my token?&#x20;

* Tokens distributed as rewards must be whitelisted on-chain. To whitelist your token, please complete this [form](https://tally.so/r/3y2bqx). Our team will carefully review and process your request to ensure accurate APR calculation and to maintain the security of our users.

### I have just created my campaign and can't see it live on Merkl. Is this normal?&#x20;

* Yes, this is normal. Campaign creation typically takes about 30 minutes to 1 hour before it becomes visible on Merkl's frontend. If it takes longer than this, please contact the Merkl team by opening a [Tech Support ticket on Discord](https://discord.com/channels/1209830388726243369/1210212731047776357/1242132474759221331).

### Are all protocols supported natively by Merkl?&#x20;

* &#x20;It depends. V2 Liquidity Pools and lending/borrowing protocols are natively supported. However, we highly recommend being integrated by Merkl for computing the APRs and TVL accurately.&#x20;
* All AMMs and ALMs shown on Merkl's app have been whitelisted and integrated into Merkl. For a comprehensive list of supported AMMs and ALMs, check this [link](https://app.merkl.xyz/integrations).&#x20;
* For any integration inquiries, please reach out to us by [opening a BD ticket on Merkl’s Discord.](https://discord.com/channels/1209830388726243369/1210212731047776357)

### Who should I contact to integrate Merkl?&#x20;

* The specific needs of your project may vary, but regardless of your situation, please open a [BD ticket on Merkl's Discord](https://discord.com/channels/1209830388726243369/1210212731047776357/1210859311970918442). Additionally:
  * If you are an Automated Market Maker (AMM) looking to integrate Merkl, please complete this [form](https://tally.so/r/3XJODP).&#x20;
  * If you are an Automated Liquidity Manager (ALM) interested in integrating Merkl, kindly fill out this [form](https://tally.so/r/w4JYLr).

### Are they fees for using Merkl ?&#x20;

* Yes, for campaigns, the maintenance fee is 3%, whereas for airdrops, it is 0.5%.

### How often are rewards made available?&#x20;

* Rewards corresponding to incentives distributed through Merkl do not compound block by block but are made available regularly (through a Merkle root update) at a frequency depending on the chain.

### Can there be delays in campaign updates

* Yes, there may be delays in the on-chain Merkle root updates due to potential flaws in the script or infrastructure used to update results on-chain.

### Can rewards posted on-chain be disputed  ?&#x20;

* Yes, everyone can permissionlessly dispute the rewards posted on-chain. When creating a campaign, you are responsible for checking the results and disputing them if necessary.

### What happens if I specify an invalid or unsupported address ?&#x20;

* If you specify an invalid address or an address not marked as supported, your rewards will not be taken into account. Instead, all rewards will be distributed to the address that created the campaign so you can recover the funds.

### What should I do about smart contract address eligible for rewards ?&#x20;

* If you do not blacklist smart contracts eligible for rewards (e.g., liquidity position managers or smart contract addresses holding LP tokens not natively supported by the Merkl system), the script will not account for the specificities of these addresses and will reward them like a normal externally owned account. If these are smart contracts that do not support external rewards, the rewards accruing to them will be lost.

### What happens to unclaimed rewards ?&#x20;

* If rewards sent through Merkl remain unclaimed for over a year, we reserve the right to recover these rewards and redistribute part of them.

### What happens if I send below the Min rewards per hour or unapproved rewards ?

* If the Rewards per hour you are sending is below the Min Rewards per hour, or if you are using a token that is not approved, your rewards will not be handled by the script and will be lost.

### Can I retrieve rewards if I send too much by mistake ?&#x20;

* No, if you mistakenly send too many rewards compared to what you intended, you will not be able to retrieve them. Additionally, you cannot prematurely end a reward distribution once it has been created.

### Does the script consider all on-chain actions during incentivization?&#x20;

* **For CLAMM:** The script handling reward distribution may not consider all on-chain actions (swaps on a pool) during the incentivization period, but only a subset to improve efficiency. Using Merkl implies that you understand how the script works, the approximations it makes, and the behaviors it may trigger (e.g., just-in-time liquidity).
* **For ERC20 campaigns:** The engine reviews ERC20 holdings every 20 minutes to ensure accurate reward distribution.
