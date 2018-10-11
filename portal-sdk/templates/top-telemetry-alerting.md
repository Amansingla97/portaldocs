{"gitdown": "contents"}

# Overview

The framework provides an out of the box alerting infrastructure. Some of these alerts you will be automatically onboarded to and others you will have to onboard manually.
They offer a great way to constantly assess your product and ensure you are within SLAs and not regressing within real time.
Each alert type has either configurable or fixed alerting criteria which is assessed on varying windows of time and then based on the results will the alert will be triggered or not.
Once an alert is triggered it will automatically open an IcM on the owning team.

## What alerting is available

There are number of framework provided alerts:

1. Extension SDK age
    - Sev3 IcM incdient for an extension when its SDK is older than 60 days
    - Sev2 for over 90 days
1. Extension alive
1. Performance
1. Availability
1. Client Error
1. Create regression

Some of these alert types are configurable per extension. The following alerts types currently support extension configuration:

1. [Performance](#performance)
1. [Availability](#Availability)
1. [Client Error](#client-error)
1. [Create regression](#create-regression)

To onboard to the configurable alerts please see the relevant sub section below.

Similarly to the non-configurable alerts, once the thresholds for any of the configured alerts are met or surpassed a ICM alert containing details will be opened agaisnt the owning team.

## Performance

The alerts can be configured for extension performance, blade performance and part performance on different environments including national clouds.

### Configuration

At a high level you define;

1. N number of environment within "environments" property like the below.
2. The performance configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true,
    "environments": [
        {
            "environment": ["portal.azure.com", "portal.azure.cn"],
            "availability": [...],
            "clientError": [...],
            "create": [...],
            "performance": [
                 {
                    "type": "extension",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                },
                {
                    "type": "blade",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                }
                ...
            ]
        },
        {
            "environment": ["ms.portal.azure.com"],
            "performance": [
                {
                    ...
                }
                ...
             ]
            ...
        }
        ...
    ]
    ...
}
```

Per each of those, you can define a set of criteria like the below.

> Only blade or part is required to have a namePath property or optionally to have an exclusion property.

#### What is environments

"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

#### What is environment

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or portal.azure.cn or canary.portal.azure.com 
or any other legit portal domain name, a.k.a., national cloud domain names are supported too. Mutiple values can be set for an "environment" property.

#### What is enabled

"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level 
depending on where it's located in customization json. For details, see "enabled" property in json snippet.

You can define N number of criteria like the below.

```json
{
    "severity": 3,
    "enabled": true,
    "percentile": 95,
    "percentileDurationThresholdInMilliseconds": 4000,
    "minAffectedUserCount": 10,
    "bottomMinAffectedUserCount": 2,
    "namePath": ["*"],
    "exclusion": [
        "Extension/Your_Extension_Name/Blade/BladeNameA",
        "Extension/Your_Extension_Name/Blade/BladeNameB"],
    "safeDeploymentStage": ["3"],
    "datacenterCode": ["AM"]
}
```

#### What is severity

This is the severity value that an IcM alert would have when an alert is fired.

#### What is percentile

This is at which percentile you want to measure the performance. Today the only options are 80 or 95.

#### What is percentileDurationThresholdInMilliseconds

This is the minimum duration (in milliseconds) when {percentile}% of users is above the {percentileDurationThresholdInMilliseconds}.

#### What is minAffectedUserCount

This is the minimum number of users whose load duration is above {percentileDurationThresholdInMilliseconds}.

#### What is bottomMinAffectedUserCount

This is used as a threshold to trigger an alert if the {percentile} defined is greater than or
equal to __double__ of the {percentileThreshold} defined.

> This is defaulted to 20% of {minAffectedUserCount}.

This is used to catch any unusual spikes on the weekends/low traffic periods.

#### What is namePath

This only applies to blades or parts and defines what blades or parts to alert on, you can either use an asterisk("*") sign to include 
all the blades or parts within your extension or specify a list of full blade or part names to alert on. The percentileDurationThresholdInMilliseconds, minAffectedUserCount and bottomMinAffectedUserCount specified in critiera are for individual blades or parts.

#### What is exclusion

This only applies to blades or parts and defines what blades or parts you wish to exclude.

#### What is safeDeploymentStage

Safe deployment stage can be "0", "1", "2", or "3". Each stage has a batch of regions. It does not support asterisk("*") sign.
Safe deployment stage is optional. If you don't specify the safe deployment stage property in critera, when alerting calculates percentileDuration and affectedUserCount, it does not take safe deployment stage into consideration. So you won't have percentileDuration and affectedUserCount per safe deployment stage. For such a case, percentileDurationThresholdInMilliseconds, minAffectedUserCount and bottomMinAffectedUserCount specified in critiera are for all(combined, overall) the safe deployment stages.
For the complete list of safe deployment stages and their regions, go to [https://aka.ms/portalfx/alerting/safe-deployment-stage][safe-deployment-stage]

#### What is datacenterCode

Datacenter code can be "`*`", "AM", "BY", etc. "`*`" represents all Azure Portal Production regions.
Datacenter code is optional. If you don't specify the datacenterCode property in critera, when alerting calculates percentileDuration and affectedUserCount, it does not take datacenter into consideration. So you won't have percentileDuration and affectedUserCount per datacenter. For such a case percentileDurationThresholdInMilliseconds, minAffectedUserCount and bottomMinAffectedUserCount specified in critiera are for all(combined, overall) the datacenters.
For the complete list of datacenter code names, go to [https://aka.ms/portalfx/alerting/datacenter-code-name][datacenter-code-name]

#### When do the alerts trigger

Every 10 minutes, we get percentile load duration for the last 90 minutes. We get the most recent 6 sample points and calculate a weighted percentile load duration based on the following formula.

```txt
Weighted duration = 8/24 * {most recent percentile load duration} + 6/24 * {2nd most recent percentile load duration} + 4/24 * {3rd…} + 3/24 * {4th …} + 2/24 * {5th …} + 1/24 * {6th …}
```

Alerts will only trigger when one of the following criteria is met.

1. Weighted duration is above {percentileDurationThresholdInMilliseconds} and affected user count is above {minAffectedUserCount}
1. Weighted duration is above 2 * {percentileDurationThresholdInMilliseconds} and affected user count is above {bottomMinAffectedUserCount}

### How often do they run

Currently performance alerts run every 10 minutes assessing the previous 90 minute of data.

## Availability

The alerts can be configured for extension availability, blade availability and part availability on different environments including national clouds. 

### Configuration

At a high level you define;

1. N number of environment within "environments" property like the below.
2. The availability configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true,
    "environments": [
        {
            "environment": ["portal.azure.com", "portal.azure.cn"],
            "availability": [
                {
                    "type": "extension",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                },
                {
                    "type": "blade",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                }
                ...
            ],
            "clientError": [...],
            "create": [...],
            "performance": [...]
        },
        {
            "environment": ["ms.portal.azure.com"],
            "performance": [
                {
                    ...
                }
                ...
             ]
            ...
        }
        ...
    ]
}
```

Per each of those, you can define a set of criteria like the below.

> Only blade or part is required to have a namePath property or optionally to have an exclusion property.

#### What is environments

"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

#### What is environment

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or portal.azure.cn or canary.portal.azure.com
or any other legit portal domain name, a.k.a., national cloud domain names are supported too. Mutiple values can be set for an "environment" property.

#### What is enabled

"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level
depending on where it's located in customization json. For details, see comments of "enabled" property in json snippet.

You can define N number of criteria like the below.

```json
{
    "severity": 3,
    "enabled": true,
    "minAvailability": 80.0,
    "minFailureCount": 10,
    "minFailureUserCount": 10,
    "namePath": ["*"],
    "exclusion": [
        "Extension/Your_Extension_Name/Blade/BladeNameA",
        "Extension/Your_Extension_Name/Blade/BladeNameB"],
    "safeDeploymentStage": ["3"],
    "datacenterCode": ["AM"]
}
```

#### What is severity

This is the severity value that an IcM alert would have when an alert is fired.

#### What is minAvailability

This is the minimum availability as a percentage. For example your extension fails to load 20 out of 100 times that would be 80% available.

#### What is minFailureCount

This is the minimum number of failures that have occurred, for example the above configuration requires 10 or more failures.

#### What is minFailureUserCount

This is the minimum number of unique users who have to encountered a failure before the threshold is surpassed.

#### What is namePath

This only applies to blades or parts and defines what blades or parts to alert on, you can either use an asterisk("*") sign to include
all the blades or parts within your extension or specify a list of full blade or part names to alert on. The minAvailability, minFailureCount and minFailureUserCount specified in critiera are for individual blades or parts.

#### What is exclusion

This only applies to blades or parts and defines what blades or parts you wish to exclude.

#### What is safeDeploymentStage

Safe deployment stage can be "0", "1", "2", or "3". Each stage has a batch of regions. It does not support asterisk("*") sign.
Safe deployment stage is optional. If you don't specify the safe deployment stage property in critera, when alerting calculates availability, failureCount and failureUserCount, it does not take safe deployment stage into consideration. So you won't have availability, failureCount and failureUserCount per safe deployment stage. For such a case, minAvailability, minFailureCount and minFailureUserCount specified in critiera are for all(combined, overall) the safe deployment stages. 
For the complete list of safe deployment stages and their regions, go to [https://aka.ms/portalfx/alerting/safe-deployment-stage][safe-deployment-stage]

#### What is datacenterCode

Datacenter code can be "`*`", "AM", "BY", etc. "`*`" represents all Azure Portal Production regions.
Datacenter code is optional. If you don't specify the datacenterCode property in critera, when alerting calculates availability, failureCount and failureUserCount, it does not take datacenter into consideration. So you won't have availability, failureCount and failureUserCount per datacenter. For such a case, minAvailability, minFailureCount and minFailureUserCount specified in critiera are for all(combined, overall) the datacenters.
For the complete list of datacenter code names, go to [https://aka.ms/portalfx/alerting/datacenter-code-name][datacenter-code-name]

### When do the alerts trigger

Alerts will only trigger for any blade in extension "Your_Extension_Name" except for blades "Extension/Your_Extension_Name/Blade/BladeNameA" and
"Extension/Your_Extension_Name/Blade/BladeNameB" and for the blade load on safe deployment stage 3 and datacenter "AM", when all 3 criteria,
minAvailability, minFailureCount and minFailureUserCount are met (AND). So the above critical configuration will only fire when 10 or more unique users encounter failures
*AND* there are 10 or more failure occurences *AND* the total availability < 80%, all within the last hour. Only then will a severity 3 alert be opened.

### How often do they run

Currently they run every 5 minutes assessing the previous hour of data.

## Client Error

There are two high level types of client error alerts, error percentage and error message on different environments including national clouds.

1. Error percentage alerts fire when the percentage of users experiencing any error(s) is above the defined threshold.
2. Error message alerts fire on specified error messages.

### Configuration

At a high level you define:

1. An environment for the alerts to run against. See definition [below](#client-error-configuration-what-is-environment)
2. The error configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true,
    "environments": [
        {
            "environment": ["portal.azure.com", "portal.azure.cn"],
            "availability": [...],
            "clientError": [
                {
                    "type": "message",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                },
                {
                    "type": "percentage",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                }
            ],
            "create": [...],
            "performance": [...],
        },
        {
            "environment": ["ms.portal.azure.com"],
            ...
            "clientError": [
                {
                    ...
                }
                ...
             ],
            ...
        }
        ...
    ]
    ...
}
```

#### What is environments

"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

#### What is environment

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or portal.azure.cn or canary.portal.azure.com 
or any other legit portal domain name, a.k.a., national cloud domain names are supported too. Mutiple values can be set for an "environment" property.

#### What is enabled

"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level 
depending on where it's located in customization json. For details, see "enabled" property in json snippet.

> Among "message" and "percentage" types, you can choose to have one type or two types. Per each of those, you can define a set of criteria like the below. You can define N number of criteria.

#### Percentage

An example of a percentage error alert criteria

> You can specify up to 3 messages in "exclusion" property. "type" property's supported value is "and" or "or".

```json
[
    {
        "type": "percentage",
        "enabled": true,
        "criteria":[
            {
                "severity": 3,
                "enabled": true,
                "minAffectedUserCount": 2,
                "minAffectedUserPercentage": 10.0,
                "exclusion": {
                    "type": "or",
                    "message1":"eastus2stage",
                    "message2":"eastus2(stage)"
                },
                "safeDeploymentStage": ["3"],
                "datacenterCode": ["AM"]
            },
            ...
        ]
    },
   ...
]
```

#### Message

An example of a message error alert criteria.

> You can specify up to 3 messages in one criteria and up to 3 messages in "exclusion" property. "type" property's supported value is "and" or "or".

```json
[
    {
        "type": "message",
        "criteria":[
            {
                "severity": 4,
                "enabled": true,
                "checkAllNullRefs": true,
                "message1": "Cannot read property",
                "message2": "of null",
                "minAffectedUserCount": 1,
                "exclusion": {
                    "type": "or",
                    "message1":"eastus2stage",
                    "message2":"eastus2(stage)"
                },
                "safeDeploymentStage": ["3"],
                "datacenterCode": ["AM"]
            },
            ...
        ]
    },
   ...
]
```

#### What is severity

This is the severity value that an IcM alert would have when an alert is fired.

#### What is minAffectedUserPercentage

This is the minimum number of percentage of users affected by any client error.

#### What is minAffectedUserCount

This is the minimum number of users affected by any client error.

#### What is checkAllNullRefs

When it's true, alert checks all the null refs client errors. You can still specify message1, message2, etc. They're additional conditions. 'checkAllNullRefs' property is optional.

#### What is message1, message2, message3 in criteria element for error message alerts

This is the error string that error message alerts check if it existis in client error logs, specifically in [message] column at (Client|Ext)Events log table. They're logical AND relations. To count as an error, all the messages that specified in criteria element have to be present in a client error message([message] column at (Client|Ext)Events log table). You can specify up to 3 messages in one criteria.

#### What is exclusion

This specifies condition(s) that alerts do not count as a client error. You can specify it for both error percentage and error message alerts.

#### What is message1, message2, message3 in the exclusion property

This is the error string(s) that alerts would not count it as a client error when they're present in a client error message([message] column at (Client|Ext)Events log table). You can specify up to 3 messages in "exclusion" property.

#### What is type in the exclusion property

This is the logical operator for messages in "exclusion" property. Its supported value is "and" or "or". "and" means when all the messages specified in "exclusion" property are present in a client error message, error alerts would not count it as a client error. "or" means when any of the messages specified in "exclusion" property is present in a client error message, error alerts would not count it as a client error.

#### What is safeDeploymentStage

Safe deployment stage can be "0", "1", "2", or "3". Each stage has a batch of regions. It does not support asterisk("*") sign.
Safe deployment stage is optional. If you don't specify the safe deployment stage property in critera, when alerting calculates affectedUserCount, affectedUserPercentage, it does not take safe deployment stage into consideration. So you won't have affectedUserCount or affectedUserPercentage per safe deployment stage. For such a case, minAffectedUserCount or minAffectedUserPercentage specified in critiera are for all(combined, overall) the safe deployment stages.
For the complete list of safe deployment stages and their regions, go to [https://aka.ms/portalfx/alerting/safe-deployment-stage][safe-deployment-stage]

#### What is datacenterCode

Datacenter code can be "`*`", "AM", "BY", etc. "`*`" represents all Azure Portal Production regions.
Datacenter code is optional. If you don't specify the datacenterCode property in critera, when alerting calculates affectedUserCount or affectedUserPercentage, it does not take datacenter into consideration. So you won't have affectedUserCount or affectedUserPercentage per datacenter. For such a case, minAffectedUserCount or minAffectedUserPercentage specified in critiera are for all(combined, overall) the datacenters. 
For the complete list of datacenter code names, go to [https://aka.ms/portalfx/alerting/datacenter-code-name][datacenter-code-name]

### How often client error alerts run

Currently error percentage alerts run every 15 minutes and error message alerts run every 5 minutes assessing the previous 60 minute of data.

## Create regression

The alerts can be configured for create blade extension on different environments including national clouds.

### Configuration

At a high level you define;

1. N number of environment within "environments" property like the below.
2. The create configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true,
    "environments": [
        {
            "environment": ["portal.azure.com", "portal.azure.cn"],
            "availability": [...],
            "clientError": [...],
            "create": [
                 {
                    "type": "regression",
                    "enabled": true,
                    "criteria": [
                       ...
                    ]
                }
            ],
            "performance": [...],
        },
        {
            "environment": ["ms.portal.azure.com"],
            "create": [
                {
                    ...
                }
                ...
             ]
            ...
        }
        ...
    ]
    ...
}
```

#### What is environments

"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

#### What is environment

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or portal.azure.cn or canary.portal.azure.com
or any other legit portal domain name, a.k.a., national cloud domain names are supported too. Mutiple values can be set for an "environment" property.

#### What is enabled

"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level
depending on where it's located in customization json. For details, see "enabled" property in json snippet.

You can define N number of criteria like the below.

```json
{
    "severity": 3,
    "enabled": true,
    "bladeName": ["CreateBlade"],
    "minSuccessRateOverPast24Hours":94.0,
    "minSuccessRateOverPastHour":94.0,
    "minTotalCountOverPast24Hours":50,
    "minTotalCountOverPastHour":3
}
```

#### What is severity

This is the severity value that an IcM alert would have when an alert is fired.

#### What is bladeName

The list of the create blade name.

#### What is minSuccessRateOverPast24Hours

This is the minimum create blade success rate over the past 24 hours.

#### What is minSuccessRateOverPastHour

This is the minimum create blade success rate over the past hour.

#### What is minTotalCountOverPast24Hours

This is the minimum number of create that gets kicked off over the past 24 hours.

#### What is minTotalCountOverPastHour

This is the minimum number of create that gets kicked off over the past hour.

### How often do create alerts run

Every 60 minutes, we get create successRate and create totalCount for the last 60 minutes and the last 24 hours.

Alerts will only trigger when the following criteria are met.

1. Hourly create successRate is below {minSuccessRateOverPastHour} and hourly create totalcount is above {minTotalCountOverPastHour}
1. 24-hour create successRate is below {minSuccessRateOverPast24Hours} and 24-hour create totalcount is above {minTotalCountOverPast24Hours}

## Fequently asked questions

### How do I onboard

1. Generate the desired per extension configuration
    - This can be done by manually editing the JSON file.
1. Fill out the following work item [https://aka.ms/portalfx/alerting-onboarding][alerting-onboarding] and attach configuration JSON
1. Set up correlation rules in ICM

| Field | Value |
| -----  | ----- |
| Routing ID | 'AIMS://AZUREPORTAL\Portal\\{ExtensionName}' |
| Correlation ID | use table below to map |
| Mode | Hit count (recommended) |
| Match DC/Region | Checked |
| Match Slice | Checked |
| Match Severity | Checked |
| Match Role | Checked |
| Match Instance/Cluster | Checked |

> Depending on the alert you're correlating you will need to use the corresponding correlation id

| Alert | Correlation ID |
| ----- | -------------- |
| Create - Regression | CreateBladeSuccessRate |
| Error - AffectedUserPercentage | ErrorAffectedUserPercentage |
| Error - Message | ErrorMessage |
| Availability - Extension | ExtensionLoadAvailability |
| Availability - Blade | BladeLoadAvailability |
| Availability - Part | PartLoadAvailability |
| Performance - Extension | ExtensionLoadPerformance |
| Performance - Blade | BladeLoadPerformance |
| Performance - Part | PartLoadPerformance|

### How do I know my extension's current configuration

Click the [this link][alerting-extension-customization] and replace `HubsExtension` with `YOUR_EXTENSION_NAME` and run Kusto function, GetExtensionCustomizationJson. Or go to [https://azportal.kusto.windows.net/Partner][kusto-partner-database] to open Kusto.Explorer and run Kusto function, 
GetExtensionCustomizationJson("YOUR_EXTENSION_NAME"). The regex is supported. You can view alert customization of onboarded extensions. The extension alert customization only exists once you have onboarded to the alerting infrastructure.
> The customizaztion has a daily sync from the SQL database at 5:00 pm PST.

### What happens if I need to update my configuration

1. Contact [ibizafxhot](mailto:ibizafxhot@microsoft.com;azurefxg@microsoft.com),
1. State in the email which configuration you require to be updated and attached the updated configuration
1. We will respond as soon as possible and apply the updates

### How is mapping done from extension name to IcM team and service names

Azure Portal partner team's IcM info is collected during parnter onboarding process and is stored in the [extension config](https://aka.ms/portalfx/extensionsprodjson). An IcM routing rule is added under Azure Portal (Ibiza) service in IcM to route incidents to corresponding partners.

> The IcM routing rule is in format 'AIMS://AZUREPORTAL\Portal\{ExtensionName}'.

### How to enable alerts for national clouds

Alerts are supported in national clouds. Specify the national cloud portal domain names in "environment" property. You can use the same criteria for national clouds or different set of criteria.The national cloud domain names are "portal.azure.cn", "portal.azure.us", "portal.microsoftazure.de". You can use any legit national cloud domain name, for instance, "aad.portal.azrue.cn".

```json
{
   ...
    "environments": [
        {
            "environment": ["portal.azure.com", "ms.portal.azure.com", "portal.azure.cn"],
            ...
        },
        {
            "environment": ["portal.azure.cn","portal.azure.us", "portal.microsoftazure.de"],
            ...
        },
        {
            "environment": ["portal.azure.us", "portal.microsoftazure.de"],
            ...
        }
        ...
    ]
    ...
}
```

[alerting-onboarding]: https://aka.ms/portalfx/alerting-onboarding
[alerting-kusto-partner]: https://ailoganalyticsportal-privatecluster.cloudapp.net/clusters/azportal.kusto.windows.net/databases/Partner?q=H4sIAAAAAAAEAEvOKS0uSS3SUHesKsgvKknMUdfUS0ksSUxKLE7VUApILCrJSy1S0tRzSU1LLM0pcS7KBKrOTNTQBABHZQn9OQAAAA%3d%3d

[datacenter-code-name]: https://aka.ms/portalfx/alerting/datacenter-code-name
[safe-deployment-stage]: https://aka.ms/portalfx/alerting/safe-deployment-stage
[alerting-onboarding]: https://aka.ms/portalfx/alerting-onboarding
[alerting-extension-customization]: https://azportal.kusto.windows.net/Partner?query=GetExtensionCustomizationJson%28%5C%22HubsExtension%5C%22%29
[kusto-partner-database]: https://azportal.kusto.windows.net/Partner