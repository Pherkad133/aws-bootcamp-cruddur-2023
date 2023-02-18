# Week 0 — Billing and Architecture


## Regarding the architectural diagram the CI/CD logical pipeline in Lucid Charts

I was able to reproduce the diagram in Lucid charts and did the memonto SVG icon 

![Diagram](/Screenshot010846.png)

## used AWS cloudshell 
Cloudshell is an AWS shell that can be bash: Bash or pwsh: PowerShell or zsh: Z shell

## creating multifactor authentication for the root user 
I've understand that in case the root user got compromised it will be a grave danger to the security of the AWS account so layers of access security measures should be placed ahead like the multi factor authuntication using google authenticator on my android phone, but AWS did not provide any backup codes in case my phone went missing.

## creating a User besides the root user 
I have creased another user using AWS IAM as a security measure to not make the root user as exposed as possible and the created user was given the permission policy `AdministratorAccess` which will be acces by an Access key.
I have also researched the possibility of enabling MFA with aws-cli [here](https://aws.amazon.com/premiumsupport/knowledge-center/authenticate-mfa-cli/) & [here](https://stackoverflow.com/questions/34795780/how-to-use-mfa-with-aws-cli)

## Gitpod cloud development environment

managed the `.gitpod.yml` which containes the commands to install aws-cli each time we open the enviroment as it is ephemeral and also managed to make the enviroment variable for gitpod 
```bash
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_DEFAULT_REGION=us-east-1
```
## regarding the billing manipulation using AWS-cli tools

I have followed the steps required to create a cost and usage budget referenced [here](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/budgets/create-budget.html)
and created two json files, I have learned how to get a notified on your forcasted usage which will be very helpful in managaing budgets also the json files resembles the forms int the AWS GUI console and that it uses UNIX timestamp.

I have also managed to make an alarm for billing which will be triggered once the budget is reached. 

Contents of `budget.json`:
```json 
{
    "BudgetLimit": {
        "Amount": "5",
        "Unit": "USD"
    },
    "BudgetName": "Example Tag Budget",
    "BudgetType": "COST",
    "CostFilters": {
        "TagKeyValue": [
            "user:Key$value1",
            "user:Key$value2"
        ]
    },
    "CostTypes": {
        "IncludeCredit": true,
        "IncludeDiscount": true,
        "IncludeOtherSubscription": true,
        "IncludeRecurring": true,
        "IncludeRefund": true,
        "IncludeSubscription": true,
        "IncludeSupport": true,
        "IncludeTax": true,
        "IncludeUpfront": true,
        "UseBlended": false
    },
    "TimePeriod": {
        "Start": 1676419200,
        "End": 3706473600
    },
    "TimeUnit": "MONTHLY"
}
```

Contents of `notifications-with-subscribers.json`:
```Json

[
    {
        "Notification": {
            "ComparisonOperator": "GREATER_THAN",
            "NotificationType": "ACTUAL",
            "Threshold": 80,
            "ThresholdType": "PERCENTAGE"
        },
        "Subscribers": [
            {
                "Address": "pherkad133@gmail.com",
                "SubscriptionType": "EMAIL"
            }
        ]
    }
]
```

using this cli I was able to make the baudget of 5$ 

```bash
aws budgets create-budget --account-id $AWS_ACCOUNT_ID --budget file://aws/json/budget.json --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json
```

## billing alarm using gitpod bash terminal 

