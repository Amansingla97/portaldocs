﻿
## Overview

The Azure portal offers three ways to build a create form:

1. Deploy to Azure

    These are simple forms that are auto-generated from an ARM template and that contain basic controls and validation. It is also  used to build create forms for community templates as specified in  [https://aka.ms/portalfx/quickstarttemplates](https://aka.ms/portalfx/quickstarttemplates),  and integrate with the Marketplace, as specified in . However, it is very limited in terms of what is available for validation and controls.

1. Solution templates

    These are simple forms that are specified in a JSON document that is separate from the template. Solution templates can be used by any service, or for IaaS-focused ARM templates, as specified in [https://github.com/Azure/azure-marketplace/wiki](https://github.com/Azure/azure-marketplace/wiki). However, there are some limitations on supported controls.

1. Custom create forms

    These are fully-customized forms that developed in  **TypeScript** for Azure Portal extensions. Most teams build custom create forms for full flexibility in the UI and validation. 

This document contains information about building custom create forms, including ARM dropdowns, automation, and validation. The following is a list of subtopics in blade creation.

| Type                          | Document           | Description |
| ----------------------------- | ---- | ---- |
| Deploy to Azure                 | [portalfx-create-deploytoazure.md](portalfx-create-deploytoazure.md) | This is the quickest way to build a `Create` form. |
| Testing Guidance     | [portalfx-test.md](portalfx-test.md) |  Information on building form tests. |
| Feedback |  [portalfx-telemetry.md](portalfx-telemetry.md) | Create form abandonment and other topics. |
| Building forms | [top-extensions-forms.md](top-extensions-forms.md) | Customize the form. |
| Form controls | [top-extensions-controls.md](top-extensions-controls.md) | Built-in form fields, like TextField and DropDown. |
| Parameter collection  | [portalfx-parameter-collection-overview.md](portalfx-parameter-collection-overview.md) | Collectors and providers that enable an extension's UX to collect data from the user | 
| Telemetry | [portalfx-telemetry-create.md](portalfx-telemetry-create.md) | Additional information on usage dashboards and queries. |
| Troubleshooting Guide | [portalfx-create-troubleshooting.md](portalfx-create-troubleshooting.md) | Additional debugging information. |

### Building custom create forms

**NOTE**: In this discussion, `<dir>` is the `SamplesExtension\Extension\` directory, and  `<dirParent>` is the `SamplesExtension\` directory, based on where the samples were installed when the developer set up the SDK. 

Start by building a template blade. All create experiences should be designed for a single blade. Then, add a provider component by using the parameter collection framework, which is a platform that enables the extension to collect data from the user.

In most cases, the blade is launched from the Marketplace, or a toolbar command like `Create` from the Browse menu. Toolbar commands act as data collectors, and consequently, the blade is expected to act as a provider, as in the code located at `<dir>\Client\V1\ParameterCollection\ParameterProviders\ViewModels\ProviderViewModels.ts`.

After the provider component is installed, the developer should add a provisioner component to supply data to the provider, as specified in [portalfx-parameter-collection-overview.md](portalfx-parameter-collection-overview.md).

<!-- TODO:  Determine whether section marks can be added to these samples for gitHub. -->

If the resource is being created by deploying a single template to ARM, the extension should use an ARM provisioner. Otherwise, you need to use a custom provisioner to implement your custom deployment process.

1. An example of ARM provisioning is located at `<dir>\Client\V1\Controls\Chart\ViewModels\CreateEngineBladeViewModel.ts`.

1. An example of custom provisioning is in the RobotV3 sample that is located at  `<dir>\Client\V1\Create\RobotV3\ViewModels\CreateRobotBladeViewModel.ts`.

**NOTE**: Dropdown controls are recommended instead of picker controls that allow selecting items from a list in a child blade. Also, avoid using selectors, like form fields that open child blades.

Reach out to <a href="mailto:ibizafxpm@microsoft.com?subject=Full-screen Create&body=I  have some questions about the current state of full-screen create experiences.">Ibiza FX PM</a> if you have any questions about the current state of full-screen create experiences.

### Standard ARM fields

All ARM subscription resources require [subscription](#subscriptions-dropdown), [resource groups](#resource-groups -dropdown), [location](#locations-dropdown) and pricing dropdowns. The Portal offers built-in controls for each of these, as demonstrated in `<dir>\Client\V1\Create\EngineV3\ViewModels\CreateEngineBladeViewModel.ts` .

Each of these fields will retrieve values from the server and populate a dropdown with them. To set the value of these dropdowns, the extension looks up the value from the `fetchedValues` array, and then sets the `value` observable, as in the following code.

```ts
locationDropDown.value(locationDropDown.fetchedValues().first((value)=> value.name === "centralus"))
```

#### Subscriptions dropdown

The following dropdown does not use an `EditScope`.

```ts
import * as SubscriptionDropDown from "Fx/Controls/SubscriptionDropDown";
```

The following code contains a `SubscriptionDropDown`. 

 {"gitdown": "include-section", "file":"../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#subscriptionDropDown"}

#### Resource groups dropdown

The following dropdown does not use an `EditScope`.

```ts
import * as ResourceGroupDropDown from "Fx/Controls/ResourceGroupDropDown";
```

The following code contains a `ResourceGroupDropDown`. 
 
 {"gitdown": "include-section", "file":"../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#resourceGroupDropDown"}

#### Locations dropdown

The following dropdown does not use an `EditScope`.

```ts
import * as LocationDropDown from "Fx/Controls/LocationDropDown";
```

The following code contains a `ResourceGroupDropDown`. 

 {"gitdown": "include-section", "file":"../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#locationDropDown"}

<!-- TODO: Determine the location of the pricing dropdown so that it can be added to these examples. -->

### ARM dropdown options

Each ARM dropdown can [disable](#the-disable-method), [hide](#the-hide-method), [group](#the-group-method), and [sort](#the-sort-method).
 
#### The disable method

This is the preferred method of displaying values from ARM that are disabled, because it reduces support calls. The disable callback runs for each value that is fetched from ARM. The return value of the callback contains the reason  why the value is disabled. The values are displayed in groups at the bottom of the dropdown list, with the reason as the group header.  If no reason is provided, then the value is not disabled. This ensures the customer has information about why they can not select an option, as in the following example.

<!-- TODO:  Determine whether this sample causes gitHub to stop. -->

{"gitdown": "include-section", "file": "../../../src/SDK/Framework.Tests/TypeScript/Tests/Controls/Forms/DropDown.Subscription.test.ts", "section": "config#disable"}

#### The hide method

This is an alternative method of disallowing values from ARM. The hide callback runs for each value that is fetched from ARM. The return value of the callback contains a boolean that specifies whether the value should be hidden. If the extension hides the value, the required message telling the user why the values are hidden is displayed, as in the following example.

<!-- TODO:  Determine whether this sample causes gitHub to stop. -->

{"gitdown": "include-section", "file": "../../../src/SDK/Framework.Tests/TypeScript/Tests/Controls/Forms/DropDown.Subscription.test.ts", "section": "config#hide"}

**NOTE**: It is recommended to use the `disable` option so that scenario-specific detail about the dropdown value  can be displayed. Customers can see that a specific value is not available instead of hiding it, therefore reducing the potential for negative reactions when the values cannot be visually located.  In extreme cases, missing data  may trigger blade incidents.

#### The group method

This method groups together the values in the dropdown. The group callback receives a value from the dropdown and returns a display string that specifies which group should contain the value. If no display string or an empty display string is provided, then the value defaults to the top level of the group dropdown.
 
If the extension should sort the groups, as opposed to the values within the group, it can supply the `sort` option.  This is a conventional comparator function that determines the sort order by returning a number greater or less than zero. It defaults to alphabetical sorting, as in the following example.
 
 <!-- TODO:  Determine whether this sample causes gitHub to stop. -->

{"gitdown": "include-section", "file": "../../../src/SDK/Framework.Tests/TypeScript/Tests/Controls/Forms/DropDown.Subscription.test.ts", "section": "config#group"}
 
If the extension uses both disable and group, values that are disabled are displayed in the disabled group instead of in the group specified by the display string. 
 
#### The sort method

The `sort` option, is a conventional comparator function that returns a number greater or less than zero. It defaults to alphabetical sorting, based on the display string of the value, as in the following example.
 
 <!-- TODO:  Determine whether this sample causes gitHub to stop. -->

{"gitdown": "include-section", "file": "../../../src/SDK/Framework.Tests/TypeScript/Tests/Controls/Forms/DropDown.Subscription.test.ts", "section": "config#sort"}
 
If the extension combines the `sort` option with the `disable` or `group` functionality, the sort will operate  inside of the groups provided.

### EditScope-accessible dropdowns

**NOTE**:  EditScopes are becoming obsolete.  It is recommended that extensions be developed without edit scopes, as specified in [portalfx-editscopeless-procedure.md](portalfx-editscopeless-procedure.md). For more information about forms without editScopes, see  [portalfx-editscopeless-overview.md](portalfx-editscopeless-overview.md) and [portalfx-controls-dropdown.md#migration-to-the-new-dropdown](portalfx-controls-dropdown.md#migration-to-the-new-dropdown).

For scenarios where a form is built with an `EditScope`, the Portal provides versions of accessible ARM dropdowns that replace the legacy, non-accessible dropdowns. The accessible controls have minimal API changes and are simple to integrate into existing blades or parts. However, the new dropdowns are based on a new accessible control that no longer supports the following options.

| Option                               | Alternative                                 |
| ------------------------------------ | ------------------------------------------- |
| `cssClass`                           | none                                        |
| `dropDownWidth`                      | none                                        |
| `filterOptions`                      | none                                        |
| `hideValidationCheck`                | none                                        |
| `iconLookup`                         | none                                        |
| `iconSize`                           | none                                        |
| `infoBalloonContent`                 | none                                        |
| `inputAlignment`                     | none                                        |
| `labelPosition`                      | The `label` option accepts html             |
| `popupAlignment`                     | none                                        |
| `showValidationMessagesBelowControl` | none                                        |
| `subLabel`                           | The `label` option accepts html             |
| `subLabelPosition`                   | The `label` option accepts html             |
| `telemetryKeys`                      | none                                        |
| `viewModelValueChangesAreClean`      | none                                        |
| `visible`                            | The `visible` binding in the  html template |

The following sections contain code that may resemble your  implementation of the legacy controls. They also contain code that can replace the legacy controls in the implementation.

#### Subscriptions dropdown

The following code for subscription dropdowns should be updated to use the new controls instead of `EditScopes`. 

* Old code

    ```ts
    import SubscriptionsDropDown = MsPortalFx.Azure.Subscriptions.DropDown;
    // The subscriptions drop down.
    var subscriptionsDropDownOptions: SubscriptionsDropDown.Options = {
        options: ko.observableArray([]),
        form: this,
        accessor: this.createEditScopeAccessor((data) => {
            return data.subscription;
        }),
        validations: ko.observableArray<MsPortalFx.ViewModels.Validation>([
            new MsPortalFx.ViewModels.RequiredValidation(ClientResources.selectSubscription)
        ]),
        // Providing a list of resource providers (NOT the resource types) makes sure that when
        // the deployment starts, the selected subscription has the necessary permissions to
        // register with the resource providers (if not already registered).
        // Example: Providers.Test, Microsoft.Compute, etc.
        resourceProviders: ko.observable([resourceProvider]),
        // Optional -> You can pass the gallery item to the subscriptions drop down, and the
        // the subscriptions will be filtered to the ones that can be used to create this
        // gallery item.
        filterByGalleryItem: this._galleryItem
    };
    this.subscriptionsDropDown = new SubscriptionsDropDown(container, subscriptionsDropDownOptions);
    ```

* New code

    The following code for subscription dropdowns demonstrates the use of  the new controls.

    With the new, accessible Subscription dropdown, you'll change your code to look something like:
    ```ts
    import * as SubscriptionDropDown from "FxObsolete/Controls/SubscriptionDropDown";
    ```

    <!-- TODO:  Determine whether this sample causes gitHub to stop. -->

   {"gitdown": "include-section", "file": "../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#subscriptionDropDown"}

#### Resource groups legacy dropdown

The following code for Resource Group dropdowns should be updated to use the new controls instead of `EditScopes`. 

* Old code

    ```ts
    import ResourceGroupsDropDown = MsPortalFx.Azure.ResourceGroups.DropDown;
    // The resource group drop down.
    var resourceGroupsDropDownOptions: ResourceGroups.DropDown.Options = {
        options: ko.observableArray([]),
        form: this,
        accessor: this.createEditScopeAccessor((data) => {
            return data.resourceGroup;
        }),
        label: ko.observable<string>(ClientResources.resourceGroup),
        subscriptionIdObservable: this.subscriptionsDropDown.subscriptionId,
        validations: ko.observableArray<MsPortalFx.ViewModels.Validation>([
            new MsPortalFx.ViewModels.RequiredValidation(ClientResources.selectResourceGroup),
            new MsPortalFx.Azure.RequiredPermissionsValidator(requiredPermissionsCallback)
        ]),
        // This is no longer supported. Set the `value` option to initial desired value
        defaultNewValue: "NewResourceGroup",
        // Optional -> RBAC permission checks on the resource group. Here, we're making sure the
        // user can create an engine under the selected resource group, but you can add any actions
        // necessary to have permissions for on the resource group.
        requiredPermissions: ko.observable({
            actions: actions,
            // Optional -> You can supply a custom error message. The message will be formatted
            // with the list of actions (so you can have {0} in your message and it will be replaced
            // with the array of actions).
            message: ClientResources.enginePermissionCheckCustomValidationMessage.format(actions.toString())
        })
    };
    this.resourceGroupDropDown = new ResourceGroups.DropDown(container, resourceGroupsDropDownOptions);
    ```

* New code

    The following code for accessible Resource dropdowns demonstrates the use of  the new controls.

    ```ts
    import * as ResourceGroupDropDown from "FxObsolete/Controls/ResourceGroupDropDown";
    ```

    <!-- TODO:  Determine whether this sample causes gitHub to stop. -->

   {"gitdown": "include-section", "file": "../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#resourceGroupDropDown"}

#### Locations **legacy** dropdown

The following code for Location dropdowns should be updated to use the new controls instead of `EditScopes`. 

* Old code

    ```ts
    import LocationsDropDown = MsPortalFx.Azure.Locations.DropDown;
    // The locations drop down.
    var locationsDropDownOptions: LocationsDropDown.Options = {
        options: ko.observableArray([]),
        form: this,
        accessor: this.createEditScopeAccessor((data) => {
            return data.location;
        }),
        subscriptionIdObservable: this.LocationDropDown.subscriptionId,
        resourceTypesObservable: ko.observable([resourceType]),
        validations: ko.observableArray<MsPortalFx.ViewModels.Validation>([
            new MsPortalFx.ViewModels.RequiredValidation(ClientResources.selectLocation)
        ])
        // Optional -> This is no longer supported. Use the `disable` option (illustrated below).
        // filter: {
        //     allowedLocations: {
        //         locationNames: [ "centralus" ],
        //         disabledMessage: "This location is disabled for demo purporses (not in allowed locations)."
        //     },
        //     OR -> disallowedLocations: [{
        //         name: "westeurope",
        //         disabledMessage: "This location is disabled for demo purporses (disallowed location)."
        //     }]
        // }
    };
    ```
* New code

    The following code for Location dropdowns demonstrates the use of  the new controls.

    ```ts
    import * as LocationDropDown from "FxObsolete/Controls/LocationDropDown";
    ```

    <!-- TODO:  Determine whether this sample causes gitHub to stop. -->

   {"gitdown": "include-section", "file": "../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#locationDropDown"}

#### Pricing dropdown

The following code for Pricing dropdowns demonstrates the use of  the new controls.

```ts
*`MsPortalFx.Azure.Pricing.DropDown`

import * as Specs from "Fx/Specs/DropDown";
```

<!-- TODO:  Determine whether this sample causes gitHub to stop. -->

{"gitdown": "include-section", "file": "../Samples/SamplesExtension/Extension/Client/V1/Create/EngineV3/ViewModels/CreateEngineBladeViewModel.ts", "section": "config#specDropDown"}


### Validation 

Sometimes an extension should perform custom validation on the ARM dropdowns. For instance, you might want to check with your Resource Provider server to make sure that the selected location is available. If so, the developer can  a custom validator similar to any form field, as in the following example.

```ts
// The locations drop down.
var locationCustomValidation = new MsPortalFx.ViewModels.CustomValidation(
    validationMessage,
    (value) => {
        return this._dataContext.validateLocation(value).then((isValid) => {
            // Resolve with undefined if 'value' is a valid selection and with an error message otherwise.
            return MsPortalFx.ViewModels.getValidationResult(!isValid && validationMessage || undefined);
        }, (error) => {
            // Make sure your custom validation never throws. Catch the error, log the unexpected failure
            // so you can investigate later, and fail open.
            logError(...);
            return MsPortalFx.ViewModels.getValidationResult();
        });
    });
var locationsDropDownOptions: LocationsDropDown.Options = {
    ...,
    validations: ko.observableArray<MsPortalFx.ViewModels.Validation>([
        new MsPortalFx.ViewModels.RequiredValidation(ClientResources.selectLocation),
        locationCustomValidation // Add your custom validation here.
    ])
    ...
};
this.locationsDropDown = new LocationsDropDown(container, locationsDropDownOptions);
```

#### Create Validation

Create is the first opportunity to engage customers. Errors and delays put an extension at risk of losing customers, especially new customers. To address potential issues, extensions should create resources quickly and easily. This means that the extension should handle all types of errors, including using the wrong location, exceeding quotas,  unhandled exceptions from servers or resource providers, and others.  Form field validation will help avoid failures and identify errors before deployment.

When a customer clicks the Create button, it should succeed. In an effort to resolve Create success regressions as early as possible, severity 2 incidents are initiated and assigned to first-party extension teams whenever the success rate for an extension drops by at least 5% for 50+ deployments over a rolling 24-hour period.

<!-- TODO:  Determine what happens for third-party extension teams. -->

Incidents are created and monitored on the ICM dashboard that is located at   [http://icm.ad.msft.net](http://icm.ad.msft.net).  

 For more information about adding validation to your form fields, see  [top-forms-field-validation.md](top-forms-field-validation.md).  

#### ARM template deployment validation

If the form uses the ARM provisioner, you need to opt in to deployment validation manually by adding
`CreateFeatures.EnableArmValidation` to the `HubsProvisioner<T>` options. Wizards are not supported. Reach out to <a href="mailto:ibizafxpm@microsoft.com?subject=Create wizards + deployment validation">Ibiza FX PM</a> if you have  questions about wizard support.

The following code demonstrates opting in to deployment validation.

```ts
 this.armProvisioner = new HubsProvisioner.HubsProvisioner<DeployFromTemplateDataModel>(container, initialState, {
    supplyTemplateDeploymentOptions: this._supplyProvisioningPromise.bind(this),
    actionBar: this.actionBar,
    parameterProvider: this.parameterProvider,
    createFeatures: Arm.Provisioner.CreateFeatures.EnableArmValidation
});
```

For more information about deployment validation see the Engine V3 sample located at `<dir>\Client\Create\EngineV3\ViewModels\CreateEngineBladeViewModel.ts`.

<!--TODO:  Determine where this sample is located, because the following link does not work.  -->

- [http://aka.ms/portalfx/samples#create/microsoft.engine](http://aka.ms/portalfx/samples#create/microsoft.engine)


### Automation options

All Create forms should have automated testing to help avoid regressions. Validation is critical, and the performance of the Create blade is important  in ensuring the highest possible quality for your customers.

If your form uses the ARM provisioner, the "Automation options" link is displayed in the action bar by default. This link
gets the same template that is sent to ARM and displays it for the user. This allows customers to automate the creation of
resources by using the command line interface (CLI), **PowerShell**, and other supported tools and platforms. Wizards are not  supported; reach out to <a href="mailto:ibizafxpm@microsoft.com?subject=Create wizards + automation options">Ibiza FX PM</a> if you have questions about wizard support.

When customers leave the Create blade before submitting the form, the Portal asks for feedback. The feedback is stored
in the standard telemetry tables, as specified in [portalfx-telemetry.md](portalfx-telemetry.md). For example, the query `source == "FeedbackPane" and action == "CreateFeedback"` will retrieve abandonment feedback.




### Initiating Create from other places

Some scenarios may require launching the Create experience from outside the `+New` menu or the Marketplace. The following are supported patterns.

* From a command using the ParameterCollectorCommand

 "Add Web Tests" command from the "Web Tests" blade for a web app

* From a part using the SetupPart flow

 "Continuous Deployment" setup from a web app blade, as in the following image.

![alt-text](../media/portalfx-create/create-originating-from-blade-part.png "Create originating from blade part")




### Create Marketplace and Gallery packages

The Marketplace provides a categorized collection of packages that can be created in the Portal. To publish your package to the Marketplace, perform the following steps.

1. Create a package and publish it to the DF Marketplace yourself, if applicable. Learn more about [publishing packages to the Marketplace](../../gallery-sdk/generated/index-gallery.md).

1. Side-load your extension to test it locally.

1. Set a "hide key" before testing in production.

1. Send the package to the Marketplace team to publish it for production, as specified in []().

1. Notify the Marketplace team when the extension is ready to go live.

**NOTE**: The **+New** menu is curated and can change at any time, based on C+E leadership business goals. Ensure documentation, demos, and tests are created from browse, as specified in [portalfx-browse.md](portalfx-browse.md) or deep-links as specified in [portalfx-links.md](portalfx-links.md) as the entry point.

![alt-text](../media/portalfx-create/plus-new.png "The +New menu")

![alt-text](../media/portalfx-ui-concepts/gallery.png "The Marketplace")

### Wizards

The Azure portal has a **legacy pattern** for wizard blades, because customer feedback and usability studies have proven that the design is not  ideal and therefore should not be used. In addition, the wizard does not include parameter collectors, as specified in 
[portalfx-parameter-collection-getting-started.md](portalfx-parameter-collection-getting-started.md), which leads to complicated designs and increased development time.  Consequently it is not recommended that extensions continue to use wizards, and they are not supported.

Reach out to <a href="mailto:ibizafxpm@microsoft.com?subject=Create wizards + full screen">Ibiza FX PM</a> if you have any questions about wizards and full-screen Create support.
