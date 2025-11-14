---
description: >-
  Get your campaign on the Merkl app while keeping control of computational
  load.
---

# Encompassing Campaigns

Merkl's All Encompassing Campaigns enable protocols to distribute rewards without requiring Merkl's Engine to calculate reward allocation. In this model, partners provide some API endpoints containing opportunity data and rewards, which are distributed based on the provided output.

The Merkl Engine aggregates the computed reward data provided by the partner into a Merkle root and pushes it onchain, enabling users to claim their rewards.

Once [a reward update](../technical-overview.md#reward-updates) has been processed, it becomes immutable and cannot be reverted. Nevertheless, campaign data can be dynamically updated throughout the campaign duration by modifying the reward endpoint output, with the final update required before the campaign's end date.

## 🛠️ Campaign configuration

Campaigns creators must provide the following information upon creation:

* 📅 Start Date – The date on which the campaign starts.
* 📅 End Date – The date on which the campaign ends.
* 🎁 Reward url - A public endpoint returning a reward JSON file.
* 📊 Data url - A public endpoint returning a data JSON file.

### 1. 🎁 Reward JSON

The expected format for the JSON reward is the following

```
type EncompassingJSON = {
    // Token being distributed
    rewardToken: string;
    // User rewards
    rewards: {
        // recipient: user address
        [recipient: string]: {
            // Reason: how the user got the rewards
            [reason: string]: {
              // amount: amount with all decimals as a string
              amount: string;
              // timestamp: unix timestamp in seconds, date at which the reward is sent to the recipient (must not exceed the end date of the campaign)
              timestamp: string; 
            }
        };
    };
};
```

Example:

```
{
  "rewardToken": "0xE0688A2FE90d0f93F17f273235031062a210d691",
  "rewards": {
    "0x9f76a95AA7535bb0893cf88A146396e00ed21A12": {
      "epoch-1": {
        "amount": "40000000000000000000",
        "timestamp": "1732294694"
      }
    },
    "0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185": {
      "epoch-2": {
        "amount": "100000000000000000000",
        "timestamp": "1741370722"
      }
    }
  }
}
```

* **rewardToken:** The token being distributed.
* **rewards:** User rewards with recipient addresses, reasons, amount and timestamp for the rewards.

Good to know:

* `timestamp` is the unix timestamp in seconds, date at which the reward is sent to the recipient. This timestamp must not exceed the end date of the campaign (it will not be processed if it's after the end date), and if it's before the start date of the campaign, the reward will be sent to the recipient at the start date.
* Once the engine has processed some rewards, even if the reward endpoint is updated, the rewards already distributed can't be reverted. If a campaign creator needs to send additional rewards to the same user, he can do it by adding another reason for the same recipient.

Example:

```
{
  "rewardToken": "0xE0688A2FE90d0f93F17f273235031062a210d691",
  "rewards": {
    "0x9f76a95AA7535bb0893cf88A146396e00ed21A12": {
      "epoch-1": {
        "amount": "40000000000000000000",
        "timestamp": "1732294694" // at this timestamp, `40000000000000000000` tokens will be claimable by the recipient
      },
      "epoch-2": { // new reason for the same recipient
        "amount": "100000000000000000000",
        "timestamp": "1741370722" // at this timestamp, `100000000000000000000` additional tokens will be claimable by the recipient
      }
    },
    "0xfdA462548Ce04282f4B6D6619823a7C64Fdc0185": {
      "epoch-2": {
        "amount": "100000000000000000000",
        "timestamp": "1741370722"
      }
    }
  }
}
```

{% hint style="warning" %}
**Keep recent history:** Include at least 3/4 days of past entries in your Reward JSON so that, if there are delays, the Merkl engine can still fetch and process recent rewards without requiring you to re-upload old data.
{% endhint %}


### 2. 📊 Data JSON

The expected format for the JSON data is the following

```
type DataJSON = {
  tvl: string;
  apr: string; // in decimal
  opportunityName: string; // optional
}
```

Example:

```
{
  "tvl": "100000",
  "apr": "0.5",  // 0.5 in decimal = 50% APR
  "opportunityName": "Testing distribution aglaMerkl"
}
```

This data will be used to display the campaign in the Merkl frontend.

### Validating your endpoints
<details>

<summary>You can use this python script to validate that your endpoints are in the correct format</summary>

```python
import requests
import json
import re
from typing import Dict, Any, Optional

def is_number_regex(s):
    # Matches integers and decimals (including negative)
    return bool(re.match(r'^-?\d+(\.\d+)?$', s))

def validate_data_url(url: str) -> Optional[Dict[str, Any]]:
    """
    Validate that the data URL returns the expected format:
    {
      "tvl": "100000",
      "apr": "0.5",
      "opportunityName": "Testing distribution aglaMerkl"
    }
    """
    print(f"\n=== Validating Data URL ===")
    print(f"URL: {url}")
    
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        
        # Check required fields
        required_fields = ["tvl", "apr", "opportunityName"]
        missing_fields = [field for field in required_fields if field not in data]
        
        if missing_fields:
            print(f"❌ INVALID: Missing required fields: {missing_fields}")
            return None
        
        # Validate types
        if not isinstance(data["tvl"], str):
            print(f"❌ INVALID: 'tvl' should be a string, got {type(data['tvl']).__name__}")
            return None
        
        if not isinstance(data["apr"], str):
            print(f"❌ INVALID: 'apr' should be a string, got {type(data['apr']).__name__}")
            return None
        
        if not isinstance(data["opportunityName"], str):
            print(f"❌ INVALID: 'opportunityName' should be a string, got {type(data['opportunityName']).__name__}")
            return None
        
        # Validate numeric formats
        if not is_number_regex(data["tvl"]):
            print(f"❌ INVALID: 'tvl' should be a numeric string, got '{data['tvl']}'")
            return None
        if not is_number_regex(data["apr"]):
            print(f"❌ INVALID: 'apr' should be a numeric string, got '{data['apr']}'")
            return None
        
        print("✅ VALID: Data URL format is correct")
        print(f"   TVL: {data['tvl']}")
        print(f"   APR: {data['apr']}")
        print(f"   Opportunity Name: {data['opportunityName']}")
        
        return data
        
    except requests.RequestException as e:
        print(f"❌ ERROR: Failed to fetch data from URL: {e}")
        return None
    except json.JSONDecodeError as e:
        print(f"❌ ERROR: Invalid JSON response: {e}")
        return None


def validate_reward_url(url: str) -> Optional[Dict[str, Any]]:
    """
    Validate that the reward URL returns the expected format:
    {
      "rewardToken": "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2",
      "rewards": {
        "0x37305B1cD40574E4C5Ce33f8e8306Be057fD7341": {
          "season1": {
            "amount": "1000000000000000000",
            "timestamp": "1741370722"
          }
        }
      }
    }
    """
    print(f"\n=== Validating Reward URL ===")
    print(f"URL: {url}")
    
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        
        # Check required top-level fields
        if "rewardToken" not in data:
            print(f"❌ INVALID: Missing 'rewardToken' field")
            return None
        
        if "rewards" not in data:
            print(f"❌ INVALID: Missing 'rewards' field")
            return None
        
        if not isinstance(data["rewardToken"], str):
            print(f"❌ INVALID: 'rewardToken' should be a string, got {type(data['rewardToken']).__name__}")
            return None
        
        if not isinstance(data["rewards"], dict):
            print(f"❌ INVALID: 'rewards' should be a dict, got {type(data['rewards']).__name__}")
            return None
        
        # Validate rewards structure
        for address, seasons in data["rewards"].items():
            if not isinstance(seasons, dict):
                print(f"❌ INVALID: Rewards for {address} should be a dict, got {type(seasons).__name__}")
                return None
            
            for season_name, season_data in seasons.items():
                if not isinstance(season_data, dict):
                    print(f"❌ INVALID: Season data for {address}/{season_name} should be a dict")
                    return None
                
                if "amount" not in season_data:
                    print(f"❌ INVALID: Missing 'amount' in {address}/{season_name}")
                    return None
                
                if "timestamp" not in season_data:
                    print(f"❌ INVALID: Missing 'timestamp' in {address}/{season_name}")
                    return None
                
                if not isinstance(season_data["amount"], str):
                    print(f"❌ INVALID: 'amount' should be a string in {address}/{season_name}")
                    return None
                
                if not isinstance(season_data["timestamp"], str):
                    print(f"❌ INVALID: 'timestamp' should be a string in {address}/{season_name}")
                    return None
        
        print("✅ VALID: Reward URL format is correct")
        print(f"   Reward Token: {data['rewardToken']}")
        print(f"   Total Addresses: {len(data['rewards'])}")
        
        return data
        
    except requests.RequestException as e:
        print(f"❌ ERROR: Failed to fetch data from URL: {e}")
        return None
    except json.JSONDecodeError as e:
        print(f"❌ ERROR: Invalid JSON response: {e}")
        return None


def calculate_user_rewards(reward_data: Dict[str, Any], decimals: int = 18):
    """
    Calculate and print the amount each user would receive (divided by 1e18)
    """
    print(f"\n=== User Reward Amounts ===")
    
    if not reward_data or "rewards" not in reward_data:
        print("No reward data available")
        return
    
    total_amount = 0
    user_totals = {}
    
    for address, seasons in reward_data["rewards"].items():
        user_total = 0
        for reason, season_data in seasons.items():
            amount_wei = int(season_data["amount"])
            amount_tokens = amount_wei / (10 ** decimals)
            user_total += amount_tokens
        
        user_totals[address] = user_total
        total_amount += user_total
    
    # Print sorted by amount (descending)
    sorted_users = sorted(user_totals.items(), key=lambda x: x[1], reverse=True)
    
    for address, amount in sorted_users:
        print(f"{address}: {amount:,.6f} tokens")
    
    print(f"\n{'='*60}")
    print(f"Total Amount Distributed: {total_amount:,.6f} tokens")
    print(f"{'='*60}")
    
    return total_amount
    
def check_campaign_rewards(campaign_id: str, expected_total: float, decimals: int = 18):
    """
    Check if the campaign has enough budget for the expected total rewards
    """
    print(f"\n=== Checking Campaign Rewards ===")
    
    url = f"https://api.merkl.xyz/v4/campaigns/{campaign_id}"
    
    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        
        total_budget_wei = int(data.get("amount", "0"))
        total_budget_tokens = total_budget_wei / (10 ** decimals)
        
        print(f"Campaign ID: {campaign_id}")
        print(f"Total Budget: {total_budget_tokens:,.6f} tokens")
        print(f"Expected Total Rewards: {expected_total:,.6f} tokens")
        
        if total_budget_tokens >= expected_total:
            print("✅ The campaign has enough budget for the expected rewards.")
        else:
            print("❌ The campaign does NOT have enough budget for the expected rewards.")
        
    except requests.RequestException as e:
        print(f"❌ ERROR: Failed to fetch campaign metrics: {e}")
    except json.JSONDecodeError as e:
        print(f"❌ ERROR: Invalid JSON response: {e}")


if __name__ == "__main__":
    # Example URLs - replace with actual URLs
    data_url = "https://gist.githubusercontent.com/BaptistG/576bd5711fded3f44d906efbcaff80e0/raw"
    reward_url = "https://gist.github.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3/raw"
    decimals = 18  # Adjust if reward token has different decimals
    campaign_id = "3141666711044140572" # Leave empty if campaign not created yet. If filled with the campaign DB ID, the script will check if the amounts match and if you have enough budget in the campaign.
    
    print("=" * 60)
    print("URL Format Validation Script")
    print("=" * 60)
    
    # Validate data URL
    data_result = validate_data_url(data_url)
    
    # Validate reward URL
    reward_result = validate_reward_url(reward_url)
    
    # If reward URL is valid, calculate user amounts
    if reward_result:
        total_amount = calculate_user_rewards(reward_result, decimals)
        # If campaign ID is provided, check campaign rewards
        if campaign_id and total_amount is not None and campaign_id.strip() != "":
            check_campaign_rewards(campaign_id, total_amount, decimals)
    
    print("\n" + "=" * 60)
    print("Validation Complete")
    print("=" * 60)
```
</details>

### ⚠️ Important Note

Merkl applies a 0.5% fee to this type of campaigns. This fee is added on top of the total airdropped amount, ensuring recipients receive the full intended distribution.

* 💡 If you want exactly 100,000 tokens to be distributed to users, you need to provide 100,502.51 tokens (calculated as 100,000 / (1 - 0.5%)).
* 💡 If you prefer to send exactly 100,000 tokens from your wallet, then the total sum of allocations in your JSON file should be 99,500 tokens (calculated as 100,000 \* (1 - 0.5%)).

Our frontend automatically calculates the correct amount for you.

### ⏳ Distribution lag

Tokens become claimable at the next [reward update](../technical-overview.md#reward-updates) on the target chain, which typically occurs within 8 hours. If you plan to announce the distribution, we recommend waiting until the rewards are claimable to notify your users.

## Tutorial for static encompassing campaigns

If you are running a quest on your app where users become eligible to rewards after a while, it can be beneficial to create an encompassing campaign because your quest will be exposed to our user base.

However, you probably don't need to go through the trouble of developing a real API endpoint for us because the data is almost static (only updated once, at the end of the quest).

What we recommend is doing this via gists.

### Data JSON
Here is an example data JSON using a gist:
[https://gist.githubusercontent.com/BaptistG/576bd5711fded3f44d906efbcaff80e0/raw](https://gist.githubusercontent.com/BaptistG/576bd5711fded3f44d906efbcaff80e0/raw)

{% hint style="warning" %}

**Important**: You need to give us a **raw** URL of a **public** gist.

**Creating a public gist:** Go to [https://gist.github.com/](https://gist.github.com/), fill in the data and select `Create public gist`

**Getting the raw URL:** Once you create your gist, the URL of the page you are on should look something like this: `https://gist.github.com/BaptistG/576bd5711fded3f44d906efbcaff80e0`. To get the raw url, you need to add `/raw` at the end (`https://gist.github.com/BaptistG/576bd5711fded3f44d906efbcaff80e0/raw` with the previous example).

{% endhint %}

### Reward JSON

You must first initialize the gist of the reward URL. You do this by creating a public gist of the following format:

```json
{
  "rewardToken": "<ADDRESS OF THE REWARD TOKEN YOU WILL AIRDROP IN CHECKSUM>",
  "rewards": {}
}
```

Here is an example if you want to airdrop [WETH](https://etherscan.io/token/0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) on Ethereum: [https://gist.github.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3/42921f90a660bfb7db8b6f890adc725644ad1490](https://gist.github.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3/42921f90a660bfb7db8b6f890adc725644ad1490)

You can now use the raw URL to initialize the campaign: `https://gist.githubusercontent.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3/raw/` (you will notice that this URL has data in there, this is because it is the latest version of the file)

### Create the campaign

You now have everything you need to create the campaign. You will need to choose the start and end dates of the campaign and commit the airdropped amount upfront.

{% hint style="info" %}

If you don't airdrop all the tokens by the end of the campaign, you will automatically get the undistributed tokens back

{% endhint %}

### Airdropping the rewards

Before the end of the campaign you created, you need to update the rewards endpoint to tell Merkl to airdrop the rewards. To do so you should go to your gist, click edit and add data. For example, if I want to airdrop the following rewards of my token that has 18 decimals to 3 users where:
- User 1 gets 1 token
- User 2 gets 4 tokens
- User 3 gets 6 tokens

The gist should look like this:

```json
{
  "rewardToken": "<ADDRESS OF THE REWARD TOKEN YOU WILL AIRDROP IN CHECKSUM>",
  "rewards": {
    "<ADDRESS OF USER 1>": {
      "season1": {
        "amount": "1000000000000000000",
        "timestamp": "1741370722"
      }
    },
    "<ADDRESS OF USER 2>": {
      "season1": {
        "amount": "4000000000000000000",
        "timestamp": "1741370722"
      }
    },
    "<ADDRESS OF USER 3>": {
      "season1": {
        "amount": "6000000000000000000",
        "timestamp": "1741370722"
      }
    }
  }
}
```

Here is an example for the WETH airdrop: [https://gist.github.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3](https://gist.github.com/BaptistG/e9bc9e9703a40cd6ad7e30d3e4e039a3)


{% hint style="warning" %}

**Important:** 
- Your GitHub account has the power to update the gist, so if you get your account compromised, someone could airdrop themselves everything.
- You must update this gist at least 12 hours before the scheduled end of the campaign. If you don't, our engine might not distribute the rewards and send you all the tokens back at the end of the campaign instead of running the airdrop
- Don't forget that Merkl will take a small fee on the tokens you send, you can't distribute more than what is in our distributor, to verify the amount, go to the campaign page on our app.

{% endhint %}