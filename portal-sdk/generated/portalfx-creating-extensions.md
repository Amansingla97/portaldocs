* [Getting started with the Portal SDK](#getting-started-with-the-portal-sdk)
    * [Prerequisites](#getting-started-with-the-portal-sdk-prerequisites)
    * [Creating an Extension](#getting-started-with-the-portal-sdk-creating-an-extension)
    * [Marketplace Gallery Integration and Create Experience](#getting-started-with-the-portal-sdk-marketplace-gallery-integration-and-create-experience)
    * [Browse](#getting-started-with-the-portal-sdk-browse)
    * [Resource Menu Blade](#getting-started-with-the-portal-sdk-resource-menu-blade)
    * [V1 versus V2 in the samples extension](#getting-started-with-the-portal-sdk-v1-versus-v2-in-the-samples-extension)
    * [Hello World for blades](#getting-started-with-the-portal-sdk-hello-world-for-blades)
    * [Next Steps](#getting-started-with-the-portal-sdk-next-steps)
    * [Questions?](#getting-started-with-the-portal-sdk-questions)


<a name="getting-started-with-the-portal-sdk"></a>
## Getting started with the Portal SDK

<a name="getting-started-with-the-portal-sdk-prerequisites"></a>
### Prerequisites

- Windows 10 [Download](https://www.microsoft.com/en-us/software-download/windows10), Windows Server 2012 R2 [Download](https://www.microsoft.com/en-us/download/details.aspx?id=41703), or the most recent edition of the client or server platform. Some 
- Visual Studio 2015 update 3 (a SKU of Professional or greater is required) - [Download](https://www.visualstudio.com/vs/older-downloads/)
- TypeScript 2.3.3 for Visual Studio 2015 — May 22, 2017 [Download](http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.3.3-TS-release-dev14update3-20170519.1/TypeScript_Dev14Full.exe)
- Node.js LTS 8.11.1 or later - [Download](https://nodejs.org/dist/v8.11.1/node-v8.11.1-x64.msi)
- Node Tools for Visual Studio 2025 v1.3.1- [Download](https://github.com/Microsoft/nodejstools/releases/tag/v1.3.1)
- The latest Azure Portal SDK - [Download](http://aka.ms/portalfx/download)

<a name="getting-started-with-the-portal-sdk-creating-an-extension"></a>
### Creating an Extension
The Azure Portal SDK includes everything you need to build extensions for the portal.  Included are variety of tools and samples that help developers build extensions on top of the framework.

To start, launch Visual Studio and navigate to File -> New -> Project.  In the new project template, select Installed -> Visual C# -> Azure Portal.  Select the `Azure Portal Extension` project type, give it a unique name, and click 'OK':

![New project template](../media/portalfx-overview/new-project-template.png)

- Next, hit F5 to compile, and run your extension in IIS Express.   
- On first run you should see a request to install a certificate for localhost for IIS express Accept the certificate to continue

  ![Accept https certificate](../media/portalfx-overview/enablehttps.png)

You may get a debugging not enabled warning.  Feel free to enable or disable debugging, depending on your preferences.

![Debugging not enabled](../media/portalfx-overview/first-run-debugging-dialog.png)

It will now open a new window to the url which your extension service is running on `https://localhost:44300/`.

Your extension will now be side loaded into the production portal. The portal will prompt you to allow your side loaded extension. Click __allow__.

![Untrusted Extensions](../media/portalfx-overview/untrusted-extensions.png)

**Note**: If the browser did not automatically open a new window to the portal with the side loaded querystring already composed.  Check the following two items:
 
 - Your web.config appSetting with key ending in `.IsDevelopmentMode` is set to true.
 - Check your browser has not blocked popups.
 
If you are still having trouble after performing both of the above you can  manually side-load your extension by going to the url:

```
 https://portal.azure.com/?feature.canmodifyextensions=true#?testExtensions={"YourExtensionName":"https://localhost:44300/"}   
```

Just replace `"YourExtensionName"` with the name you chose for your new Azure Portal Extension project name and `"https://localhost:44300/"` with the url of your extension service (but this is the default url).

Congratulations!  You've just created your first extension.

You will find that the project template has implemented many of the key components of an extension.  

- Marketplace Gallery Integration (How people create your resources)
- Browse (How people browse resources they have created)
- Resource Menu Blade (How people use and manage resources they have created)

<a name="getting-started-with-the-portal-sdk-marketplace-gallery-integration-and-create-experience"></a>
### Marketplace Gallery Integration and Create Experience

If you plan on selling things (like resources) in the portal then you probably want to integrate into the marketplace.  The marketplace offers users a consistent way to browse and search the set of curated items that they can purchase (or create).

You integrate into the marketplace by building and publishing a package to the marketplace service.  This section will walk you through the basics of the marketplace.

In your running portal, go to the marketplace by clicking __+New__ and then __See all__.

![Launch Marketplace](../media/portalfx-overview/marketplace-launch.png)

Then click the __Local Development__ category. A marketplace item will appear that matches the name you selected when creating your project.  The code to define the gallery package is located in the __GalleryPackages__ directory.

![Local Development](../media/portalfx-overview/marketplace-local-development.png)

Click the gallery item to launch the item details blade. 

![gallery item details](../media/portalfx-overview/gallery-item-details.png)

The gallery package includes all the categorization, text, and images that you've seen so far.  The code for all of this is located in the __GalleryPackages__ folder.

When you click the __Create__ button the portal will open the blade that is defined in __GalleryPackages/Create/UIDefinition.json__. 

![Reference to create blade](../media/portalfx-overview/ui-definition-create-blade.png)

The project template has implemented a basic create form that asks the user to specify the common Azure Resource properties.

![Create blade screenshot](../media/portalfx-overview/create-blade-screenshot.png)

The code that implements the create blade is located in __Client/Resource/Create__.  Note that the name "CreateBlade" in the __Create.pdl__ file matches the name in the __UIDefinition.json__ file.

![Create blade code](../media/portalfx-overview/create-blade.png)

Fill out the create form and click create to actually create a resource.

For more information on creating gallery packages and create forms see the [gallery documentation](/gallery-sdk/generated/index-gallery.md#Marketplace-Gallery-Integration-and-Create-Experience).

<a name="getting-started-with-the-portal-sdk-browse"></a>
### Browse

The portal exposes a common navigation experience, called 'Browse', that gives end users a list of all the different services and resource types offered by the portal.

You probably want your assets to be exposed in the navigation system. This section provides an overview.

In the running portal you can navigate to __Browse -> My Resources__ to see your extension's browse experience.  
  
![Browse My Resources](../media/portalfx-overview/browse-my-resources.png)

If you have completed the marketplace and create tutorial above then you should see an item in the list.

![Browse My Resources List](../media/portalfx-overview/browse-my-resources-list.png)

The code for the browse implementation is located in __Client/Browse__.  You can replace the __Microsoft.PortalSdk/rootResources__ resource type name with your production Resource Provider (RP) type and your resources will show in the list on your next F5.

![Browse Code](../media/portalfx-overview/browse-code.png)

For more information on the browse experience see the [Browse documentation](#getting-started-with-the-portal-sdk-browse).

<a name="getting-started-with-the-portal-sdk-resource-menu-blade"></a>
### Resource Menu Blade

If you are building an extension for an Azure service then it's likely you have built a resource provider that exposes a top-level resource (e.g. Virtual Machine, Storage account).

If that's the case then the resource menu blade is a great starting point. The idea is that after the user selects a particular resource from the browse experience they land on a menu blade that has a mixture of standard resource features (e.g. activity log, role based access control, Support, etc) and service-specific features (e.g. Tables in a storage account). This section walks through the basics. 

Click on your resource from within the browse list to open the resource menu blade.  Many of the standard Azure experiences such as __tags__, __locks__, and __acess control__ have been automatically injected into your menu.

![Resource menu blade](../media/portalfx-overview/resource-menu.png)

The code for the resource menu blade is located in __Browse/ViewModels/AssetTypeViewModel__. You can extend the menu by modifying the __getMenuConfig__ function.

![Resource menu blade code](../media/portalfx-overview/resource-menu-code.png)

For more information on the resource menu blade see the [Resource menu blade documentation](/gallery-sdk/generated/index-gallery.md#resource-management-resource-menu).

<a name="getting-started-with-the-portal-sdk-v1-versus-v2-in-the-samples-extension"></a>
### V1 versus V2 in the samples extension

You will probably notice that the samples extension is forked into V1 and V2 folders.  

V2 is our post-GA collection of new APIs that, when we're done, is meant to be the only set of APIs needed to develop a modern Ibiza extension.

Our V1 APIs are riddled with APIs that support old UX patterns that we either have or want to move extensions away from. The V1 APIs are also quite difficult to use in many places, as they were developed quickly and early in the project when there was a lot of trial/error in both the UX design and in the associated APIs.

So far, V2 covers these API areas, and we're adding more:

- New Blade variations -- TemplateBlade, FrameBlade, MenuBlade importantly
- Blade-opening/closing -- 'container.openBlade, et al'
- no-PDL TypeScript decorators -- to define all recommended Blade/Part variations
- Forms -- No V1 EditScope concept

As for V1 concepts, these are concepts we're asking extensions to avoid where there are V2 APIs that can be used:
- __PDL__
- __EditScope__
- __ParameterCollector/ParameterProvider__
- __"Blades containing Parts"__
- __Non-full-screen Blades__ -- that is, ones that open with a fixed width
- __V1 Blade-opening__ -- Selectable/SelectableSet APIs
- __V1 Forms__ -- using EditScope

Bear in mind that we don't have the V2 space entirely built out. In the meantime, you will have to use V1 APIs in places, even the V1 concepts listed above.

<a name="getting-started-with-the-portal-sdk-hello-world-for-blades"></a>
### Hello World for blades

Blades are the main unit of UX that can be built using the SDK. They are basically pages that can be loaded in the portal.

To create a blade add a single file to your project in `$ProjectRoot\Client\<Area>\<Blade>.ts` where `<Area>` is a logically named area of your choosing and `<Blade>` is the name of your blade. An area is basically a folder.  Name it however it makes sense for your team.

If you are defining a new area then you must add a file directly in the area folder called `<Area>Area.ts` where `<Area>` is your area name.  The contents of that file should be:

```
export class DataContext {
}
```

Your blade file has a few required parts.  At the top you will have some import statements.

```
import * as TemplateBlade from "Fx/Composition/TemplateBlade";
import { DataContext } from "./<Area>Area"; // todo - put your area name here
```

The next step is to add the template blade decorator.  This lets the framework discover your blade.  It is also where you define the html template.

This template shows a simple text data-binding (corresponding data-binding code is below).

```
@TemplateBlade.Decorator({
    htmlTemplate: `<div data-bind='text: helloWorldMessage'></div>`
})
export class <Name> // todo - put your blade name here
{
}
```

Next we'll implement the class. In this simple case we'll just set the title, subtitle, and bind the string 'Hello world!' to the UI.

```
export class <Name> // todo - put your blade name here
{
    public title = "My blade title";
    public subtitle = "My blade subtitle";
    public context: TemplateBlade.Context<void, DataContext>; // there are useful framework APIs that hang off of here
    public helloWorldMessage = ko.observable("Hello world!"); // this is bound to the template above

    public onInitialize()
    {
       // run any initialization code you need here
       return Q();   // if you load data then return a loading promise here
    }
}
```

That's it. After you compile your blade you can test it out by deep linking using this format:

`<PORTAL URI>#blade/<YOUR EXTENSION NAME>/<YOUR BLADE NAME>`

<a name="getting-started-with-the-portal-sdk-next-steps"></a>
### Next Steps

Read more about [testing in production](portalfx-testinprod.md).

Next Steps: To debug issues loading an extension in the portal, go through the [Debugging extension failures](portalfx-debugging-extension-load-failures.md) guide.

<a name="getting-started-with-the-portal-sdk-questions"></a>
### Questions?

Ask questions on: [https://stackoverflow.microsoft.com/questions/tagged?tagnames=ibiza](https://stackoverflow.microsoft.com/questions/tagged?tagnames=ibiza).
