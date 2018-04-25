# Availability

The alerts can be configured for extension availability, blade availability and part availability on different environments including national clouds. 

## Configuration

At a high level you define;

1. N number of environment within "environments" property like the below.
2. The availability configuration for the alerts within that environment

```json
{
    "extensionName": "Your_Extension_Name",
    "enabled": true, // Enable or disable alerts for Your_Extension_Name.
    "environments": [
        {
            "environment": ["portal.azure.com", "portal.azure.cn"], // National clouds are supported.
            "availability": [
                {
                    "type": "extension", // Support value, "extension", "blade" or "part".
                    "enabled": true, // Enable or disable extension type alerts for Your_Extension_Name.
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
            "clientError": [...], // Optional. Add it when you want to enable client error alerts.
            "create": [...], // Optional. Add it when you want to enable create alerts.
            "performance": [...] // Optional. Add it when you want to enable performance alerts.
            
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

### What is environments?
"environments" property is an array. Each of its elements represents a set of alerting critiera for an environment.

### What is environment?

"environment" property is an array. Its supported value is portal.azure.com or ms.portal.azure.com or portal.azure.cn or canary.portal.azure.com 
or any other legit portal domain name, a.k.a., national cloud domain names are supported too. Mutiple values can be set for an "environment" property.

### What is enabled?
"enabled" property is used to enable (when "enabled" is true) or disable ("enabled" is false) alerts on various level 
depending on where it's located in customization json. For details, see comments of "enabled" property in json snippet.

You can define N number of criteria like the below.

```json
{
    "severity": 3, // Support value 0, 1, 2, 3 or 4.
    "enabled": true, // Enable or disable this criteria.
    "minAvailability": 80.0,
    "minFailureCount": 10,
    "minFailureUserCount": 10,
    "namePath": ["*"], // Only support for blade or part type.
    "exclusion": [
        "Extension/Your_Extension_Name/Blade/BladeNameA",
        "Extension/Your_Extension_Name/Blade/BladeNameB"], // Only support for blade or part type.
    "safeDeploymentStage": ["3"], // Optional. It does not support asterisk("*") sign.
    "datacenterCode": ["AM"] // Optional.
}
```
### What is severity?

This is the severity value that an IcM alert would have when an alert is fired.

### What is minAvailability?

This is the minimum availability as a percentage. For example your extension fails to load 20 out of 100 times that would be 80% available.

### What is minFailureCount?

This is the minimum number of failures that have occurred, for example the above configuration requires 10 or more failures.

### What is minFailureUserCount?

This is the minimum number of unique users who have to encountered a failure before the threshold is surpassed.

### What is namePath?

This only applies to blades or parts and defines what blades or parts to alert on, you can either use an asterisk("*") sign to include 
all the blades or parts within your extension or specify a list of full blade or part names to alert on. The minAvailability, minFailureCount and minFailureUserCount specified in critiera are for individual blades or parts. 

### What is exclusion?

This only applies to blades or parts and defines what blades or parts you wish to exclude.

### What is safeDeploymentStage?

Safe deployment stage can be "0", "1", "2", or "3". Each stage has a batch of regions. It does not support asterisk("*") sign.
Safe deployment stage is optional. If you don't specify the safe deployment stage property in critera, when alerting calculates availability, failureCount and failureUserCount, it does not take safe deployment stage into consideration. So you won't have availability, failureCount and failureUserCount per safe deployment stage. For such a case, minAvailability, minFailureCount and minFailureUserCount specified in critiera are for all(combined, overall) the safe deployment stages. 
For the complete list of safe deployment stages and their regions, go to [https://aka.ms/portalfx/alerting/safe-deployment-stage][safe-deployment-stage]

### What is datacenterCode?

Datacenter code can be "`*`", "AM", "BY", etc. "`*`" represents all Azure Portal Production regions.
Datacenter code is optional. If you don't specify the datacenterCode property in critera, when alerting calculates availability, failureCount and failureUserCount, it does not take datacenter into consideration. So you won't have availability, failureCount and failureUserCount per datacenter. For such a case, minAvailability, minFailureCount and minFailureUserCount specified in critiera are for all(combined, overall) the datacenters. 
For the complete list of datacenter code names, go to [https://aka.ms/portalfx/alerting/datacenter-code-name][datacenter-code-name]

### When do the alerts trigger?

Alerts will only trigger for any blade in extension "Your_Extension_Name" except for blades "Extension/Your_Extension_Name/Blade/BladeNameA" and 
"Extension/Your_Extension_Name/Blade/BladeNameB" and for the blade load on safe deployment stage 3 and datacenter "AM", when all 3 criteria, 
minAvailability, minFailureCount and minFailureUserCount are met (AND). So the above critical configuration will only fire when 10 or more unique users encounter failures 
*AND* there are 10 or more failure occurences *AND* the total availability < 80%, all within the last hour. Only then will a severity 3 alert be opened.

### Is National Cloud Supported?
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

## How often do they run?

Currently they run every 5 minutes assessing the previous hour of data.

## How do I onboard?

1. Generate the desired per extension configuration
    - This can be done by manually editing the JSON file.
1. Fill out the following work item [https://aka.ms/portalfx/alerting-onboarding][alerting-onboarding] and attach configuration JSON
1. Set up correlation rules in ICM

| Field | Value |
| -----  | ----- |
| Routing ID | 'AIMS://AZUREPORTAL\Portal\\{ExtensionName}' |
| Correlation ID | Specific to alert, use table below to map |
| Mode | Hit count (recommended) |
| Match DC/Region | Checked |
| Match Slice | Checked |
| Match Severity | Checked |
| Match Role | Checked |
| Match Instance/Cluster | Checked |


| Alert | Correlation ID |
| ----- | -------------- |
| Availability - Extension | ExtensionLoadAvailability |
| Availability - Blade | BladeLoadAvailability |
| Availability - Part | PartLoadAvailability | 
 
## How do I know my extension's current customization?

Click the [this link][alerting-extension-customization] and replace `HubsExtension` with `YOUR_EXTENSION_NAME` and run Kusto function, GetExtensionCustomizationJson. Or go to [https://azportal.kusto.windows.net/Partner][kusto-partner-database] to open Kusto.Explorer and run Kusto function, 
GetExtensionCustomizationJson("YOUR_EXTENSION_NAME"). The regex is supported. You can view alert customization of onboarded extensions. The extension alert customization only exists once you have onboarded to the alerting infrastructure.
> The customizaztion has a daily sync from the SQL database at 5:00 pm PST.

## What happens if I need to update them?

1. Contact [ibizafxhot](mailto:ibizafxhot@microsoft.com) and attached the updated configuration
1. We will respond as soon as possible and apply the updates

[datacenter-code-name]: https://aka.ms/portalfx/alerting/datacenter-code-name
[safe-deployment-stage]: https://aka.ms/portalfx/alerting/safe-deployment-stage
[alerting-onboarding]: https://aka.ms/portalfx/alerting-onboarding
[alerting-extension-customization]: https://azportal.kusto.windows.net/Partner?query=GetExtensionCustomizationJson%28%5C%22HubsExtension%5C%22%29
[kusto-partner-database]: https://azportal.kusto.windows.net/Partner