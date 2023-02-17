# Week 0 — Billing and Architecture


## Regarding the architectural diagram the CI/CD logical pipeline in Lucid Charts



## regarding the billing manipulation using AWS-cli tools

I have followed the steps required to create a cost and usage budget referenced [here](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/budgets/create-budget.html)
and created two json files 

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
