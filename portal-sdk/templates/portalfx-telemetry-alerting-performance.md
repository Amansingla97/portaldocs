# Performance

The alerts can be configured for extension performance, blade performance and part performance on different environments.

## Configuration

At a high level you define;

1. N number of environment within "environments" property like the below.
2. The performance configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true,
    "environments": [
        {
            "environment": ["portal.azure.com"],
            "availability": [...], // Optional. Have it when you want to enable availability alerts.
            "performance": [
                 {
                    "type": "extension", // Support value, "extension", "blade" or "part"
                    "enabled": true, // Enable or disable extension type alerts for Your_Extension_Name
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
            "clientError": [...] // Optional. Have it when you want to enable Client Error alerts.
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

### What is environments?
"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

### What is environment?

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or canary.portal.azure.com 
or any other legit portal domain name. Mutiple values can be set for an "environment" property.

### What is enabled?
"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level 
depending on where it's located in customization json. For details, see "enabled" property in json snippet.

You can define N number of criteria like the below.

```json
{
    "severity": 3, // Support value 0, 1, 2, 3 or 4
    "enabled": true, // Enable or disable this criteria
    "percentile": 95, // Support value 80 or 95.
    "percentileDurationThresholdInMilliseconds": 4000,
    "minAffectedUserCount": 10,
    "bottomMinAffectedUserCount": 2,
    "namePath": ["*"], // Only support for blade or part type
    "exclusion": [
        "Extension/Your_Extension_Name/Blade/BladeNameA",
        "Extension/Your_Extension_Name/Blade/BladeNameB"], // Only support for blade or part type
    "safeDeploymentStage": ["3"], // Optional. It does not support asterisk("*") sign
    "datacenterCode": ["AM"] // Optional
}
```
### What is severity?

This is the severity value that an IcM alert would have when an alert is fired.

### What is percentile?

This is at which percentile you want to measure the performance. Today the only options are 80 or 95.

### What is percentileDurationThresholdInMilliseconds?

This is the minimum duration (in milliseconds) when {percentile}% of users is above the {percentileDurationThresholdInMilliseconds}.

### What is minAffectedUserCount?

This is the minimum number of users whose load duration is above {percentileDurationThresholdInMilliseconds}.

### What is bottomMinAffectedUserCount?

This is used as a threshold to trigger an alert if the {percentile} defined is greater than or
equal to __double__ of the {percentileThreshold} defined.

> This is defaulted to 20% of {minAffectedUserCount}.

This is used to catch any unusual spikes on the weekends/low traffic periods.

### What is namePath?

This only applies to blades or parts and defines what blades or parts to alert on, you can either use an asterisk("*") sign to include 
all the blades or parts within your extension or specify a list of full blade or part names to alert on.

### What is exclusion?

This only applies to blades or parts and defines what blades or parts you wish to exclude.

### What is safeDeploymentStage?

Safe deployment stage can be "0", "1", "2", or "3". Each stage has a batch of regions. It does not support asterisk("*") sign.
Safe deployment stage is optional. Without it, the alert looks at all the stages.
For the complete list of safe deployment stages and their regions, go to [https://aka.ms/portalfx/alerting/safe-deployment-stage][safe-deployment-stage]

### What is datacenterCode?

Datacenter code can be "`*`", "AM", "BY", etc. "`*`" represents all Azure Portal Production regions.
Datacenter code is optional. Without it, the alert looks at all the datacenters.
For the complete list of datacenter code names, go to [https://aka.ms/portalfx/alerting/datacenter-code-name][datacenter-code-name]

### When do the alerts trigger?

Every 10 minutes, we get percentile load duration for the last 90 minutes. We get the most recent 6 sample points and calculate a weighted percentile load duration based on the following formula.

```
Weighted duration = 8/24 * {most recent percentile load duration} + 6/24 * {2nd most recent percentile load duration} + 4/24 * {3rd…} + 3/24 * {4th …} + 2/24 * {5th …} + 1/24 * {6th …}
```

Alerts will only trigger when one of the following criteria is met.

1. Weighted duration is above {percentileDurationThresholdInMilliseconds} and affected user count is above {minAffectedUserCount}
1. Weighted duration is above 2 * {percentileDurationThresholdInMilliseconds} and affected user count is above {bottomMinAffectedUserCount}

## How often do they run?

Currently performance alerts run every 10 minutes assessing the previous 90 minute of data.

## How do I onboard?

1. Generate the desired per extension configuration
    - This can be done by manually editing the JSON file.
1. Fill out the following work item [https://aka.ms/portalfx/alerting-onboarding][alerting-onboarding] and attach configuration JSON
1. Set up correlation rules in ICM


| Field | Value |
| -----  | ----- |
| Routing ID | For all others 'AIMS://AZUREPORTAL\Portal\\{ExtensionName}' |
| Correlation ID | Specific to alert, use table below to map |
| Mode | Hit count (recommended) |
| Match DC/Region | Checked |
| Match Slice | Checked |
| Match Severity | Checked |
| Match Role | Checked |
| Match Instance/Cluster | Checked |


| Alert | Correlation ID |
| ----- | -------------- |
| Performance - Extension | ExtensionLoadPerformance |
| Performance - Blade | BladeLoadPerformance |
| Performance - Part | PartLoadPerformance|

## What happens if I need to update them or how do I know my extension's current configuration?

1. Contact [ibizafxhot](mailto:ibizafxhot@microsoft.com) and attached the updated configuration
1. We will respond as soon as possible and apply the updates

[datacenter-code-name]: https://aka.ms/portalfx/alerting/datacenter-code-name
[safe-deployment-stage]: https://aka.ms/portalfx/alerting/safe-deployment-stage
[alerting-onboarding]: https://aka.ms/portalfx/alerting-onboarding