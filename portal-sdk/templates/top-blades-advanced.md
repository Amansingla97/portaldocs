
## Advanced Topics

The following sections discuss advanced topics in template blade development.

* [Deep linking](#deep-linking)

* [Displaying notifications using the status bar](#displaying-notifications-using-the-status-bar) 

* [Pinning the blade](#pinning-the-blade)

* [Storing settings](#storing-settings)

* [Displaying Unauthorized UI](#displaying-unauthorized-ui)

* [Dynamically displaying Notice UI](#dynamically-displaying-notice-ui)

**NOTE**: In this discussion, `<dir>` is the `SamplesExtension\Extension\` directory, and  `<dirParent>`  is the `SamplesExtension\` directory, based on where the samples were installed when the developer set up the SDK. If there is a working copy of the sample in the Dogfood environment, it is also included.

* * *

### Deep linking

The portal lets the user link directly to particular blade. As long as your blade does not have the ReturnsData decorator then users can deep link to it. There are a few supported link formats.

* The #blade format lets you link to arbitrary blades

    Format:

    ```
    https://portal.azure.com/{directory}#blade/{extension}/{blade}
    ```

    Example: 

    [https://portal.azure.com/microsoft.com#blade/HubsExtension/HelpAndSupportBlade](https://portal.azure.com/microsoft.com#blade/HubsExtension/HelpAndSupportBlade)

    Blade parameters are serialized in consecutive name/value pairs. 

    Format:

    ```
    https://portal.azure.com/{directory}#blade/{extension}/{blade}/{param1name}/{param1Val}/{param2name}/{param2Val}
    ```

    Example:
    [https://portal.azure.com/microsoft.com#blade/HubsExtension/BrowseAllBladeWithType/type/HubsExtension_Tag](https://portal.azure.com/microsoft.com#blade/HubsExtension/BrowseAllBladeWithType/type/HubsExtension_Tag)


* The #create format lets you link to marketplace items

    Format:

    ```
    https://portal.azure.com/{directory}#create/{packageid}
    ```

    Example:

    [https://portal.azure.com/microsoft.com#create/NewRelic.NewRelicAccount](https://portal.azure.com/microsoft.com#create/NewRelic.NewRelicAccount)

    To link to the Marketplace item details blade for your package, add "/preview" to the end of your Create blade link. 

    Format:

    ```
    https://portal.azure.com/{directory}#create/{packageid}/preview
    ```

    Example:

    [https://portal.azure.com/microsoft.com#create/NewRelic.NewRelicAccount/preview](https://portal.azure.com/microsoft.com#create/NewRelic.NewRelicAccount/preview)

* The #resource format lets you link to Azure resources

    To link to resources, all you need is the resource id. Currently, only subscription resources are supported. Tenant resources and nested resources are not supported.

    Format:

    ```
    https://portal.azure.com/{directory}#resource{resourceid}
    ```

    Example:
    [https://portal.azure.com/microsoft.com#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/foo/providers/microsoft.web/sites/bar](https://portal.azure.com/microsoft.com#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/foo/providers/microsoft.web/sites/bar)

* The #asset format lets you link to arbitrary portal assets

    Format:

    ```
    https://portal.azure.com/{directory}#asset/{extension}/{assettype}/{assetid}
    ```

    Example:
    [https://portal.azure.com/microsoft.com#asset/Microsoft_Azure_Billing/BillingSubscriptionBrowseService/00000000-0000-0000-0000-000000000000](com#asset/Microsoft_Azure_Billing/BillingSubscriptionBrowseService/00000000-0000-0000-0000-000000000000)

### Displaying notifications using the status bar

A status bar can be displayed at the top of a blade. The status bar supports text, and icon, and multiple severities (e.g. warning,info,error) and supports navigating to a blade or external site when clicked.

There is a sample for the status bar at [https://df.onecloud.azure-test.net/?feature.samplesextension=true#blade/SamplesExtension/TemplateBladeWithStatusBar](https://df.onecloud.azure-test.net/?feature.samplesextension=true#blade/SamplesExtension/TemplateBladeWithStatusBar). 

If you have the source code to the samples you can find the source code at this path: `Client/V2/Blades/Template/TemplateBladeWithStatusBar.ts`. It is also included in the following code.

{"gitdown": "include-file", "file":"../Samples/SamplesExtension/Extension/Client/V2/Blades/Template/TemplateBladeWithStatusBar.ts"}

### Making your blade pinnable to a dashboard

No-pdl blades can be made pinnable by making the following changes to your code.

1. Add the `@TemplateBlade.Pinnable.Decorator` decorator to the  Template Blade class, or `@FrameBlade.Pinnable.Decorator`, `@Blade.Pinnable.Decorator` for these less commonly used Blade variations.

1. Implement an `onPin' method`.

1. Return a `PartReference` instance to a Part you design.  The Part should open the Blade when clicked. 

These concepts are illustrated in the sample located at `<dir>/Client/V2/Blades/Pinning/PinnableBlade.ts` and in the working copy located at [https://df.onecloud.azure-test.net/#blade/SamplesExtension/PinnableBlade/personId/111](https://df.onecloud.azure-test.net/#blade/SamplesExtension/PinnableBlade/personId/111).

### Exception cases that affect the entire blade 

#### The user is not authorized to view the blade

You may determine that the current user does not have access to the current blade's content. In this case you can call `container.unauthorized()` and the user will be presented with a message that is consistent across all experiences in the portal.

For optimal performance, we recommend you avoid extra calls to determine access rights. When possible, simply try to make the calls needed to load the page and call the `unauthorized()` function if the call failed with an authorization error.

#### The service is not configured properly for the user to view the blade

You may have a scenario where the current user can't use the blade for some unexpected reason (e.g. they are not a part of a private preview or they have not done some prerequisite step). For scenarios like this you can call `container.enableNotice()` to render a generic notice with formatting that will be consistent across all experiences.

#### The underlying resource for this blade does not exist

A user may try to deep link to a blade for a resource that no longer exists. In this case you can call `container.notFound()` and provide a message. A generic not found UI will be presented to the user.

## The data required to load the blade failed for some unknown reason (e.g. 500 internal server error)

If your blade cannot load because of an unexpected error then you should call the `container.fail()` API. The message you provide here will be logged, but not presented to the user. This is because you should only use this API for unexpected errors. If you are handling an expected error condition then you should either use the `enableNotice()` API or do some custom rendering if the scenario requires it.