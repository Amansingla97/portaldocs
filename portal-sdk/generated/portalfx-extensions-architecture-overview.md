<a name="azure-portal-a-composed-web-application"></a>
## Azure Portal -- A composed web application

The Azure Portal web application is based on a UI composition system whose primary design goal is to enable the integration of UI built by 100's of teams into a single, robust single-page web application.

With this system, a team develops a UI extension to plug into and extend the UI of the Azure Portal.  Teams develop and refine UI iteratively and can choose a deployment cadence that suits their team schedule and their customer needs.  They can safely link from their UI to UI constructed by other teams, resulting in a Portal application that -- to the Azure user -- appears to have been built by a single team.  Any bug in a team's UI has only a local impact on that team's UI and does not impact the availability/reliability of the larger Azure Portal UX or that of any other UI extension.

<a name="azure-portal-a-composed-web-application-the-portal-shell"></a>
### The Portal &quot;Shell&quot;

The Azure Portal web application is designed to the single-page application pattern (https://en.wikipedia.org/wiki/Single-page_application), where UI is generated via client-side-evaluated JavaScript and dynamic HTML.  The Azure Portal "Shell" is the client-side JavaScript that controls the overall rendering of UI in the browser.  The Shell is responsible for rendering the "chrome" of the Azure Portal (the navigation menu on the left and bar at the top).  Any team- or service-specific UI is developed in UI extensions as Blades (pages or windows) and Parts (tiles).  Based on user interaction with the Azure Portal UI, the Shell determines which Blades/Parts are to do be displayed and it delegates the rendering of these Blades/Parts to the appropriate extension(s).

<Insert diagram here>

<a name="azure-portal-a-composed-web-application-ui-extensions"></a>
### UI extensions

UI extensions are simple, static web applications.  By static, I mean that UI extensions do no server-side UI-generation.  UI extensions are developed as client-side-evaluated TypeScript (compiled to JavaScript), as HTML templates, and as LESS stylesheets (compiled to CSS).  When the Shell determines that it needs to display a Blade or Part from UI extension "A", it JIT loads an HTML page that collects the extension JavaScript/HTML/CSS for that extension.  The Shell loads the HTML page from an endpoint URL registered centrally as part of the Azure Portal's configuration.  The Shell then directs the extension (and its client-side JavaScript) to render the required Blade or Part.  When the user navigates in such a way that no Blades/Parts from extension "A" are in use, the Shell can unload the UI extension from the browser, allowing the larger Azure Portal app to reclaim browser memory and network connections.

<a name="azure-portal-a-composed-web-application-ui-extension-isolation"></a>
### UI extension isolation

The business logic and UI generation accomplished by UI extensions is isolated from the Azure Portal "chrome" and from the UI of other UI extensions.  This is important for three reasons:
	- Security - UI extension HTTP access can be restricted to specific origins/endpoints. UI extension JavaScript can be isolated to their own JavaScript heap.
	- Reliability - UI extension isolation limits the user impact of UI extension bugs
	- Scale - Separating UI extension JavaScript allows the UI extension to be "unloaded" from the browser

In 2013, when the Azure Portal was designed, the only browser facility suitable for client-side JavaScript/DOM isolation was the <IFRAME> element.  Unfortunately, browsers circa 2013 did not scale performance-wise to the number of IFrames required by the many Parts (tiles) suggested by the 2013 Azure Portal UX design.  For this reason, UI extensions are loaded into the browser into non-visible IFrames.  Rather than using dynamic HTML techniques that require direct DOM access by the UI extension JavaScript, UI extensions "project" UI using Ibiza FX Controls and HTML templates.  Such "projected" UI is rendered in the single, visible IFrame that is managed by the Shell.

(Note: Since browsers circa 2018 scale better re: IFrames and since the Ibiza UX design is pivoting towards full-screen Blades, the Ibiza team continues to invest in a more conventional use of IFrames, where UI extensions can access the DOM directly and can craft their UI generation following stander web development patterns and OSS libraries.)

<a name="azure-portal-a-composed-web-application-projecting-blade-part-ui"></a>
### Projecting Blade/Part UI

UI extensions develop their Blades and Parts following the MVVM pattern (see https://en.wikipedia.org/wiki/Model–view–viewmodel).  Here:
	- The "view" is defined as a Blade/Part-specific HTML template.  The HTML template typically arranges uses of FX controls in the Blade/Part content area.
	- The HTML template and FX controls are bound to a UI-extension-developed "view model" TypeScript class, which is where the UI extension business logic is isolated from the JavaScript of the larger Portal application and from other UI extensions.
	- The "view model" frequently includes "model" data loaded via AJAX from the cloud (though most often from the Azure Resource Manager or from the team's/service's Resource Provider).

UI extensions develop a Blade or Part to this pattern by developing a TypeScript class adorned with a TypeScript decorator:

<insert code snippet>

(Note: Blades and Parts were previously developed by authoring XAML that describes the mapping from a Blade / Part name to its corresponding "view model" TypeScript class and its associated "view" HTML template.  This XAML API (named "PDL" for Portal Definition Language) was found to be developer-unfriendly in that it required that all three artifacts -- the XAML file, the TypeScript class file and the HTML template file -- be managed separately and kept in sync.  The new "no-PDL" TypeScript decorator APIs allow for a Blade or Part to be developed in as little as a single TypeScript file).

Now, when a UI extension's Blade or Part is to be displayed, the Shell instantiates in that UI extension's IFrame an instance of the Blade / Part TypeScript class (the "view model").  To "project" this Blade/Part UI into the Shell-managed visible IFrame that the user sees, the Shell makes use of a simple object-remoting API.  Here, the Blade / Part "view" and "view model" are copied and sent via the HTML "postMessage" API to the visible IFrame managed by the Shell.  It's in the Shell-managed, visible IFrame that the "view" and "view model" are two-way bound (using the Knockout.js OSS library at http://knockoutjs.com/).

<insert diagram>

As most UI is dynamic (like Forms that the user updates or like Grids/Lists that are refreshed to reflect new/updated server data), changes to the "view model" are kept consistent between the Shell and UI extension IFrames.  The object-remoting system detects changes to Knockout.js observables (see http://knockoutjs.com/) embedded in the "view model", computes diffs between the two "view model" copies and using "postMessage" to send diff-grams between the two "view model" copies.  Beyond the conventional use of the Knockout.js library by the UI extension and its "view model" class, complexities of the object-remoting system are hidden from the UI extension developer.

<a name="azure-portal-a-composed-web-application-secure-per-service-ui"></a>
### Secure per-service UI

The security model for UI extensions builds upon the standard same-origin policy that supported by all browsers and is the basis for today's web applications.  A UI extension's homepage URL is typically located on an origin specific to that UI extension and its Resource Provider.  This HTML page can only issue HTTPS calls to its origin domain and any origins that allow CORs (see https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) calls from the UI extension's origin.

In practice, HTTPS calls from UI extensions are made from the client to load "model" data, and the HTTPS calls are typically directed to these locations:
	- Using CORs, to the Azure Resource Manager (ARM) and/or to the service's Resource Provider (RP);
	- Less common, not recommended -- Using same-origin, to HTTP endpoints (to "extension controllers") dedicated to the operation of the UI extension.
In any of these cases, the HTTPS call includes an AAD token authorizing the UI extension to act on behalf of the user against those Azure resource types that the UI extension supports.  The AAD token is obtained during AAD single-sign-on authentication that precedes the loading of the Portal Shell.  When a UI extension is loaded into its client-side IFrame and asked to render a Blade or a Part, the UI extension typically calls an FX API with which it can acquire an AAD token scoped to that UI extension.  To load "model" data, the UI extension then issues HTTP calls carrying this token to ARM, to its RP or to its "extension controller".

<a name="azure-portal-a-composed-web-application-linking-navigating-within-the-portal"></a>
### Linking/navigating within the Portal

Frequently, user interactions with the Portal "chrome" and within Blade/Part UI will cause in-Portal navigation to a new Blade.  This navigation is accomplished via FX APIs, like so:

<insert code snippet>

There are two important concepts re: navigation that are demonstrated here.  First, in-Portal navigation is not accomplished via URL but, rather, through a FX TypeScript API available to UI-extension-authored Blades and Parts.  Second, the API requires use of a code-generated BladeReference used:
	- to identify the target Blade in question and 
	- to provide a compiler-verified API for the Blade's parameters.
For every Blade and Part developed in a UI extension, Ibiza tooling will code generate a corresponding BladeReference or PartReference that can be utilized with FX APIs to "open a Blade" and to "pin a Part" respectively.

<insert pin part snippet>

These APIs and associated code-generation are critical to integrating UI and UX across Azure services.  The same BladeReference and PartReference classes useful to extension "A" for navigating among its Blades/Parts can be employed by extension "B" to link to Blades from "A".  All that is necessary is for extension "A" to redistribute a code package containing:
	- A PDE (portal definition exports) file emitted as part of extension "A"'s build
	- A TypeScript definition file for those API types used in the construction of extension "A"'s exported Blades and Parts

<a name="azure-portal-a-composed-web-application-blade-part-api-versioning"></a>
### Blade/Part API versioning

Each UI-extension-developed Blade and Part includes TypeScript types that describe the set of "parameters" with which that Blade/Part can be invoked:

<insert code snippet>

These form the APIs for the Blades and Parts exported by extension "A" to those teams who wish to link to extension "A" UI.  Like any other API that is produced by one team for the consumption of others, these APIs should be updated only in a backwards-compatible manner.  The TypeScript implementation of a Blade in extension "A" must continue to support all versions of the "parameters" type ever published/exported to consuming teams.  Typically, extensions follow the best practices of:
	- Never changing the name of a Blade or a Part
	- Limiting their "parameters" updates to the addition of parameters that are marked (in TypeScript) as optional
	- Never removing parameters from their "Parameters" type

With this, extensions preserve the flexibility to evolve their sets of Blades and Parts and their APIs (independent of the Portal team and of team's reusing their UI) and to deploy changes at their own cadence (including backward-compatible API changes).

Additionally, the Ibiza SDK contains APIs that allow for the wholesale replacement of one Blade or Part for new equivalent Blades/Parts.  It also has APIs that allow for the safe migration of Blades and Part between UI extensions, as responsibilities for certain UI transfers between teams (for instance).

<a name="azure-portal-a-composed-web-application-common-portal-ux-marketplace-and-browse"></a>
### Common Portal UX -- Marketplace and Browse

Beyond Blades and Parts, UI extensions can benefit from other UI integration with the Azure Portal, mentioned briefly here.

First, a UI extension team can develop "Marketplace packages".  Each of these packages describes an entry that will be displayed in the Azure Portal "Marketplace" UI.  A package includes:
	- Metadata that specifies the UI for the item in the marketplace (icons, text, etc);
	- Metadata that locates the Blade that will be invoked once that item is selected by the user in the Marketplace.  This Blade is a stylized Form that includes logic that knows how to provision Azure resources;
	- ARM templates that describe how a resource will be provisioned via ARM.
A UI extension team publishes their N>0 packages to a service, where all such packages/items are available in queryable form to the Azure Portal and to the Marketplace UI (which, incidentally, is itself implemented as Blades in the "Azure Marketplace" UI extension).

Second, for a given UI extension / ARM resource type to be represented in the standard Azure Portal "Browse" UI, UI extensions develop UI metadata for the resource types their UI extension supports.  At time of writing, this is done with the PDL <AssetType> tag.  No-PDL variants of <AssetType> are currently in development.  An "asset type" is expressed as metadata so that standard Azure Portal UI (like "Browse" Blades and Parts) can display resources without the need to involve UI extension business log (potentially a performance problem when spanning N resources types).
