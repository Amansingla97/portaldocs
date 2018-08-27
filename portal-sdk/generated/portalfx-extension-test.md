* [Choosing the right test Framework](#choosing-the-right-test-framework)
* [C# Portal Test Framework](#c-portal-test-framework)
    * [Testing Best Practices](#c-portal-test-framework-testing-best-practices)
        * [Always verify that every action completed as expected](#c-portal-test-framework-testing-best-practices-always-verify-that-every-action-completed-as-expected)
        * [Log everything](#c-portal-test-framework-testing-best-practices-log-everything)
        * [Use built in Test Framework methods](#c-portal-test-framework-testing-best-practices-use-built-in-test-framework-methods)
        * [Use WaitUntil for retrying actions and waiting for conditions](#c-portal-test-framework-testing-best-practices-use-waituntil-for-retrying-actions-and-waiting-for-conditions)
        * [Prefer WaitUntil to Assert for non instantaneous conditions](#c-portal-test-framework-testing-best-practices-prefer-waituntil-to-assert-for-non-instantaneous-conditions)
        * [Create proper wrapper/abstractions for commonly used patterns](#c-portal-test-framework-testing-best-practices-create-proper-wrapper-abstractions-for-commonly-used-patterns)
        * [Clear user settings before starting a test](#c-portal-test-framework-testing-best-practices-clear-user-settings-before-starting-a-test)
        * [Use FindElements to verify the absence of elements](#c-portal-test-framework-testing-best-practices-use-findelements-to-verify-the-absence-of-elements)
        * [Prefer CssSelector to Xpath](#c-portal-test-framework-testing-best-practices-prefer-cssselector-to-xpath)
* [msportalfx-test](#msportalfx-test)
    * [Overview](#msportalfx-test-overview)
    * [Getting Started](#msportalfx-test-getting-started)
        * [Installation](#msportalfx-test-getting-started-installation)
        * [Write a test](#msportalfx-test-getting-started-write-a-test)
        * [Add the configuration](#msportalfx-test-getting-started-add-the-configuration)
        * [Compile and run](#msportalfx-test-getting-started-compile-and-run)
        * [Updating](#msportalfx-test-getting-started-updating)
        * [More Examples](#msportalfx-test-getting-started-more-examples)
        * [Running tests in Visual Studio](#msportalfx-test-getting-started-running-tests-in-visual-studio)
    * [Side loading a local extension during the test session](#msportalfx-test-side-loading-a-local-extension-during-the-test-session)
    * [Running](#msportalfx-test-running)
        * [In Dev](#msportalfx-test-running-in-dev)
        * [CI](#msportalfx-test-running-ci)
    * [Debugging](#msportalfx-test-debugging)
        * [debug tests 101](#msportalfx-test-debugging-debug-tests-101)
        * [debugging tests in VS Code](#msportalfx-test-debugging-debugging-tests-in-vs-code)
        * [Checking the result of the currently running test in code](#msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code)
        * [How to take a screenshot of the browser](#msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser)
        * [How to capture browser console output](#msportalfx-test-debugging-how-to-capture-browser-console-output)
        * [Callstack](#msportalfx-test-debugging-callstack)
        * [Test output artifacts](#msportalfx-test-debugging-test-output-artifacts)
    * [Localization](#msportalfx-test-localization)
    * [User Management](#msportalfx-test-user-management)
    * [Configuration](#msportalfx-test-configuration)
        * [Configuration options](#msportalfx-test-configuration-configuration-options)
    * [Scenarios](#msportalfx-test-scenarios)
        * [Create](#msportalfx-test-scenarios-create)
        * [Browse](#msportalfx-test-scenarios-browse)
        * [How to test the grid in browse shows additional extension resource columns that are selected](#msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected)
        * [Blades](#msportalfx-test-scenarios-blades)
        * [Parts](#msportalfx-test-scenarios-parts)
        * [Command](#msportalfx-test-scenarios-command)
        * [Action Bar](#msportalfx-test-scenarios-action-bar)
        * [Delete](#msportalfx-test-scenarios-delete)
        * [Styling / layout regression detection](#msportalfx-test-scenarios-styling-layout-regression-detection)
        * [Locators](#msportalfx-test-scenarios-locators)
        * [Consuming Updates](#msportalfx-test-scenarios-consuming-updates)
        * [Mocking](#msportalfx-test-scenarios-mocking)
        * [Code Coverage](#msportalfx-test-scenarios-code-coverage)
        * [Contributing](#msportalfx-test-scenarios-contributing)
        * [To run the tests](#msportalfx-test-scenarios-to-run-the-tests)
        * [API Reference](#msportalfx-test-scenarios-api-reference)


<a name="choosing-the-right-test-framework"></a>
# Choosing the right test Framework

The Ibiza team supports two different end to end test frameworks; a C# based test framework and a typescript based test framework. Partners can choose either of the test frameworks for developing end to end tests. We are working towards making both the test frameworks open source. An open source approach will enable partners to leverage APIs that are contributed by other partners. Building a strong open source community around these test frameworks will help improve the development experience for the test frameworks.

1. C# Test Framework (portalfx-test)

C# based test-fx is fully supported by the Ibiza team. The C# is internally used for testing the Azure Portal (Shell). The C# test framework is available for partners to view the code contribute features and bug fixes. 

[Link to documentation](portalfx-test.md)

1. Typescript Test Framework (msportalfx-test)

We have partnered with the IaaS team to develop an open-source typescript test framework. In fact, we are building an end to end test suite for our own extensions against this test framework. As part of developing this test framework we are building certain abstractions such as ‘open blade’ and ‘Navigate to settings blade’ that can be useful for testing your extensions too. The release of the typescript test framework is already decoupled from the release of SDK so partners can pick up latest version of it without recompiling their extension against a newer version of the Ibiza SDK.

[Link to documentation](msportalfx-test.md)

Comparison of test-frameworks:

- Maturity (Number of Selector APIs) : C# > typescript
- [Built on Selenium webdriver open standard](http://www.seleniumhq.org/projects/webdriver/) : Both Supported by Ibiza
- Documentation for Typescript test framework is more up to date than C# test framework
- Browser Support: Both support Chrome.  C# also has limited support for Firefox and IE.
- ARM API support:  MsPortalFx-Test has limited support for directly calling ARM to create/delete resources.  
- Test Execution Speed: typescript is 20% faster
- Distributed independently from SDK: Both
- Open Source contribution Model: Actively working on moving Typescript based test-fx to open source contribution model. We are investigating dev work to move C# based test-fx to open source contribution Model.

* [Choosing the right test Framework](#choosing-the-right-test-framework)
* [C# Portal Test Framework](#c-portal-test-framework)
    * [Testing Best Practices](#c-portal-test-framework-testing-best-practices)
        * [Always verify that every action completed as expected](#c-portal-test-framework-testing-best-practices-always-verify-that-every-action-completed-as-expected)
        * [Log everything](#c-portal-test-framework-testing-best-practices-log-everything)
        * [Use built in Test Framework methods](#c-portal-test-framework-testing-best-practices-use-built-in-test-framework-methods)
        * [Use WaitUntil for retrying actions and waiting for conditions](#c-portal-test-framework-testing-best-practices-use-waituntil-for-retrying-actions-and-waiting-for-conditions)
        * [Prefer WaitUntil to Assert for non instantaneous conditions](#c-portal-test-framework-testing-best-practices-prefer-waituntil-to-assert-for-non-instantaneous-conditions)
        * [Create proper wrapper/abstractions for commonly used patterns](#c-portal-test-framework-testing-best-practices-create-proper-wrapper-abstractions-for-commonly-used-patterns)
        * [Clear user settings before starting a test](#c-portal-test-framework-testing-best-practices-clear-user-settings-before-starting-a-test)
        * [Use FindElements to verify the absence of elements](#c-portal-test-framework-testing-best-practices-use-findelements-to-verify-the-absence-of-elements)
        * [Prefer CssSelector to Xpath](#c-portal-test-framework-testing-best-practices-prefer-cssselector-to-xpath)
* [msportalfx-test](#msportalfx-test)
    * [Overview](#msportalfx-test-overview)
    * [Getting Started](#msportalfx-test-getting-started)
        * [Installation](#msportalfx-test-getting-started-installation)
        * [Write a test](#msportalfx-test-getting-started-write-a-test)
        * [Add the configuration](#msportalfx-test-getting-started-add-the-configuration)
        * [Compile and run](#msportalfx-test-getting-started-compile-and-run)
        * [Updating](#msportalfx-test-getting-started-updating)
        * [More Examples](#msportalfx-test-getting-started-more-examples)
        * [Running tests in Visual Studio](#msportalfx-test-getting-started-running-tests-in-visual-studio)
    * [Side loading a local extension during the test session](#msportalfx-test-side-loading-a-local-extension-during-the-test-session)
    * [Running](#msportalfx-test-running)
        * [In Dev](#msportalfx-test-running-in-dev)
        * [CI](#msportalfx-test-running-ci)
    * [Debugging](#msportalfx-test-debugging)
        * [debug tests 101](#msportalfx-test-debugging-debug-tests-101)
        * [debugging tests in VS Code](#msportalfx-test-debugging-debugging-tests-in-vs-code)
        * [Checking the result of the currently running test in code](#msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code)
        * [How to take a screenshot of the browser](#msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser)
        * [How to capture browser console output](#msportalfx-test-debugging-how-to-capture-browser-console-output)
        * [Callstack](#msportalfx-test-debugging-callstack)
        * [Test output artifacts](#msportalfx-test-debugging-test-output-artifacts)
    * [Localization](#msportalfx-test-localization)
    * [User Management](#msportalfx-test-user-management)
    * [Configuration](#msportalfx-test-configuration)
        * [Configuration options](#msportalfx-test-configuration-configuration-options)
    * [Scenarios](#msportalfx-test-scenarios)
        * [Create](#msportalfx-test-scenarios-create)
        * [Browse](#msportalfx-test-scenarios-browse)
        * [How to test the grid in browse shows additional extension resource columns that are selected](#msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected)
        * [Blades](#msportalfx-test-scenarios-blades)
        * [Parts](#msportalfx-test-scenarios-parts)
        * [Command](#msportalfx-test-scenarios-command)
        * [Action Bar](#msportalfx-test-scenarios-action-bar)
        * [Delete](#msportalfx-test-scenarios-delete)
        * [Styling / layout regression detection](#msportalfx-test-scenarios-styling-layout-regression-detection)
        * [Locators](#msportalfx-test-scenarios-locators)
        * [Consuming Updates](#msportalfx-test-scenarios-consuming-updates)
        * [Mocking](#msportalfx-test-scenarios-mocking)
        * [Code Coverage](#msportalfx-test-scenarios-code-coverage)
        * [Contributing](#msportalfx-test-scenarios-contributing)
        * [To run the tests](#msportalfx-test-scenarios-to-run-the-tests)
        * [API Reference](#msportalfx-test-scenarios-api-reference)


<a name="c-portal-test-framework"></a>
# C# Portal Test Framework

Please use the following links for info on how to use the C# Portal Test Framework:


The page you requested has moved to [top-extensions-csharp-test-framework.md](top-extensions-csharp-test-framework.md). 


The page you requested has moved to [top-extensions-csharp-test-framework.md#creating-the-test-project](top-extensions-csharp-test-framework.md#creating-the-test-project). 



   The page you requested has moved to [top-extensions-csharp-test-framework.md#testing-parts-and-blades](top-extensions-csharp-test-framework.md#testing-parts-and-blades). 




The page you requested has moved to [top-extensions-csharp-test-framework.md#entering-data-into-forms](top-extensions-csharp-test-framework.md#entering-data-into-forms). 




   The page you requested has moved to 
[top-extensions-csharp-test-framework.md#testing-parts-and-blades](top-extensions-csharp-test-framework.md#testing-parts-and-blades).



   The page you requested has moved to [top-extensions-csharp-test-framework.md#testing-commands](top-extensions-csharp-test-framework.md#testing-commands). 




The page you requested has moved to [top-extensions-flags.md#shell-feature-flags](top-extensions-flags.md#shell-feature-flags). 



<a name="c-portal-test-framework-testing-best-practices"></a>
## Testing Best Practices

As you write UI based test cases using the Portal Test Framework it is recommended you follow a few best practices to ensure maximum reliability and to get the best value from your tests.

<a name="c-portal-test-framework-testing-best-practices-always-verify-that-every-action-completed-as-expected"></a>
### Always verify that every action completed as expected
In many cases the browser is not as fast as the test execution, so if you don't wait until expected conditions have completed your tests could easily fail. For example:

```cs
commandBar.FindMessageBox("Delete contact").ClickButton("Yes");
webDriver.WaitUntil(() => !commandBar.HasMessageBox, "There is still a message box in the command bar.");
```

Here, the "Yes" button of a message box is clicked and you would expect it to go away as soon as the button is clicked. However this might not happen as fast as you think. Therefore we wait until the CommandBar.HasMessageBox property reports 'false' before proceeding, which ensures the message box is gone and will not interfere with the next action.

<a name="c-portal-test-framework-testing-best-practices-log-everything"></a>
### Log everything
It can be very difficult to diagnose a failed test case without some good logging. An easy way to write these logs is to use the **TestContext.WriteLine** method:

```cs
TestContext.WriteLine("Starting provisioning from the StartBoard...");
```

<a name="c-portal-test-framework-testing-best-practices-use-built-in-test-framework-methods"></a>
### Use built in Test Framework methods
The Portal Test Framework provides many built in methods to perform actions on Portal web elements and it is recommended to use them for maximum test maintainability and reliability. For example, one way to find a StartBoard part by it's title is this:

```cs
var part = webDriver.WaitUntil(
    () => portal.StartBoard.FindElements<Part>()
    .FirstOrDefault(p => p.PartTitle.Equals("TheTitle")),
    "Could not find a part with title 'Samples'.");
```

However this can be greatly simplified by using a built in method:

```cs
var part = portal.StartBoard.FindSinglePartByTitle("TheTitle");
```

Not only this significantly reduces the amount of code to write and maintain but also encapsulates the best practice of waiting until elements are found since FindSinglePartByTitle will internally perform a WaitUntil operation that will retry until the part is found (or the timeout is reached).

BaseElement also contains an extension method that wraps the webDriver.WaitUntil call.

```cs
var part = blade.WaitForAndFindElement<Part>(p => p.PartTitle.Equals("TheTitle"));
```

<a name="c-portal-test-framework-testing-best-practices-use-waituntil-for-retrying-actions-and-waiting-for-conditions"></a>
### Use WaitUntil for retrying actions and waiting for conditions
WaitUntil can also be used to retry an action since it just takes a lambda function which could be an action and then a verification step afterwards.  WaitUntil will return when a "truthy" (not false or null value) is returned.  This can be useful if the particular action is flakey.  Please be careful to only use actions that are idempotent when trying to use WaitUntil in this pattern.

<a name="c-portal-test-framework-testing-best-practices-prefer-waituntil-to-assert-for-non-instantaneous-conditions"></a>
### Prefer WaitUntil to Assert for non instantaneous conditions
The traditional way to verify conditions within test cases is by using **Assert** methods. However, when dealing with conditions that won't be satisfied immediately you should instead use **WebDriver.WaitUntil**:

```cs
var field = form.FindField<Textbox>("contactName");
field.Value = contactName + Keys.Tab;
webDriver.WaitUntil(() => field.IsValid, "The 'contactName' field did not pass validations.");
```

In this example, if we would have instead used Assert to verify the IsValid propery the test would most like have failed since the TextBox field has a custom async validation that will perform a request to the backend server to perform the required validation, and this is expected to take at least a second.

<a name="c-portal-test-framework-testing-best-practices-create-proper-wrapper-abstractions-for-commonly-used-patterns"></a>
### Create proper wrapper/abstractions for commonly used patterns
A good practice is to create wrappers and abstractions for common patterns of code you use (eg when writing a WaitUntil, you may want to wrap it in a function that describes its actual intent).  This makes your test code clear in its intent by hiding the actual details to the abstraction's implementation.  It also helps with dealing with breaking changes as you can just modify your abstraction rather than every single test.  

If you think an abstraction you wrote would be generic and useful to the test framework, feel free to contribute it!

<a name="c-portal-test-framework-testing-best-practices-clear-user-settings-before-starting-a-test"></a>
### Clear user settings before starting a test
As you may know, the Portal keeps good track of all user customizations via persistent user settings. This behavior might not be ideal for test cases since each test case could potentially find a Portal with different customizations each time. To avoid this you can use the **portal.ResetDesktopState** method.  Note that the method will force a reload of the Portal.

```cs
portal.ResetDesktopState();
```

<a name="c-portal-test-framework-testing-best-practices-use-findelements-to-verify-the-absence-of-elements"></a>
### Use FindElements to verify the absence of elements
Sometimes you are not trying to find a web element but instead you want to verify that the element is not there. In these cases you can use the **FindElements** method in combination with Linq methods to verify if the element is there:

```cs
webDriver.WaitUntil(() => portal.StartBoard.FindElements<Part>()
                                           .Count(p => p.PartTitle.Equals("John Doe")) == 0,
                    "Expected to not find a part with title 'John Doe' in the StartBoard");
```

In the example, we are verifying that there is no part with title 'John Doe' in the StartBoard.

<a name="c-portal-test-framework-testing-best-practices-prefer-cssselector-to-xpath"></a>
### Prefer CssSelector to Xpath
You will eventually be faced with the choice of using either CSS selectors or XPath to find some web elements in the Portal. As a general rule the preferred approach is to use CSS selectors because:

- Xpath engines are different in each browser
- XPath can become complex and hense it can be harder to read
- CSS selectors are faster
- CSS is JQuery's locating strategy
- IE doesn't have a native XPath engine

Here for a simple example where we are finding the selected row in a grid:

```cs
grid.FindElements(By.CssSelector("[aria-selected=true]"))
```

If you think the element you found would be a useful abstraction, feel free to contribute it back to the test framework!



The page you requested has moved to [portalfx-csharp-test-publish.md](portalfx-csharp-test-publish.md). 




* [Choosing the right test Framework](#choosing-the-right-test-framework)
* [C# Portal Test Framework](#c-portal-test-framework)
    * [Testing Best Practices](#c-portal-test-framework-testing-best-practices)
        * [Always verify that every action completed as expected](#c-portal-test-framework-testing-best-practices-always-verify-that-every-action-completed-as-expected)
        * [Log everything](#c-portal-test-framework-testing-best-practices-log-everything)
        * [Use built in Test Framework methods](#c-portal-test-framework-testing-best-practices-use-built-in-test-framework-methods)
        * [Use WaitUntil for retrying actions and waiting for conditions](#c-portal-test-framework-testing-best-practices-use-waituntil-for-retrying-actions-and-waiting-for-conditions)
        * [Prefer WaitUntil to Assert for non instantaneous conditions](#c-portal-test-framework-testing-best-practices-prefer-waituntil-to-assert-for-non-instantaneous-conditions)
        * [Create proper wrapper/abstractions for commonly used patterns](#c-portal-test-framework-testing-best-practices-create-proper-wrapper-abstractions-for-commonly-used-patterns)
        * [Clear user settings before starting a test](#c-portal-test-framework-testing-best-practices-clear-user-settings-before-starting-a-test)
        * [Use FindElements to verify the absence of elements](#c-portal-test-framework-testing-best-practices-use-findelements-to-verify-the-absence-of-elements)
        * [Prefer CssSelector to Xpath](#c-portal-test-framework-testing-best-practices-prefer-cssselector-to-xpath)
* [msportalfx-test](#msportalfx-test)
    * [Overview](#msportalfx-test-overview)
    * [Getting Started](#msportalfx-test-getting-started)
        * [Installation](#msportalfx-test-getting-started-installation)
        * [Write a test](#msportalfx-test-getting-started-write-a-test)
        * [Add the configuration](#msportalfx-test-getting-started-add-the-configuration)
        * [Compile and run](#msportalfx-test-getting-started-compile-and-run)
        * [Updating](#msportalfx-test-getting-started-updating)
        * [More Examples](#msportalfx-test-getting-started-more-examples)
        * [Running tests in Visual Studio](#msportalfx-test-getting-started-running-tests-in-visual-studio)
    * [Side loading a local extension during the test session](#msportalfx-test-side-loading-a-local-extension-during-the-test-session)
    * [Running](#msportalfx-test-running)
        * [In Dev](#msportalfx-test-running-in-dev)
        * [CI](#msportalfx-test-running-ci)
    * [Debugging](#msportalfx-test-debugging)
        * [debug tests 101](#msportalfx-test-debugging-debug-tests-101)
        * [debugging tests in VS Code](#msportalfx-test-debugging-debugging-tests-in-vs-code)
        * [Checking the result of the currently running test in code](#msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code)
        * [How to take a screenshot of the browser](#msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser)
        * [How to capture browser console output](#msportalfx-test-debugging-how-to-capture-browser-console-output)
        * [Callstack](#msportalfx-test-debugging-callstack)
        * [Test output artifacts](#msportalfx-test-debugging-test-output-artifacts)
    * [Localization](#msportalfx-test-localization)
    * [User Management](#msportalfx-test-user-management)
    * [Configuration](#msportalfx-test-configuration)
        * [Configuration options](#msportalfx-test-configuration-configuration-options)
    * [Scenarios](#msportalfx-test-scenarios)
        * [Create](#msportalfx-test-scenarios-create)
        * [Browse](#msportalfx-test-scenarios-browse)
        * [How to test the grid in browse shows additional extension resource columns that are selected](#msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected)
        * [Blades](#msportalfx-test-scenarios-blades)
        * [Parts](#msportalfx-test-scenarios-parts)
        * [Command](#msportalfx-test-scenarios-command)
        * [Action Bar](#msportalfx-test-scenarios-action-bar)
        * [Delete](#msportalfx-test-scenarios-delete)
        * [Styling / layout regression detection](#msportalfx-test-scenarios-styling-layout-regression-detection)
        * [Locators](#msportalfx-test-scenarios-locators)
        * [Consuming Updates](#msportalfx-test-scenarios-consuming-updates)
        * [Mocking](#msportalfx-test-scenarios-mocking)
        * [Code Coverage](#msportalfx-test-scenarios-code-coverage)
        * [Contributing](#msportalfx-test-scenarios-contributing)
        * [To run the tests](#msportalfx-test-scenarios-to-run-the-tests)
        * [API Reference](#msportalfx-test-scenarios-api-reference)


<a name="msportalfx-test"></a>
<a name="msportalfx-test"></a>
# msportalfx-test

Generated on 2017-05-11

* [msportalfx-test](#msportalfx-test)
    * [Overview](#msportalfx-test-overview)
        * [Goals](#msportalfx-test-overview-goals)
        * [General Architecture](#msportalfx-test-overview-general-architecture)
    * [Getting Started](#msportalfx-test-getting-started)
        * [Installation](#msportalfx-test-getting-started-installation)
        * [Write a test](#msportalfx-test-getting-started-write-a-test)
        * [Add the configuration](#msportalfx-test-getting-started-add-the-configuration)
        * [Compile and run](#msportalfx-test-getting-started-compile-and-run)
        * [Updating](#msportalfx-test-getting-started-updating)
        * [More Examples](#msportalfx-test-getting-started-more-examples)
        * [Running tests in Visual Studio](#msportalfx-test-getting-started-running-tests-in-visual-studio)
    * [Side loading a local extension during the test session](#msportalfx-test-side-loading-a-local-extension-during-the-test-session)
    * [Running](#msportalfx-test-running)
        * [In Dev](#msportalfx-test-running-in-dev)
            * [From VS](#msportalfx-test-running-in-dev-from-vs)
            * [From cmdline](#msportalfx-test-running-in-dev-from-cmdline)
        * [CI](#msportalfx-test-running-ci)
            * [Cloudtest](#msportalfx-test-running-ci-cloudtest)
                * [Environment Setup](#msportalfx-test-running-ci-cloudtest-environment-setup)
                * [Running Tests](#msportalfx-test-running-ci-cloudtest-running-tests)
            * [Windows Azure Engineering System (WAES)](#msportalfx-test-running-ci-windows-azure-engineering-system-waes)
            * [Jenkins](#msportalfx-test-running-ci-jenkins)
            * [How to setup test run parallelization](#msportalfx-test-running-ci-how-to-setup-test-run-parallelization)
    * [Debugging](#msportalfx-test-debugging)
        * [debug tests 101](#msportalfx-test-debugging-debug-tests-101)
        * [debugging tests in VS Code](#msportalfx-test-debugging-debugging-tests-in-vs-code)
        * [Checking the result of the currently running test in code](#msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code)
        * [How to take a screenshot of the browser](#msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser)
        * [How to capture browser console output](#msportalfx-test-debugging-how-to-capture-browser-console-output)
        * [Callstack](#msportalfx-test-debugging-callstack)
        * [Test output artifacts](#msportalfx-test-debugging-test-output-artifacts)
    * [Localization](#msportalfx-test-localization)
    * [User Management](#msportalfx-test-user-management)
    * [Configuration](#msportalfx-test-configuration)
        * [Configuration options](#msportalfx-test-configuration-configuration-options)
            * [Behavior](#msportalfx-test-configuration-configuration-options-behavior)
            * [PortalContext](#msportalfx-test-configuration-configuration-options-portalcontext)
            * [Running tests against the Dogfood environment](#msportalfx-test-configuration-configuration-options-running-tests-against-the-dogfood-environment)
    * [Scenarios](#msportalfx-test-scenarios)
        * [Create](#msportalfx-test-scenarios-create)
            * [Opening the create blade from a deployed gallery package](#msportalfx-test-scenarios-create-opening-the-create-blade-from-a-deployed-gallery-package)
            * [Opening the create blade from a local gallery package](#msportalfx-test-scenarios-create-opening-the-create-blade-from-a-local-gallery-package)
            * [Validation State](#msportalfx-test-scenarios-create-validation-state)
            * [Get the validation state of fields on your create form](#msportalfx-test-scenarios-create-get-the-validation-state-of-fields-on-your-create-form)
            * [Wait on a fields validation state](#msportalfx-test-scenarios-create-wait-on-a-fields-validation-state)
        * [Browse](#msportalfx-test-scenarios-browse)
            * [How to test the context menu in browse shows your extensions commands?](#msportalfx-test-scenarios-browse-how-to-test-the-context-menu-in-browse-shows-your-extensions-commands)
            * [How to test the grid in browse shows the expected default columns for your extension resource](#msportalfx-test-scenarios-browse-how-to-test-the-grid-in-browse-shows-the-expected-default-columns-for-your-extension-resource)
        * [How to test the grid in browse shows additional extension resource columns that are selected](#msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected)
        * [Blades](#msportalfx-test-scenarios-blades)
            * [Blade navigation](#msportalfx-test-scenarios-blades-blade-navigation)
            * [Locating an open blade](#msportalfx-test-scenarios-blades-locating-an-open-blade)
            * [Common portal blades](#msportalfx-test-scenarios-blades-common-portal-blades)
                * [Opening the extensions Create blade](#msportalfx-test-scenarios-blades-common-portal-blades-opening-the-extensions-create-blade)
                * [Opening the Browse blade for your resource](#msportalfx-test-scenarios-blades-common-portal-blades-opening-the-browse-blade-for-your-resource)
                * [Opening a Resource Summary blade](#msportalfx-test-scenarios-blades-common-portal-blades-opening-a-resource-summary-blade)
                * [Spec Picker Blade](#msportalfx-test-scenarios-blades-common-portal-blades-spec-picker-blade)
                * [Settings Blade](#msportalfx-test-scenarios-blades-common-portal-blades-settings-blade)
                * [Properties Blade](#msportalfx-test-scenarios-blades-common-portal-blades-properties-blade)
                * [QuickStart Blade](#msportalfx-test-scenarios-blades-common-portal-blades-quickstart-blade)
                * [Users Blade](#msportalfx-test-scenarios-blades-common-portal-blades-users-blade)
                * [Move Resource Blade](#msportalfx-test-scenarios-blades-common-portal-blades-move-resource-blade)
            * [Blade Dialogs](#msportalfx-test-scenarios-blades-blade-dialogs)
        * [Parts](#msportalfx-test-scenarios-parts)
            * [How to get the reference to a part on a blade](#msportalfx-test-scenarios-parts-how-to-get-the-reference-to-a-part-on-a-blade)
            * [CollectionPart](#msportalfx-test-scenarios-parts-collectionpart)
            * [Grid](#msportalfx-test-scenarios-parts-grid)
                * [Finding a row within a grid](#msportalfx-test-scenarios-parts-grid-finding-a-row-within-a-grid)
            * [CreateComboBoxField](#msportalfx-test-scenarios-parts-createcomboboxfield)
            * [Editor](#msportalfx-test-scenarios-parts-editor)
                * [Can read and write content](#msportalfx-test-scenarios-parts-editor-can-read-and-write-content)
            * [essentials](#msportalfx-test-scenarios-parts-essentials)
                * [Essentials tests](#msportalfx-test-scenarios-parts-essentials-essentials-tests)
        * [Command](#msportalfx-test-scenarios-command)
        * [Action Bar](#msportalfx-test-scenarios-action-bar)
        * [Delete](#msportalfx-test-scenarios-delete)
        * [Styling / layout regression detection](#msportalfx-test-scenarios-styling-layout-regression-detection)
            * [...](#msportalfx-test-scenarios-styling-layout-regression-detection-)
        * [Locators](#msportalfx-test-scenarios-locators)
        * [Consuming Updates](#msportalfx-test-scenarios-consuming-updates)
        * [Mocking](#msportalfx-test-scenarios-mocking)
            * [How to show mock data into the Portal](#msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal)
                * [Mocking ARM](#msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal-mocking-arm)
        * [Code Coverage](#msportalfx-test-scenarios-code-coverage)
            * [Interop, how to run .NET code from your tests](#msportalfx-test-scenarios-code-coverage-interop-how-to-run-net-code-from-your-tests)
        * [Contributing](#msportalfx-test-scenarios-contributing)
            * [To enlist](#msportalfx-test-scenarios-contributing-to-enlist)
            * [To build the source](#msportalfx-test-scenarios-contributing-to-build-the-source)
            * [To setup the tests](#msportalfx-test-scenarios-contributing-to-setup-the-tests)
        * [To run the tests](#msportalfx-test-scenarios-to-run-the-tests)
            * [Authoring documents](#msportalfx-test-scenarios-to-run-the-tests-authoring-documents)
            * [Generating the docs](#msportalfx-test-scenarios-to-run-the-tests-generating-the-docs)
            * [To submit your contribution](#msportalfx-test-scenarios-to-run-the-tests-to-submit-your-contribution)
            * [Questions?](#msportalfx-test-scenarios-to-run-the-tests-questions)
        * [API Reference](#msportalfx-test-scenarios-api-reference)


<a name="msportalfx-test-overview"></a>
<a name="msportalfx-test-overview"></a>
## Overview

MsPortalFx-Test is an end-to-end test framework that runs tests against the Microsoft Azure Portal interacting with it as a user would. 

<a name="msportalfx-test-overview-goals"></a>
<a name="msportalfx-test-overview-goals"></a>
#### Goals

- Strive for zero breaking changes to partner team CI
- Develop tests in the same language as the extension
- Focus on partner needs rather than internal portal needs
- Distributed independently from the SDK
- Uses an open source contribution model
- Performant
- Robust
- Great Docs

<a name="msportalfx-test-overview-general-architecture"></a>
<a name="msportalfx-test-overview-general-architecture"></a>
#### General Architecture
3 layers of abstraction (note the names may change but the general idea should be the same).  There may also be some future refactoring to easily differentiate between the layers.
- Test layer 

    - Useful wrappers for testing common functionality.  Will retry if necessary.  Throws if the test/verification fails.  
    - Should be used in writing tests
    - Built upon the action and control layers
    - EG: parts.canPinAllBladeParts
    
- Action layer 

    - Performs an action and verifies it was successful.  Will retry if necessary.
    - Should be used in writing tests
    - Built upon the controls layer
    - EG: portal.openBrowseBlade
    
- Controls layer 

    - The basic controls used in the portal (eg blades, checkboxes, textboxes, etc).  Little to no retry logic.  Should be used mainly for composing the actions and tests layers.
    - Should be used for writing test and action layers.  Should not be used directly by tests in most cases.
    - Built upon webdriver primitives
    - EG: part, checkbox, etc  


<a name="msportalfx-test-getting-started"></a>
<a name="msportalfx-test-getting-started"></a>
## Getting Started

<a name="msportalfx-test-getting-started-installation"></a>
<a name="msportalfx-test-getting-started-installation"></a>
### Installation

1. Install [Node.js](https://nodejs.org) if you have not done so. This will also install npm, which is the package manager for Node.js.  We have only verified support for LTS Node versions 4.5 and 5.1 which can be found in the "previous downloads" section.  Newer versions of Node are known to have compilation errors.  
1. Install [Node Tools for Visual Studio](https://www.visualstudio.com/en-us/features/node-js-vs.aspx)
1. Install [TypeScript](http://www.typescriptlang.org/) 1.8.10 or greater.
1. Verify that your:
    - node version is v4.5 or v5.1 using `node -v`
    - npm version is 3.10.6 or greater using `npm -v`.  To update npm version use `npm install npm -g`
    - tsc version is 1.8.10 or greater using tsc -v.

1. Open a command prompt and create a directory for your test files.

		md e2etests 

1. Switch to the new directory and install the msportalfx-test module via npm:

		cd e2etests
		npm install msportalfx-test --no-optional

1. The msportalfx-test module comes with useful TypeScript definitions and its dependencies. To make them available to your tests, we recommend that you use the [typings](https://www.npmjs.com/package/typings) Typescript Definition Manager.

1. First install the [typings](https://www.npmjs.com/package/typings) Typescript Definition Manager globally:

		npm install typings -g

	Next copy the provided **typings.json** file from the **node_modules/msportalfx-test/typescript** folder to your test root directory and use *typings* to install the provided definitions:

		*copy typings.json to your test root directory*
        *navigate to your test root directory*
		typings install

1. MsPortalFx-Test needs a WebDriver server in order to be able to drive the browser. Currently only [ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver) is supported, so downloaded it and place it in your machine's PATH or just install it from the [chromedriver Node module](https://www.npmjs.com/package/chromedriver).  You may also need a C++ compiler (Visual Studio includes one):

		npm install chromedriver


<a name="msportalfx-test-getting-started-write-a-test"></a>
<a name="msportalfx-test-getting-started-write-a-test"></a>
### Write a test
You'll need an existing cloud service for the test you'll write below, so if you don't have one please go to the Azure Portal and [create a new cloud service](https://ms.portal.azure.com/#create/Microsoft.CloudService). Write down the dns name of your cloud service to use it in your test.

We'll use the [Mocha](https://mochajs.org/) testing framework to layout the following test, but you could use any framework that supports Node.js modules and promises. Let's install Mocha (its typescript definitions are already installed from the typings install earlier):

	npm install mocha

Now, create a **portaltests.ts** file in your e2etests directory and paste the following:

```ts
/// <reference path="./typings/index.d.ts" />

import assert = require('assert');
import testFx = require('MsPortalFx-Test');
import nconf = require('nconf');
import until = testFx.until;

describe('Cloud Service Tests', function () {
	this.timeout(0);
	
	it('Can Browse To A Cloud Service', () => {

		
        // Load command line arguments, environment variables and config.json into nconf
        nconf.argv()
            .env()
            .file(__dirname + "/config.json");

        //provide windows credential manager as a fallback to the above three
        nconf[testFx.Utils.NConfWindowsCredentialManager.ProviderName] = testFx.Utils.NConfWindowsCredentialManager;
        nconf.use(testFx.Utils.NConfWindowsCredentialManager.ProviderName);
        
		
		testFx.portal.portalContext.signInEmail = 'johndoe@outlook.com';
		testFx.portal.portalContext.signInPassword = nconf.get('msportalfx-test/johndoe@outlook.com/signInPassword');
		
		// Update this variable to use the dns name of your actual cloud service 
		let dnsName = "mycloudservice";
		
		return testFx.portal.openBrowseBlade('microsoft.classicCompute', 'domainNames', "Cloud services (classic)").then((blade) => {
			return blade.filterItems(dnsName);
		}).then((blade) => {
            return testFx.portal.wait<testFx.Controls.GridRow>(() => {
                return blade.grid.rows.count().then((count) => {
                    return count === 1 ? blade.grid.rows.first() : null;
                });
            }, null, "Expected only one row matching '" + dnsName + "'.");
        }).then((row) => {
            return row.click();			
		}).then(() => {            
			let summaryBlade = testFx.portal.blade({ title: dnsName + " - Production" });
            return testFx.portal.wait(until.isPresent(summaryBlade));
		}).then(() => {
			return testFx.portal.quit();
		});
	});
});
```
1. write credentials to the windows credential manager

```
    cmdkey /generic:msportalfx-test/johndoe@outlook.com/signInPassword /user:johndoe@outlook.com /pass:somePassword
```

Remember to replace "mycloudservice" with the dns name of your actual cloud service.

In this test we start by importing the **MsPortalFx-Test** module. Then the credentials are specified for the user that will sign in to the Portal. These should be the credentials of a user that already has an active Azure subscription. 

After that we can use the Portal object to drive a test scenario that opens the Cloud Services Browse blade, filters the list of cloud services, checks that the grid has only one row after the filter, selects the only row and waits for the correct blade to open. Finally, the call to quit() closes the browser.

<a name="msportalfx-test-getting-started-add-the-configuration"></a>
<a name="msportalfx-test-getting-started-add-the-configuration"></a>
### Add the configuration

Create a file named **config.json** next to portaltests.ts. Paste this in the file:

	```json

		{
		"capabilities": {
			"browserName": "chrome"
		},
		"portalUrl": "https://portal.azure.com"
		}

	```	

This configuration tells MsPortalFx-Test that Google Chrome should be used for the test session and [https://portal.azure.com](https://portal.azure.com) should be the Portal under test. 

<a name="msportalfx-test-getting-started-compile-and-run"></a>
<a name="msportalfx-test-getting-started-compile-and-run"></a>
### Compile and run
Compile your TypeScript test file:

	tsc portaltests.ts --module commonjs

...and then run Mocha against the generated JavaScript file (note using an elevated command prompt may cause Chrome to crash.  Use a non-elevated command prompt for best results):

	node_modules\.bin\mocha portaltests.js

The following output will be sent to your console as the test progresses:

	  Portal Tests
	Opening the Browse blade for the microsoft.classicCompute/domainNames resource type...
	Starting the ChromeDriver process...
	Performing SignIn...
	Waiting for the Portal...
	Waiting for the splash screen to go away...
	Applying filter 'mycloudservice'...
	    √ Can Browse To A Cloud Service (37822ms)	
	
	  1 passing (38s)

If you run into a compilation error with node.d.ts, verify that the tsc version you are using is 1.8.x (we do not currently support Typescript 2+).  You can check the version by running:
    
    tsc --version

If the version is incorrect, then you may need to adjust your path variables or directly call the correct version of tsc.exe  

<a name="msportalfx-test-getting-started-updating"></a>
<a name="msportalfx-test-getting-started-updating"></a>
### Updating

1.  In order to keep up to date with the latest changes, we recommend that you update whenever a new version of MsportalFx-Test is released.  npm install will automatically pull the latest version of msportalfx-test. 
 
        Make sure to copy typescript definition files in your *typings\* folder from the updated version in *\node_modules\msportalfx-test\typescript*.  
      
<a name="msportalfx-test-getting-started-more-examples"></a>
<a name="msportalfx-test-getting-started-more-examples"></a>
### More Examples
More examples can be found
- within this document
- in the [msportalfx-test /test folder] (https://github.com/Azure/msportalfx-test/tree/master/test) 
- and the [Contacts Extension Tests](http://vstfrd:8080/Azure/One/_git/AzureUX-PortalFx#path=%2Fsrc%2FSDK%2FAcceptanceTests%2FExtensions%2FContactsExtension%2FTests). 

If you don't have access, please follow the enlistment instructions below.
 
<a name="msportalfx-test-getting-started-running-tests-in-visual-studio"></a>
<a name="msportalfx-test-getting-started-running-tests-in-visual-studio"></a>
### Running tests in Visual Studio
 
1. Install [Node Tools for Visual Studio](https://www.visualstudio.com/en-us/features/node-js-vs.aspx) (Note that we recommend using the Node.js “LTS” versions rather than the “Stable” versions since sometimes NTVS doesn’t work with newer Node.js versions.)

1. Once that’s done, you should be able to open Visual Studio and then create new project: *New -> Project -> Installed, Templates, TypeScript, Node.js -> From Existing Node.js code*.

![NewTypeScriptNodeJsExistingProject][NewTypeScriptNodeJsExistingProject]

1. Then open the properties on your typescript file and set the TestFramework property to “mocha”.

![FileSetTestFrameworkPropertyMocha][FileSetTestFrameworkPropertyMocha]

1. Once that is done you should be able to build and then see your test in the test explorer.  If you don’t see your tests, then make sure you don’t have any build errors.  You can also try restarting Visual Studio to see if that makes them show up.  

[FileSetTestFrameworkPropertyMocha]: ../media/msportalfx-test/FileSetTestFrameworkPropertyMocha.png
[NewTypeScriptNodeJsExistingProject]: ../media/msportalfx-test/NewTypeScriptNodeJsExistingProject.png


<a name="msportalfx-test-side-loading-a-local-extension-during-the-test-session"></a>
<a name="msportalfx-test-side-loading-a-local-extension-during-the-test-session"></a>
## Side loading a local extension during the test session

You can use MsPortalFx-Test to write end to end tests that side load your local extension in the Portal. You can do this by specifying additional options in the Portal object. If you have not done so, please take a look at the *Installation* section of [this page](https://auxdocs.azurewebsites.net/en-us/documentation/articles/portalfx-testing-getting-started) to learn how to get started with MsPortalFx-Test. 

We'll write a test that verifies that the Browse experience for our extension has been correctly implemented. But before doing that we should have an extension to test and something to browse to, so let's work on those first.

**To prepare the target extension and resource:**

1. Create a new Portal extension in Visual Studio following [these steps](https://auxdocs.azurewebsites.net/en-us/documentation/articles/portalfx-creating-extensions) and then hit CTRL+F5 to get it up and running. For the purpose of this example we named the extension 'LocalExtension' and we made it run in the default [https://localhost:44300](https://localhost:44300) address. 

1. That should have taken you to the Portal, so sign in and then go to New --> Marketplace --> Local Development --> LocalExtension --> Create.

1. In the **My Resource** blade, enter **theresource** as the resource name, complete the required fields and hit Create.

1. Wait for the resource to get created.


**To write a test verifies the Browse experience while side loading your local extension:**

1. Create a new TypeScript file called **localextensiontests.ts**.
 
1. In the created file, import the MsPortalFx-Test module and layout the Mocha test:

	```ts
	/// <reference path="./typings/index.d.ts" />
	
	import assert = require('assert');
	import testFx = require('MsPortalFx-Test');
	import until = testFx.until;
	
	describe('Local Extension Tests', function () {
		this.timeout(0);
	
		it('Can Browse To The Resource Blade', () => {
			
		});
	});
	```

1. In the **Can Browse To The Resource Blade** test body,  specify the credentials for the test session (replace with your actual Azure credentials):
 
	```ts
	// Hardcoding credentials to simplify the example, but you should never hardcode credentials
	testFx.portal.portalContext.signInEmail = 'johndoe@outlook.com';
	testFx.portal.portalContext.signInPassword = '12345';
	```

1. Now, use the **features** option of the portal.PortalContext object to enable the **canmodifyextensions** feature flag and use the **testExtensions** option to specify the name and address of the local extension to load:
 
	```ts
	testFx.portal.portalContext.features = [{ name: "feature.canmodifyextensions", value: "true" }];
	testFx.portal.portalContext.testExtensions = [{ name: 'LocalExtension', uri: 'https://localhost:44300/' }];
	```

1. Let's also declare a variable with the name of the resource that the test will browse to:

	```ts
	let resourceName = 'theresource';
	```
 
1. To be able to open the browse blade for our resource we'll need to know three things: The resource provider, the resource type and the title of the blade. You can get all that info from the Browse PDL implementation of your extension. In this case the resource provider is **Microsoft.PortalSdk**, the resource type is **rootResources** and the browse blade title is **My Resources**. With that info we can call the **openBrowseBlade** function of the Portal object:

	```ts
	return testFx.portal.openBrowseBlade('Microsoft.PortalSdk', 'rootResources', 'My Resources')
	```
 
1. From there on we can use the returned Blade object to filter the list, verify that only one row remains after filtering and select that row:
	
	```ts
	.then((blade) => {
        return testFx.portal.wait<testFx.Controls.GridRow>(() => {
            return blade.grid.rows.count().then((count) => {
                return count === 1 ? blade.grid.rows.first() : null;
            });
        }, null, "Expected only one row matching '" + resourceName + "'.");
    }).then((row) => {
        return row.click();				
	})
	```

1. And finally we'll verify the correct blade opened and will close the Portal when done:

	```ts
	.then(() => {
		let summaryBlade = testFx.portal.blade({ title: resourceName });
		return testFx.portal.wait(until.isPresent(summaryBlade));
	}).then(() => {
		return testFx.portal.quit();
	});
	```

1. Here for the complete test:

```ts
/// <reference path="./typings/index.d.ts" />

import assert = require('assert');
import testFx = require('MsPortalFx-Test');
import until = testFx.until;

describe('Local Extension Tests', function () {
    this.timeout(0);

    it('Can Browse To The Resource Blade', () => {
        // Hardcoding credentials to simplify the example, but you should never hardcode credentials
        testFx.portal.portalContext.signInEmail = 'johndoe@outlook.com';
        testFx.portal.portalContext.signInPassword = '12345';
        testFx.portal.portalContext.features = [{ name: "feature.canmodifyextensions", value: "true" }];
        testFx.portal.portalContext.testExtensions = [{ name: 'LocalExtension', uri: 'https://localhost:44300/' }];
        
        let resourceName = 'theresource';
        
        return testFx.portal.openBrowseBlade('Microsoft.PortalSdk', 'rootResources', 'My Resources').then((blade) => {
            return blade.filterItems(resourceName);
        }).then((blade) => {
            return testFx.portal.wait<testFx.Controls.GridRow>(() => {
                return blade.grid.rows.count().then((count) => {
                    return count === 1 ? blade.grid.rows.first() : null;
                });
            }, null, "Expected only one row matching '" + resourceName + "'.");
        }).then((row) => {
            return row.click();				
        }).then(() => {
            let summaryBlade = testFx.portal.blade({ title: resourceName });
            return testFx.portal.wait(until.isPresent(summaryBlade));
        }).then(() => {
            return testFx.portal.quit();
        });
    });
});
```

**To add the required configuration and run the test:**

1. Create a file named **config.json** next to localextensiontests.ts. Paste this in the file:

	```json
	{
	  "capabilities": {
	    "browserName": "chrome"
	  },
	  "portalUrl": "https://portal.azure.com"
	}
	```		

1. Compile your TypeScript test file:

		tsc localextensiontests.ts --module commonjs

1. Run Mocha against the generated JavaScript file:

		node_modules\.bin\mocha localextensiontests.js

The following output will be sent to your console as the test progresses:

	  Local Extension Tests
	Opening the Browse blade for the Microsoft.PortalSdk/rootResources resource type...
	Starting the ChromeDriver process...
	Performing SignIn...
	Waiting for the Portal...
	Waiting for the splash screen...
	Allowing trusted extensions...
	Waiting for the splash screen to go away...
	Applying filter 'theresource'...
	    √ Can Browse To The Resource Blade (22872ms)
	
	
	  1 passing (23s)

<a name="msportalfx-test-running"></a>
<a name="msportalfx-test-running"></a>
## Running

<a name="msportalfx-test-running-in-dev"></a>
<a name="msportalfx-test-running-in-dev"></a>
### In Dev

<a name="msportalfx-test-running-in-dev-from-vs"></a>
<a name="msportalfx-test-running-in-dev-from-vs"></a>
#### From VS

<a name="msportalfx-test-running-in-dev-from-cmdline"></a>
<a name="msportalfx-test-running-in-dev-from-cmdline"></a>
#### From cmdline

<a name="msportalfx-test-running-ci"></a>
<a name="msportalfx-test-running-ci"></a>
### CI

<a name="msportalfx-test-running-ci-cloudtest"></a>
<a name="msportalfx-test-running-ci-cloudtest"></a>
#### Cloudtest

Running mocha nodejs tests in cloudtest requires a bit of engineering work to set up the test VM. Unfortunetly, the nodejs test adaptor cannot be used with vs.console.exe since it requires a full installation of Visual Studio which is absent on the VMs. Luckily, we can run a script to set up our environment and then the Exe Execution type for our TestJob against the powershell/cmd executable.

<a name="msportalfx-test-running-ci-cloudtest-environment-setup"></a>
<a name="msportalfx-test-running-ci-cloudtest-environment-setup"></a>
##### Environment Setup
Nodejs (and npm) is already installed on the cloudtest VMs. Chrome is not installed by default, so we can include the chrome executable in our build drop for quick installation.

setup.bat
```
cd UITests
call npm install --no-optional
call npm install -g typescript
call "%APPDATA%\npm\tsc.cmd"
call chrome_installer.exe /silent /install
exit 0
```

<a name="msportalfx-test-running-ci-cloudtest-running-tests"></a>
<a name="msportalfx-test-running-ci-cloudtest-running-tests"></a>
##### Running Tests
Use the Exe execution type in your TestJob to specify the powershell (or cmd) exe. Then, point to a script which will run your tests:

TestGroup.xml
```
<TestJob Name="WorkspaceExtension.UITests" Type="SingleBox" Size="Small" Tags="Suite=Suite0">
    <Execution Type="Exe" Path="C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" Args='[SharedWorkingDirectory]\UITests\RunTests.ps1' />
</TestJob>
```

At the end of your script you will need to copy the resulting trx file to the TestResults folder where Cloudtest expects to pick it up from. To generate a trx file, we used the mocha-trx-reporter npm package. To pass secrets to cloudtest, you can either use test secretstore which has been configured to use a certificate installed on all cloudtest VMs for particular paths, or one of the other solutions shown [here](https://stackoverflow.microsoft.com/questions/11589/getting-ais-token-in-cloudtest-machine/11665#11665)

RunTests.ps1
```
cd ..\UITests

$env:USER_EMAIL = ..\StashClient\StashClient.exe -env:test pgetdecrypted -name:Your/Secret/Path -cstorename:My -cstorelocation:LocalMachine -cthumbprint:0000000000000000000000000000000000000000

$env:USER_PASSWORD = ..\StashClient\StashClient.exe -env:test pgetdecrypted -name:Your/Secret/Path -cstorename:My -cstorelocation:LocalMachine -cthumbprint:0000000000000000000000000000000000000000

$env:TEST_ENVIRONMENT = [environment]::GetEnvironmentVariable("TEST_ENVIRONMENT","Machine")

mocha WorkspaceTests.js --reporter mocha-trx-reporter --reporter-file ./TestResults/result.trx

xcopy TestResults\* ..\TestResults
```

To pass non-secret parameters to your cloudtest session (and the msportalfx-tests) use the props switch when kicking off a cloudtest session. The properties will become machine level environment variables on your cloudtest VM. Once these are set as environment variables of the session, you can use nconf to pick them up in your UI test configuration.
```
ct -t "amd64\CloudTest\TestMap.xml" -tenant Default -BuildId "GUID" -props worker:TEST_ENVIRONMENT=canary
```

<a name="msportalfx-test-running-ci-windows-azure-engineering-system-waes"></a>
<a name="msportalfx-test-running-ci-windows-azure-engineering-system-waes"></a>
#### Windows Azure Engineering System (WAES)

See [WAES](http://aka.ms/WAES)

<a name="msportalfx-test-running-ci-jenkins"></a>
<a name="msportalfx-test-running-ci-jenkins"></a>
#### Jenkins

<a name="msportalfx-test-running-ci-how-to-setup-test-run-parallelization"></a>
<a name="msportalfx-test-running-ci-how-to-setup-test-run-parallelization"></a>
#### How to setup test run parallelization

<a name="msportalfx-test-debugging"></a>
<a name="msportalfx-test-debugging"></a>
## Debugging

<a name="msportalfx-test-debugging-debug-tests-101"></a>
<a name="msportalfx-test-debugging-debug-tests-101"></a>
### debug tests 101

<a name="msportalfx-test-debugging-debugging-tests-in-vs-code"></a>
<a name="msportalfx-test-debugging-debugging-tests-in-vs-code"></a>
### debugging tests in VS Code
If you run mocha with the --debug-brk flag, you can press F5 and the project will attach to a debugger. 

<a name="msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code"></a>
<a name="msportalfx-test-debugging-checking-the-result-of-the-currently-running-test-in-code"></a>
### Checking the result of the currently running test in code

Sometimes it is useful to get the result of the currently running test, for example: you want to take a screenshot only when the test fails.

```ts

    afterEach(function () {
        if (this.currentTest.state === "failed") {
            return testSupport.GatherTestFailureDetails(this.currentTest.title);
        }
    });

```

One thing to watch out for in typescript is how lambda functions, "() => {}", behave.  Lambda functions (also called "fat arrow" sometimes) in Typescript capture the "this" variable from the surrounding context.  This can cause problems when trying to access Mocha's current test state.  See [arrow functions](https://basarat.gitbooks.io/typescript/content/docs/arrow-functions.html) for details.

<a name="msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser"></a>
<a name="msportalfx-test-debugging-how-to-take-a-screenshot-of-the-browser"></a>
### How to take a screenshot of the browser

This is an example of how to take a screenshot of what is currently displayed in the browser.  

```ts
//1. import test fx
import testFx = require('MsPortalFx-Test');
...
    var screenshotPromise = testFx.portal.takeScreenshot(ScreenshotTitleHere);
```

Taking a screenshot when there is a test failure is a handy way to help diagnose issues.  If you are using the mocha test runner, then you can do the following to take a screenshot whenever a test fails:

```ts
import testFx = require('MsPortalFx-Test');
...

    afterEach(function () {
        if (this.currentTest.state === "failed") {
            return testSupport.GatherTestFailureDetails(this.currentTest.title);
        }
    });

```

<a name="msportalfx-test-debugging-how-to-capture-browser-console-output"></a>
<a name="msportalfx-test-debugging-how-to-capture-browser-console-output"></a>
### How to capture browser console output

When trying to identify reasons for failure of a test its useful to capture the console logs of the browser that was used to execute your test. You can capture the logs at a given level e.g error, warning, etc or at all levels using the LogLevel parameter. The following example demonstrates how to call getBrowserLogs and how to work with the result. getBrowserLogs will return a Promise of string[] which when resolved will contain the array of logs that you can view during debug or write to the test console for later analysis.    

```ts
import testFx = require('MsPortalFx-Test');
...

        return testFx.portal.goHome(20000).then(() => {
            return testFx.portal.getBrowserLogs(LogLevel.All);
        }).then((logs) => {
            assert.ok(logs.length > 0, "Expected to collect at least one log.");
        });

```

<a name="msportalfx-test-debugging-callstack"></a>
<a name="msportalfx-test-debugging-callstack"></a>
### Callstack

<a name="msportalfx-test-debugging-test-output-artifacts"></a>
<a name="msportalfx-test-debugging-test-output-artifacts"></a>
### Test output artifacts

<a name="msportalfx-test-localization"></a>
<a name="msportalfx-test-localization"></a>
## Localization

<a name="msportalfx-test-user-management"></a>
<a name="msportalfx-test-user-management"></a>
## User Management

<a name="msportalfx-test-configuration"></a>
<a name="msportalfx-test-configuration"></a>
## Configuration

<a name="msportalfx-test-configuration-configuration-options"></a>
<a name="msportalfx-test-configuration-configuration-options"></a>
### Configuration options
This document will describe the behavior and list common configuration settings used by the MsPortalFx-Test framework.

<a name="msportalfx-test-configuration-configuration-options-behavior"></a>
<a name="msportalfx-test-configuration-configuration-options-behavior"></a>
#### Behavior
The test framework will search for a config.json in the current working directory (usually the directory the test is invoked from).  If no config.json is found then it will check the parent folder for a config.json (and so on...).

<a name="msportalfx-test-configuration-configuration-options-portalcontext"></a>
<a name="msportalfx-test-configuration-configuration-options-portalcontext"></a>
#### PortalContext
This file contains a list of configuration values used by the test framework for context when running tests against the portal.
These values are mutable to allow test writers to set the values in cases where they prefer not to store them in the config.json.
**We strongly recommend that passwords should not be stored in the config.json file.**

```ts
﻿import TestExtension = require("./TestExtension");
import Feature = require("./Feature");
import BrowserResolution = require("./BrowserResolution");
import Timeout = require("./Timeout");

/**
 * Represents The set of options used to configure a Portal instance.
 */
interface PortalContext {
    /**
     * The set of capabilities enabled in the webdriver session. 
     * For a list of available capabilities, see https://github.com/SeleniumHQ/selenium/wiki/DesiredCapabilities
     */    
    capabilities: {
        /**
         * The name of the browser being used; should be one of {chrome} 
         */
        browserName: string;

        /**
         * Chrome-specific supported capabilities. 
         */
        chromeOptions: {
            /**
             * List of command-line arguments to use when starting Chrome.
             */
            args: string[]
        };

        /**
         * The desired starting browser's resolution in pixels.
         */
        browserResolution: BrowserResolution;
    },
    /**
     * The path to the ChromeDriver binary. 
     */
    chromeDriverPath?: string;
    /**
     * The url of the Portal.
     */
    portalUrl: string;
    /**
     * The url of the page where signin is performed.
     */
    signInUrl?: string;
    /**
     * Email of the user used to sign in to the Portal.
     */
    signInEmail?: string;
    /**
     * Password of the user used to sign in to the Portal.
     */
    signInPassword?: string;    
    /**
     * The set of features to enable while navigating within the Portal.
     */
    features?: Feature[];
    /**
     * The list of patch files to load within the Portal.
     */
    patches?: string[];
    /**
     * The set of extensions to side load while navigating within the Portal.
     */
    testExtensions?: TestExtension[];
    /**
     * The set of timeouts used to override the default timeouts.
     * e.g. 
     * timeouts: {
     *      timeout: 15000  //Overrides the default short timeout of 10000 (10 seconds).
     *      longTimeout: 70000 //Overrides the default long timetout of 60000 (60 seconds).
     * }
     */
    timeouts?: Timeout;
}

export = PortalContext;
```

<a name="msportalfx-test-configuration-configuration-options-running-tests-against-the-dogfood-environment"></a>
<a name="msportalfx-test-configuration-configuration-options-running-tests-against-the-dogfood-environment"></a>
#### Running tests against the Dogfood environment
In order to run tests against the Dogfood test environment, you will need to update the follow configuration settings in the config.json:

```json
{
  portalUrl: https://df.onecloud.azure-test.net/,
  signInUrl: https://login.windows-ppe.net/
}
```

<a name="msportalfx-test-scenarios"></a>
<a name="msportalfx-test-scenarios"></a>
## Scenarios

<a name="msportalfx-test-scenarios-create"></a>
<a name="msportalfx-test-scenarios-create"></a>
### Create

<a name="msportalfx-test-scenarios-create-opening-the-create-blade-from-a-deployed-gallery-package"></a>
<a name="msportalfx-test-scenarios-create-opening-the-create-blade-from-a-deployed-gallery-package"></a>
#### Opening the create blade from a deployed gallery package

To open/navigate to the create blade a gallery package previously deployed to the Azure Marketplace you can use `portal.openGalleryCreateBlade`.  The returned promise will resolve with the CreateBlade defined by that gallery package. 

```ts 
import TestFx = require('MsPortalFx-Test');
...
FromLocalPackage
        return testFx.portal.openGalleryCreateBladeFromLocalPackage(
            extensionResources.samplesExtensionStrings.Engine.engineV3,     //title of the item in the marketplace e.g "EngineV3"
            extensionResources.samplesExtensionStrings.Engine.createEngine, //the title of the blade that will be opened e.g "Create Engine"
            10000)                                                          //an optional timeout to wait on the blade
        .then(() => createEngineBlade.checkFieldValidation())
        .then(() => createEngineBlade.fillRequiredFields(resourceName, "600cc", "type1", subscriptionName, resourceName, locationDescription))
        .then(() => createEngineBlade.actionBar.pinToDashboardCheckbox.click())
        .then(() => createEngineBlade.actionBar.createButton.click())
        .then(() => testFx.portal.wait(until.isPresent(testFx.portal.blade({ title: resourceName })), 120000, "The resource blade was not opened, could be deployment timeout."));
        
...
```

<a name="msportalfx-test-scenarios-create-opening-the-create-blade-from-a-local-gallery-package"></a>
<a name="msportalfx-test-scenarios-create-opening-the-create-blade-from-a-local-gallery-package"></a>
#### Opening the create blade from a local gallery package

To open/navigate to the create blade a local gallery package that has been side loaded into the portal along with your extension you can use `portal.openGalleryCreateBladeFromLocalPackage`.  The returned promise will resolve with the CreateBlade defined by that gallery package. 

```ts 
import TestFx = require('MsPortalFx-Test');
...

        return testFx.portal.openGalleryCreateBladeFromLocalPackage(
            extensionResources.samplesExtensionStrings.Engine.engineV3,     //title of the item in the marketplace e.g "EngineV3"
            extensionResources.samplesExtensionStrings.Engine.createEngine, //the title of the blade that will be opened e.g "Create Engine"
            10000)                                                          //an optional timeout to wait on the blade
        .then(() => createEngineBlade.checkFieldValidation())
        .then(() => createEngineBlade.fillRequiredFields(resourceName, "600cc", "type1", subscriptionName, resourceName, locationDescription))
        .then(() => createEngineBlade.actionBar.pinToDashboardCheckbox.click())
        .then(() => createEngineBlade.actionBar.createButton.click())
        .then(() => testFx.portal.wait(until.isPresent(testFx.portal.blade({ title: resourceName })), 120000, "The resource blade was not opened, could be deployment timeout."));
        
...
```

<a name="msportalfx-test-scenarios-create-validation-state"></a>
<a name="msportalfx-test-scenarios-create-validation-state"></a>
#### Validation State

<a name="msportalfx-test-scenarios-create-get-the-validation-state-of-fields-on-your-create-form"></a>
<a name="msportalfx-test-scenarios-create-get-the-validation-state-of-fields-on-your-create-form"></a>
#### Get the validation state of fields on your create form

`FormElement` exposes two useful functions for working with the ValidationState of controls. 

The function `getValidationState` returns a promise that resolves with the current state of the control and can be used as follows

```ts
import TestFx = require('MsPortalFx-Test');
...

            //click the createButton on the create blade to fire validation
            .then(() => this.actionBar.createButton.click())
            //get the validation state of the control
            .then(() => this.name.getValidationState())
            //assert state matches expected
            .then((state) => assert.equal(state, testFx.Constants.ControlValidationState.invalid, "name should have invalid state"));
...

```

<a name="msportalfx-test-scenarios-create-wait-on-a-fields-validation-state"></a>
<a name="msportalfx-test-scenarios-create-wait-on-a-fields-validation-state"></a>
#### Wait on a fields validation state

The function `waitOnValidationState(someState, optionalTimeout)` returns a promise that resolves when the current state of the control is equivalent to someState supplied.  This is particularly useful for scenarions where you may be performing serverside validation and the control remains in a pending state for the duration of the network IO.

```ts
import TestFx = require('MsPortalFx-Test');
...

            //change the value to initiate validation
            .then(() => this.name.sendKeys(nameTxt + webdriver.Key.TAB))
            //wait for the control to reach the valid state
            .then(() => this.name.waitOnValidationState(testFx.Constants.ControlValidationState.valid))
...

```

<a name="msportalfx-test-scenarios-browse"></a>
<a name="msportalfx-test-scenarios-browse"></a>
### Browse

<a name="msportalfx-test-scenarios-browse-how-to-test-the-context-menu-in-browse-shows-your-extensions-commands"></a>
<a name="msportalfx-test-scenarios-browse-how-to-test-the-context-menu-in-browse-shows-your-extensions-commands"></a>
#### How to test the context menu in browse shows your extensions commands?

There is a simple abstraction available in MsPortalFx.Tests.Browse.  You can use it as follows: 

```ts
//1. import test fx
import TestFx = require('MsPortalFx-Test');

...

it("Can Use Context Click On Browse Grid Rows", () => {
    ...
//2. Setup an array of commands that are expected to be present in the context menu
//  and call the contextMenuContainsExpectedCommands on Tests.Browse. 
    //  The method will assert expectedCommands match what was displayed  
    
        let expectedContextMenuCommands: Array<string> = [
            PortalFxResources.pinToDashboard,
            extensionResources.deleteLabel
        ];
        return testFx.Tests.Browse.contextMenuContainsExpectedCommands(
            resourceProvider, // the resource provider e.g "microsoft.classicCompute"
            resourceType, // the resourceType e.g "domainNames"
            resourceBladeTitle, // the resource blade title "Cloud services (classic)"
            expectedContextMenuCommands) 
});
```

<a name="msportalfx-test-scenarios-browse-how-to-test-the-grid-in-browse-shows-the-expected-default-columns-for-your-extension-resource"></a>
<a name="msportalfx-test-scenarios-browse-how-to-test-the-grid-in-browse-shows-the-expected-default-columns-for-your-extension-resource"></a>
#### How to test the grid in browse shows the expected default columns for your extension resource

There is a simple abstraction available in MsPortalFx.Tests.Browse.  You can use it as follows:

```ts
//1. import test fx
import TestFx = require('MsPortalFx-Test');

...

it("Browse contains default columns with expected column header", () => {
 // 2. setup an array of expectedDefaultColumns to be shown in browse
    
    const expectedDefaultColumns: Array<testFx.Tests.Browse.ColumnTestOptions> =
        [
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.name },
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.subscription },
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.location },
        ];

// 3. call the TestFx.Tests.Browse.containsExpectedDefaultColumns function.
// This function this will perform a reset columns action in browse and then assert
// the expectedDefaultColumns match what is displayed
        
        return testFx.Tests.Browse.containsExpectedDefaultColumns(
            resourceProvider,
            resourceType,
            resourceBladeTitle,
            expectedDefaultColumns);
}
```

<a name="msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected"></a>
<a name="msportalfx-test-scenarios-how-to-test-the-grid-in-browse-shows-additional-extension-resource-columns-that-are-selected"></a>
### How to test the grid in browse shows additional extension resource columns that are selected

There is a simple abstraction available in MsPortalFx.Tests.Browse that asserts extension resource specific columns can be selected in browse and that after selection they show up in the browse grid.  
the function is called `canSelectResourceColumns`. You can use it as follows:

```ts
// 1. import test fx
import TestFx = require('MsPortalFx-Test');

...

it("Can select additional columns for the resourcetype and columns have expected title", () => {

// 2. setup an array of expectedDefaultColumns to be shown in browse
    
    const expectedDefaultColumns: Array<testFx.Tests.Browse.ColumnTestOptions> =
        [
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.name },
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.subscription },
            { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.location },
        ];

// 3. setup an array of columns to be selected and call the TestFx.Tests.Browse.canSelectResourceColumns function.
// This function this will perform:
//   - a reset columns action in browse 
//   - select the provided columnsToSelect
//   - assert that brows shows the union of 
// the expectedDefaultColumns match what is displayed expectedDefaultColumns and columnsToSelect

        let columnsToSelect: Array<testFx.Tests.Browse.ColumnTestOptions> =
            [
                { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.locationId },
                { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.resourceGroupId },
                { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.status },
                { columnLabel: extensionResources.hubsExtension.resourceGroups.browse.columnLabels.subscriptionId },
            ];
        return testFx.Tests.Browse.canSelectResourceColumns(
            resourceProvider,
            resourceType,
            resourceBladeTitle,
            expectedDefaultColumns,
            columnsToSelect); 
}
```


<a name="msportalfx-test-scenarios-blades"></a>
<a name="msportalfx-test-scenarios-blades"></a>
### Blades

<a name="msportalfx-test-scenarios-blades-blade-navigation"></a>
<a name="msportalfx-test-scenarios-blades-blade-navigation"></a>
#### Blade navigation

To navigate to blades within msportalfx-test can use one of several approaches

- via a deep link to the blade using the `portal.navigateToUriFragment` function e.g

    ```ts
    import testFx = require('MsPortalFx-Test');
    ...
    
        const resourceName = resourcePrefix + guid.newGuid();
        const createOptions = {
            name: resourceName,
            resourceGroup: resourceGroupName,
            location: locationId,
            resourceProvider: resourceProvider,
            resourceType: resourceType,
            resourceProviderApiVersion: resourceProviderApiVersion,
            properties: {
                displacement: "600cc",
                model: "type1",
                status: 0
            }
        };
        return testSupport.armClient.createResource(createOptions)
            //form deep link to the quickstart blade
            .then((resourceId) => {
                return testFx.portal.navigateToUriFragment(`blade/SamplesExtension/EngineQuickStartBlade/id/${encodeURIComponent(resourceId)}`)
                    .then(() =>
                        testFx.portal.wait(ExpectedConditions.isPresent(testFx.portal.blade({ title: resourceId, bladeType: QuickStartBlade }))));
    ```

- via clicking on another ux component that opens the blade

    ```ts
    import testFx = require('MsPortalFx-Test');
    ...
    
            .then((blade) => blade.asType(SummaryBlade).essentialsPart.settingsHotSpot.click())
            .then(() => settingsBlade.clickSetting(PortalFxResources.properties))
            .then(() => testFx.portal.wait(until.isPresent(testFx.portal.element(testFx.Blades.PropertiesBlade))));
    ```

- via `portal.open*` functions open common portal blades like create, browse and resource summary blades. See [Opening common portal blades](#msportalfx-test-scenarios-blades-common-portal-blades)

- via `portal.search` function to search for, and open browse and resource blades

    ```ts
    import testFx = require('MsPortalFx-Test');
    ...
    
        const subscriptionsBlade = testFx.portal.blade({ title: testSupport.subscription });
        
        testFx.portal.portalContext.features.push({ name: "feature.resourcemenu", value: "true" });
        return testFx.portal.goHome().then(() => {
            return testFx.portal.search(testSupport.subscription);
        }).then((results) => {
            return testFx.portal.wait<SearchResult>(() => {
                var result = results[0];
                return result.title.getText().then((title) => {
                    return title === testSupport.subscription ? result : null;
                });
            });
        }).then((result) => {
            return result.click();
        }).then(() => {
            return testFx.portal.wait(until.isPresent(subscriptionsBlade));
        }); 
    ```

<a name="msportalfx-test-scenarios-blades-locating-an-open-blade"></a>
<a name="msportalfx-test-scenarios-blades-locating-an-open-blade"></a>
#### Locating an open blade

There are several approaches that can be used for locating an already opened blade use `testfx.portal.blade`.

- by blade title
    ```ts
    
        const resourceBlade = testFx.portal.blade({ title: resourceGroupName });
    ```  
 
- by using an existing blade type and its predefined locator
    ```ts
    
        const settingsBlade = testFx.portal.blade({ bladeType: testFx.Blades.SettingsBlade });
        
    ```

<a name="msportalfx-test-scenarios-blades-common-portal-blades"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades"></a>
#### Common portal blades

<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-the-extensions-create-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-the-extensions-create-blade"></a>
##### Opening the extensions Create blade

See [Opening an extensions gallery package create blade](#msportalfx-test-scenarios-create-opening-the-create-blade-from-a-deployed-gallery-package)

<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-the-browse-blade-for-your-resource"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-the-browse-blade-for-your-resource"></a>
##### Opening the Browse blade for your resource

To open/navigate to the Browse blade from resource type you can use `portal.openBrowseBlade`.  The returned promise will resolve with the browse blade. 

```ts
import testFx = require('MsPortalFx-Test');
...

        return testFx.portal.openBrowseBlade(resourceProvider, resourceType, resourceBladeTitle, 20000)
            .then((blade) => blade.filterItems(resourceName))
...
```
<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-a-resource-summary-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-opening-a-resource-summary-blade"></a>
##### Opening a Resource Summary blade

To open/navigate to the Resource Summary blade for a specific resource you can use `portal.openResourceBlade`.  The returned promise will resolve with the Resource summary blade for the given resource. 

```ts
import testFx = require('MsPortalFx-Test');
...

        return testSupport.armClient.createResourceGroup(resourceGroupName, locationId)
            .then((result) => testFx.portal.openResourceBlade(result.resourceGroup.id, result.resourceGroup.name, 70000))
            .then(() => resourceBlade.clickCommand(extensionResources.deleteLabel))
...
```

<a name="msportalfx-test-scenarios-blades-common-portal-blades-spec-picker-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-spec-picker-blade"></a>
##### Spec Picker Blade

The `SpecPickerBlade` can be used to select/change the current spec of a resource.  The following example demonstrates how to navigate to the spec picker for a given resource then changes the selected spec.    

```ts
//1. imports
import testFx = require('MsPortalFx-Test');
import SpecPickerBlade = testFx.Parts.SpecPickerBlade;


        const pricingTierBlade = testFx.portal.blade({ title: extensionResources.samplesExtensionStrings.PricingTierBlade.title });
        let pricingTierPart: PricingTierPart;
        //2. Open navigate blade and select the pricing tier part.  
        // Note if navigating to a resourceblade use testFx.portal.openResourceBlade and blade.element
        return testFx.portal.navigateToUriFragment("blade/SamplesExtension/PricingTierV3Blade", 15000).then(() => {
            return pricingTierBlade.waitUntilBladeAndAllTilesLoaded();
        }).then(() => {
            pricingTierPart = testFx.portal.element(PricingTierPart);
            return pricingTierPart.click();
        }).then(() => {
            //3. get a reference to the picker blade and pick a spec 
            var pickerBlade = testFx.portal.blade({ bladeType: SpecPickerBlade, title: extensionResources.choosePricingTier});
            return pickerBlade.pickSpec(extensionResources.M);
        }).then(() => {
        
```

There are also several API's available to make testing common functionality within browse such as context menu commands and column picking fucntionality for more details see [Browse Scenarios](#msportalfx-test-scenarios-browse).

<a name="msportalfx-test-scenarios-blades-common-portal-blades-settings-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-settings-blade"></a>
##### Settings Blade

Navigation to the `SettingsBlade` is done via the `ResourceSummaryPart` on a resource summary blade. The following demonstrates how to navigate to a settings blade and click on a setting.

```ts
import testFx = require('MsPortalFx-Test');
...
//1. model your resource summary blade which contains a resource summary part

import Blade = testFx.Blades.Blade;
import ResourceSummaryPart = testFx.Parts.ResourceSummaryPart;

class SummaryBlade extends Blade {
    public essentialsPart = this.element(ResourceSummaryPart);
    public rolesAndInstancesPart = this.part({ innerText: resources.rolesAndInstances });
    public estimatedSpendPart = this.part({ innerText: resources.estimatedSpend });
}

...
//2. navigate to the quickstart and click a link

        const settingsBlade = testFx.portal.blade({ bladeType: testFx.Blades.SettingsBlade });
        return testSupport.armClient.createResourceGroup(resourceGroups[0], locationId)
            .then((result) => testFx.portal.openResourceBlade(result.resourceGroup.id, result.resourceGroup.name, 70000))
            //click on the settings hotspot to open the settings blade
             //blades#navigateClick
            .then((blade) => blade.asType(SummaryBlade).essentialsPart.settingsHotSpot.click())
            .then(() => settingsBlade.clickSetting(PortalFxResources.properties))
            .then(() => testFx.portal.wait(until.isPresent(testFx.portal.element(testFx.Blades.PropertiesBlade))));//blades#navigateClick
        
...
```

<a name="msportalfx-test-scenarios-blades-common-portal-blades-properties-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-properties-blade"></a>
##### Properties Blade

Navigation to the `PropertiesBlade` is done via the resource summary blade. The following demonstrates how to navigate to the properties blade

```ts
import testFx = require('MsPortalFx-Test');
...
//2. navigate to the properties blade from the resource blade and check the value of one of the properties

                return testFx.portal.openResourceBlade(resourceId, resourceName, 70000);
            })
            .then(() => testFx.portal.blade({ bladeType: SummaryBlade })
                .essentialsPart.settingsHotSpot.click())
            .then(() => testFx.portal.blade({ bladeType: testFx.Blades.SettingsBlade })
                .clickSetting(PortalFxResources.properties))
            .then(() => {
                const expectedPropertiesCount = 6;
                return testFx.portal.wait(() => {
                    return propertiesBlade.properties.count().then((count) => {
                        return count === expectedPropertiesCount;
                    });
                }, null, testFx.Utils.String.format("Expected to have {0} properties in the Properties blade.", expectedPropertiesCount));
            }).then(() => propertiesBlade.property({ name: PortalFxResources.nameLabel }).value.getText())
            .then((nameProperty) => assert.equal(nameProperty, resourceName, testFx.Utils.String.format("Expected the value for the 'NAME' property to be '{0}' but found '{1}'.", resourceName, nameProperty)));
...
```

<a name="msportalfx-test-scenarios-blades-common-portal-blades-quickstart-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-quickstart-blade"></a>
##### QuickStart Blade

Using a deep link you can navigate directly into a `QuickStartBlade` for a resource with `Portal.navigateToUriFragment`.
   
```ts
import testFx = require('MsPortalFx-Test');
...

        const resourceName = resourcePrefix + guid.newGuid();
        const createOptions = {
            name: resourceName,
            resourceGroup: resourceGroupName,
            location: locationId,
            resourceProvider: resourceProvider,
            resourceType: resourceType,
            resourceProviderApiVersion: resourceProviderApiVersion,
            properties: {
                displacement: "600cc",
                model: "type1",
                status: 0
            }
        };
        return testSupport.armClient.createResource(createOptions)
            //form deep link to the quickstart blade
            .then((resourceId) => {
                return testFx.portal.navigateToUriFragment(`blade/SamplesExtension/EngineQuickStartBlade/id/${encodeURIComponent(resourceId)}`)
                    .then(() =>
                        testFx.portal.wait(ExpectedConditions.isPresent(testFx.portal.blade({ title: resourceId, bladeType: QuickStartBlade }))));
```

While deeplinking is fast it does not validate that the user can actually navigate to a QuickStartBlade via a `ResourceSummaryPart` on a resource summary blade.  The following demonstrates how to verify the user can do so.

```ts
import testFx = require('MsPortalFx-Test');
...
//1. model your resource summary blade which contains a resource summary part

import Blade = testFx.Blades.Blade;
import ResourceSummaryPart = testFx.Parts.ResourceSummaryPart;

class SummaryBlade extends Blade {
    public essentialsPart = this.element(ResourceSummaryPart);
    public rolesAndInstancesPart = this.part({ innerText: resources.rolesAndInstances });
    public estimatedSpendPart = this.part({ innerText: resources.estimatedSpend });
}

...
//2. navigate to the quickstart and click a link

        const summaryBlade = testFx.portal.blade({ title: resourceGroupName, bladeType: SummaryBlade });

        return testFx.portal.openResourceBlade(resourceGroupId, resourceGroupName, 70000)
            //click to open the quickstart blade
            .then(() => summaryBlade.essentialsPart.quickStartHotSpot.click())
            .then(() => summaryBlade.essentialsPart.quickStartHotSpot.isSelected())
            .then((isSelected) => assert.equal(isSelected, true))
            .then(() => testFx.portal.wait(testFx.until.isPresent(testFx.portal.blade({ title: resourceGroupName, bladeType: QuickStartBlade }))))
            .then((result) => assert.equal(result, true));
...
```

<a name="msportalfx-test-scenarios-blades-common-portal-blades-users-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-users-blade"></a>
##### Users Blade

Using a deep link you can navigate directly into the user access blade for a resource with `Portal.navigateToUriFragment`.
   
```ts
import testFx = require('MsPortalFx-Test');
...

        const resourceName = resourcePrefix + guid.newGuid();
        const createOptions = {
            name: resourceName,
            resourceGroup: resourceGroupName,
            location: locationId,
            resourceProvider: resourceProvider,
            resourceType: resourceType,
            resourceProviderApiVersion: resourceProviderApiVersion,
            properties: {
                displacement: "600cc",
                model: "type2",
                status: 0
            }
        };

        return testSupport.armClient.createResource(createOptions)
            //form deep link to the quickstart blade
            .then((resourceId) => testFx.portal.navigateToUriFragment(`blade/Microsoft_Azure_AD/UserAssignmentsBlade/scope/${resourceId}`, timeouts.defaultLongTimeout))
            .then(() => testFx.portal.wait(ExpectedConditions.isPresent(testFx.portal.element(testFx.Blades.UsersBlade))));
```

While deeplinking is fast it does not validate that the user can actually navigate to a UsersBlade via a `ResourceSummaryPart` on a resource summary blade.  The following demonstrates how to verify the user can do so.

```ts
import testFx = require('MsPortalFx-Test');
...
//1. model your resource summary blade which contains a resource summary part

import Blade = testFx.Blades.Blade;
import ResourceSummaryPart = testFx.Parts.ResourceSummaryPart;

class SummaryBlade extends Blade {
    public essentialsPart = this.element(ResourceSummaryPart);
    public rolesAndInstancesPart = this.part({ innerText: resources.rolesAndInstances });
    public estimatedSpendPart = this.part({ innerText: resources.estimatedSpend });
}

...
//2. navigate to the quickstart and click a link

        const summaryBlade = testFx.portal.blade({ title: resourceGroupName, bladeType: SummaryBlade });

        return testFx.portal.openResourceBlade(resourceGroupId, resourceGroupName, 70000)
            //click to open the user access blade
            .then(() => summaryBlade.essentialsPart.accessHotSpot.click())
            .then(() => summaryBlade.essentialsPart.accessHotSpot.isSelected())
            .then((isSelected) => assert.equal(isSelected, true))
            .then(() => testFx.portal.wait(ExpectedConditions.isPresent(testFx.portal.element(UsersBlade)), 90000))
            .then((result) => assert.equal(result, true));
...
```

<a name="msportalfx-test-scenarios-blades-common-portal-blades-move-resource-blade"></a>
<a name="msportalfx-test-scenarios-blades-common-portal-blades-move-resource-blade"></a>
##### Move Resource Blade

The `MoveResourcesBlade` represents the portals blade used to move resources from a resource group to a new resource group `portal.startMoveResource` provides a simple abstraction that will iniate the move of an existing resource to a new resource group.  The following example demonstrates how to initiate the move and then wait on successful notification of completion.
  
```ts
import testFx = require('MsPortalFx-Test');
...
    
            return testFx.portal.startMoveResource(
                {
                    resourceId: resourceId,
                    targetResourceGroup: newResourceGroup,
                    createNewGroup: true,
                    subscriptionName: subscriptionName,
                    timeout: 120000
                });
            }).then(() => {
            return testFx.portal.element(NotificationsMenu).waitForNewNotification(portalFxResources.movingResourcesComplete, null, 5 * 60 * 1000);
```

<a name="msportalfx-test-scenarios-blades-blade-dialogs"></a>
<a name="msportalfx-test-scenarios-blades-blade-dialogs"></a>
#### Blade Dialogs

On some blades you may use commands that cause a blade dialog that generally required the user to perform some acknowledgement action.  
The `Blade` class exposes a `dialog` function that can be used to locate the dialog on the given blade and perform an action against it. 
The following example demonstrates how to:

- get a reference to a dialog by title
- find a field within the dialog and sendKeys to it 
- clicking on a button in a dialog

```ts
import testFx = require('MsPortalFx-Test');
...

        const samplesBlade = testFx.portal.blade({ title: "Samples", bladeType: SamplesBlade });
        return testFx.portal.goHome(20000).then(() => {
            return testFx.portal.navigateToUriFragment("blade/SamplesExtension/SamplesBlade");
        }).then(() => {
            return samplesBlade.openSample(extensionResources.samplesExtensionStrings.SamplesBlade.bladeWithToolbar);
        }).then((blade) => {
            return blade.clickCommand(extensionResources.samplesExtensionStrings.BladeWithToolbar.commands.save);
            }).then((blade) => {
            //get a reference to a dialog by title
            let dialog = blade.dialog({ title: extensionResources.samplesExtensionStrings.BladeWithToolbar.bladeDialogs.saveFile });
            //sending keys to a field in a dialog
            return dialog.field(testFx.Controls.TextField, { label: extensionResources.samplesExtensionStrings.BladeWithToolbar.bladeDialogs.filename })
                .sendKeys("Something goes here")
                //clicking a button within a dialog
                .then(() => dialog.clickButton(extensionResources.ok));
            });
```

<a name="msportalfx-test-scenarios-parts"></a>
<a name="msportalfx-test-scenarios-parts"></a>
### Parts

<a name="msportalfx-test-scenarios-parts-how-to-get-the-reference-to-a-part-on-a-blade"></a>
<a name="msportalfx-test-scenarios-parts-how-to-get-the-reference-to-a-part-on-a-blade"></a>
#### How to get the reference to a part on a blade

1. If it is a specific part, like the essentials for example:
```
	let thePart = blade.element(testFx.Parts.ResourceSummaryPart);
```
1. For a more generic part:
```
	let thePart = blade.part({innerText: "some part text"});
``` 
1. To get a handle of this part using something else than simple text you can also do this:
```
	let thePart = blade.element(By.Classname("myPartClass")).AsType(testFx.Parts.Part);
```

<a name="msportalfx-test-scenarios-parts-collectionpart"></a>
<a name="msportalfx-test-scenarios-parts-collectionpart"></a>
#### CollectionPart

The following example demonstrates how to:

- get a reference to the collection part using `blade.element(...)`. 
- get the rollup count using `collectionPart.getRollupCount()`
- get the rollup count lable using `collectionPart.getRollupLabel()`
- get the grid rows using `collectionPart.grid.rows`

```ts

    it("Can get rollup count, rollup label and grid", () => {
        const collectionBlade = testFx.portal.blade({ title: "Collection" });

        return testFx.portal.navigateToUriFragment("blade/SamplesExtension/CollectionPartIntrinsicInstructions")
            .then(() => testFx.portal.wait(() => collectionBlade.waitUntilBladeAndAllTilesLoaded()))
            .then(() => collectionBlade.element(testFx.Parts.CollectionPart))
            .then((collectionPart) => {
                return collectionPart.getRollupCount()
                    .then((rollupCount) => assert.equal(4, rollupCount, "expected rollupcount to be 4"))
                    .then(() => collectionPart.getRollupLabel())
                    .then((label) => assert.equal(extensionResources.samplesExtensionStrings.Robots, label, "expected rollupLabel is Robots"))
                    .then(() => collectionPart.grid.rows.count())
                    .then((count) => assert.ok(count > 0, "expect the grid to have rows"));
            });
    });
```

Note if you have multiple collection parts you may want to use `blade.part(...)` to search by text.

<a name="msportalfx-test-scenarios-parts-grid"></a>
<a name="msportalfx-test-scenarios-parts-grid"></a>
#### Grid

<a name="msportalfx-test-scenarios-parts-grid-finding-a-row-within-a-grid"></a>
<a name="msportalfx-test-scenarios-parts-grid-finding-a-row-within-a-grid"></a>
##### Finding a row within a grid

The following demonstrates how to use `Grid.findRow` to:

- find a `GridRow` with the given text at the specified index
- get the text from all cells within the row using `GridRow.cells.getText`

```ts
                
                return grid.findRow({ text: "John", cellIndex: 0 })
                    .then((row) => row.cells.getText())
                    .then((texts) => texts.length > 2 && texts[0] === "John" && texts[1] === "333");
                
```

<a name="msportalfx-test-scenarios-parts-createcomboboxfield"></a>
<a name="msportalfx-test-scenarios-parts-createcomboboxfield"></a>
#### CreateComboBoxField

use this for modeling the resouce group `CreateComboBoxField` on create blades.

- use `selectOption(...)` to chose an existing resource group
- use `setCreateValue(...)` and `getCreateValue(...)` to get and check the value of the create new field respectively 

```ts

        return testFx.portal.goHome(40000).then(() => {
            //1. get a reference to the create blade
            return testFx.portal.openGalleryCreateBlade(
                galleryPackageName,              //the gallery package name e.g "Microsoft.CloudService"
                bladeTitle, //the title of the blade e.g "Cloud Services"
                timeouts.defaultLongTimeout             //an optional timeout to wait on the blade
            )
        }).then((blade: testFx.Blades.CreateBlade) => {
            //2. find the CreateComboBoxField
            var resourceGroup = blade.element(CreateComboBoxField);
            //3. set the value of the Create New text field for the resource group
            return resourceGroup.setCreateValue("NewResourceGroup")
                .then(() =>
                    resourceGroup.getCreateValue()
                ).then((value) =>
                    assert.equal("NewResourceGroup", value, "Set resource group name")
                ).then(() =>
                    resourceGroup.selectOption("NewResourceGroup_4")
                ).then(() =>
                    resourceGroup.getDropdownValue()
                ).then((value) =>
                    assert.equal("NewResourceGroup_4", value, "Set resource group dropdown")
                );
        });
        
```

<a name="msportalfx-test-scenarios-parts-editor"></a>
<a name="msportalfx-test-scenarios-parts-editor"></a>
#### Editor

<a name="msportalfx-test-scenarios-parts-editor-can-read-and-write-content"></a>
<a name="msportalfx-test-scenarios-parts-editor-can-read-and-write-content"></a>
##### Can read and write content

The following example demonstrates how to:

- use `read(...)` to read the content
- use `empty(...)` to empty the content
- use `sendKeys(...)` to write the content

```ts

        let editorBlade: EditorBlade;
        let editor: Editor;
        
        return BladeOpener.openSamplesExtensionBlade(
            editorBladeTitle,
            editorUriFragment,
            EditorBlade,
            10000)
            .then((blade: EditorBlade) => {
                editorBlade = blade;
                editor = blade.editor;
                return editor.read();
            })
            .then((content) => assert.equal(content, expectedContent, "expectedContent is not matching"))
            .then(() => editor.empty())
            .then(() => editor.sendKeys("document."))
            .then(() => testFx.portal.wait(() => editor.isIntellisenseUp().then((isUp: boolean) => isUp)))
            .then(() => {
                let saveButton = By.css(`.azc-button[data-bind="pcControl: saveButton"]`);
                return editorBlade.element(saveButton).click();
            })
            .then(() => testFx.portal.wait(() => editor.read().then((content) => content === "document.")))
            .then(() => editor.workerIFramesCount())
            .then((count) => assert.equal(count, 0, "We did not find the expected number of iframes in the portal.  It is likely that the editor is failing to start web workers and is falling back to creating new iframes"));
        
```

<a name="msportalfx-test-scenarios-parts-essentials"></a>
<a name="msportalfx-test-scenarios-parts-essentials"></a>
#### essentials

<a name="msportalfx-test-scenarios-parts-essentials-essentials-tests"></a>
<a name="msportalfx-test-scenarios-parts-essentials-essentials-tests"></a>
##### Essentials tests

The following example demonstrates how to:

- use `countItems(...)` to count the number of all items. (MultiLine Item is counted as one)
- use `isDisabled(...)` to get a value that determines whether the essentials is disabled
- use `hasViewAll(...)` to get a value that determines whether the essentials has ViewAll button or not
- use `getViewAllButton(...)` to get a PortalElement of the essentials' viewAll button
- use `getItemByLabelText(...)` to get an EssentialsItem that is found by its label text
- use `getPropertyElementByValue(...)` to get a PortalELement of matching property value
- use `getExpandedState(...)` to get a value that determines the essentials' expanded state
- use `setExpandedState(...)` to set the essentials' expanded state
- use `getExpander(...)` to get the Expander element
- use `getProperties(...)` to get an array of properties
- use `getLabelText(...)` to get the item label text
- use `hasMoveResource(...)` to get whether the item has move resource blade or not
- use `getLabelId(...)` to get the item label id
- use `getSide(...)` to get the item's side

```ts

        let essentialsBlade: EssentialsBlade;
        let essentials: Essentials;

        return BladeOpener.openSamplesExtensionBlade(
            essentialsBladeTitle,
            essentialsUriFragment,
            EssentialsBlade,
            waitTime)
            .then((blade) => blade.waitUntilLoaded(waitTime).then(() => blade))
            .then((blade: EssentialsBlade) => {
                essentialsBlade = blade;
                essentials = blade.essentials;
                return Q.all<any>([
                    essentials.countItems(),
                    essentials.getExpandedState(),
                    essentials.hasViewAll()
                ]);
            })
            .then((values: any[]) => {
                // Essentials item count, expanded state and viewAll button state check
                assert.equal(values[0], 10, "Essentials has 10 items at the beginning");
                assert.ok(values[1], "Essentials should be expanded by default");
                assert.ok(values[2], "Essentials has the ViewAll button at the beginning");
                essentials.getViewAllButton().click();
            })
            .then(() => essentials.countItems())
            .then((count: number) => {
                // Essentials item count after click ViewAll button
                assert.equal(count, 12, "Essentials has 12 items after adding dynamic properties");
                // Item label and properties check
                return Q.all([
                    itemAssertionHelper(essentials, {
                        label: "Resource group",
                        hasMoveResource: true,
                        side: "left",
                        properties: [
                            { type: EssentialsItemPropertyType.Function, value: "snowtraxpsx600" }
                        ]
                    }),
                    itemAssertionHelper(essentials, {
                        label: "Multi-line Item",
                        hasMoveResource: false,
                        side: "right",
                        properties: [
                            { type: EssentialsItemPropertyType.Text, value: "sample string value" },
                            { type: EssentialsItemPropertyType.Link, value: "Bing.com" }
                        ]
                    }),
                    itemAssertionHelper(essentials, {
                        label: "Status",
                        hasMoveResource: false,
                        side: "left",
                        properties: [
                            { type: EssentialsItemPropertyType.Text, value: "--" }
                        ]
                    })
                ]);
            })
            .then(() => essentials.getPropertyElementByValue("status will be changed")
                .then((propElement: testFx.PortalElement) =>  propElement.click()))
                // Item label and properties check after clicking "status will be changed"
                .then(() => itemAssertionHelper(essentials, {
                    label: "Status",
                    hasMoveResource: false,
                    side: "left",
                    properties: [
                        { type: EssentialsItemPropertyType.Text, value: "1 times clicked!" }
                    ]
                }))
            .then(() => essentials.getPropertyElementByValue("snowtraxpsx600")
                .then((propElement: testFx.PortalElement) =>  propElement.click()))
                // Item label and properties check after clicking "snowtraxpsx600"
                .then(() => itemAssertionHelper(essentials, {
                    label: "Status",
                    hasMoveResource: false,
                    side: "left",
                    properties: [
                        { type: EssentialsItemPropertyType.Text, value: "Resource group window is opened" }
                    ]
                }))
            .then(() => essentials.getExpander().click())
            .then(() => Q.all<any>([
                essentials.getExpandedState(),
                essentials.items.isDisplayed()
            ]))
            .then((values: any) => {
                // check expander state and items' visibilities after closing expander
                assert.ok(!values[0], "expander should be closed");
                values[1].forEach((state: boolean) => {
                    assert.ok(!state, "items should be invisible");
                });
                return essentials.getExpander().click()
                    .then(() => Q.all<any>([
                    essentials.getExpandedState(),
                    essentials.items.isDisplayed()
                ]));
            })
            .then((values: any) => {
                // check expander state and items' visibilities after opening expander
                assert.ok(values[0], "expander should be opened");
                values[1].forEach((state: boolean) => {
                    assert.ok(state, "items should be visible");
                });
            });
        
```

<a name="msportalfx-test-scenarios-command"></a>
<a name="msportalfx-test-scenarios-command"></a>
### Command

<a name="msportalfx-test-scenarios-action-bar"></a>
<a name="msportalfx-test-scenarios-action-bar"></a>
### Action Bar

<a name="msportalfx-test-scenarios-delete"></a>
<a name="msportalfx-test-scenarios-delete"></a>
### Delete

<a name="msportalfx-test-scenarios-styling-layout-regression-detection"></a>
<a name="msportalfx-test-scenarios-styling-layout-regression-detection"></a>
### Styling / layout regression detection

To detect styling or layout regressions in your tests, use the `portal.detectStylingRegression` function.

    ```ts
    import testFx = require('MsPortalFx-Test');
    ...
    it("Can do action X", () => {    
        // Your test goes here, dummy test follows...
        return testFx.portal.goHome().then(() => {
            return testFx.portal.detectStylingRegression("MyExtension/Home");
        });
    });
    ```
    
The function will upload a screenshot to the "cicss" container of the storage account with the name, key, subscription id and resource group you will provide; 

Put the following values into your config.json:

```
    "CSS_REGRESSION_STORAGE_ACCOUNT_NAME":"myaccountname",
    "CSS_REGRESSION_STORAGE_ACCOUNT_SUBSCRIPTIONID":"mysubscriptionid",
    "CSS_REGRESSION_STORAGE_ACCOUNT_RESOURCE_GROUP":"mygresourcegroupname",
    "CSS_REGRESSION_STORAGE_ACCOUNT_KEY":"myaccountkey",
```

Put the storage account key into Windows Credential Manager using cmdkey i.e

```
    cmdkey /generic:CSS_REGRESSION_STORAGE_ACCOUNT_KEY /user:myaccountname /pass:myaccountkey
```

The screenshot will then be compared with a Last Known Good screenshot and, if different, a diff html file will be produced and uploaded to your storage account.
The link to that html file will be in the failed test's error message and includes a powershell script download to promote the Latest screenshot to Last Known Good.
The initial Last Known Good file is the screenshot taken when there was no Last Known Good screenshot to compare it to; i.e. to seed your test, just run it once.
    
For reference, here's the signature of the `portal.detectStylingRegression` function.
    
    ```ts
    ...
    
    /**
     * Takes a browser screenshot and verifies that it does not differ from LKG screenshot;
     * contains an assert that will fail on screenshot mismatch
     * @param uniqueID Test-specific unique screenshot identifier, e.g. "MyExtension/ResourceGroupTagsTest"
     * @returns Promise resolved once styling regression detection is done (so you can chain on it)
     */
    public detectStylingRegression(uniqueID: string): Q.Promise<void> {
        
    ```

<a name="msportalfx-test-scenarios-styling-layout-regression-detection-"></a>
<a name="msportalfx-test-scenarios-styling-layout-regression-detection-"></a>
#### ...

<a name="msportalfx-test-scenarios-locators"></a>
<a name="msportalfx-test-scenarios-locators"></a>
### Locators

<a name="msportalfx-test-scenarios-consuming-updates"></a>
<a name="msportalfx-test-scenarios-consuming-updates"></a>
### Consuming Updates

<a name="msportalfx-test-scenarios-mocking"></a>
<a name="msportalfx-test-scenarios-mocking"></a>
### Mocking

<a name="msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal"></a>
<a name="msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal"></a>
#### How to show mock data into the Portal

The [MsPortalFx-Mock](https://www.npmjs.com/package/msportalfx-mock) package provides a framework for showing mock data in the portal. It come with builtin support for mocking ARM data.

<a name="msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal-mocking-arm"></a>
<a name="msportalfx-test-scenarios-mocking-how-to-show-mock-data-into-the-portal-mocking-arm"></a>
##### Mocking ARM

Mock data including providers, subscriptions, resource groups and resources can be defined in JSON object and used to initialize the ArmManager.

```ts

import { ArmProxy } from "MsPortalFx-Mock/lib/src/ArmProxy/ArmProxy";
import { ArmManager } from "MsPortalFx-Mock/lib/src/ArmProxy/ArmManager";

...

const mockData: ArmManager.MockData = {
    providers: [
        {
            namespace: "Providers.Test",
            resourceTypes: [
                {
                    resourceType: "type1",
                    locations: ["WestUS"],
                    apiVersions: ["2014-04-01"]
                },
                {
                    resourceType: "type2",
                    locations: ["East US", "East US 2", "North Central US"],
                    apiVersions: ["2014-04-01"]
                }
            ]
        }
    ],
    subscriptions: [
        {
            subscriptionId: "sub1",
            displayName: "Test sub 1",
            state: "Active",
            subscriptionPolicies: {
                locationPlacementId: "Public_2014-09-01",
                quotaId: "FreeTrial_2014-09-01"
            },
            resourceGroups: [
                {
                    name: "sub1rg1",
                    location: "WestUS",
                    tags: {
                        "env": "prod",
                        "ver": "4.6"
                    },
                    properties: {
                        lockState: "Unlocked",
                        provisioningState: "Succeeded",
                    },
                    resources: [
                        {
                            name: "sub1rg1r1",
                            location: "WestUS",
                            tags: {
                                "env": "prod"
                            },
                            type: "providers.test/type1",
                            kind: null,
                            nestedResources: [
                                {
                                    name: "nr1",
                                    tags: {},
                                    type: "nested"
                                }
                            ]
                        }
                    ]
                },
                {
                    name: "sub1rg2",
                    location: "WestUS",
                    tags: {
                        "env": "prod",
                        "ver": "4.6"
                    },
                    properties: {
                        lockState: "Unlocked",
                        provisioningState: "Succeeded",
                    },
                    resources: [
                        {
                            name: "sub1rg2r1",
                            location: "WestUS",
                            tags: {
                                "env": "prod"
                            },
                            type: "providers.test/type2",
                            kind: null
                        }
                    ]
                }
            ]
        }
    ]
};

...

    let armManager = new ArmManager.Manager();
    armManager.initializeMockData(mockData);
    
```

The ArmProxy needs to initialized at the beginning of your tests with the ArmManager. The ArmProxy supports two modes for showing data 1) mock ONLY and 2) mock + actual.

You will need to initialize the portalContext->patches to the local server address setup by the proxy.

```ts

        return ArmProxy.create(nconf.get("armEndpoint"), 5000, armManager, null, true).then(proxy => {
            armProxy = proxy;
            testFx.portal.portalContext.patches = [proxy.patchAddress];
        });
        
```

The proxy can be disposed at the end of your tests.
```ts

        return testFx.portal.quit().then(() => ArmProxy.dispose(armProxy));
        
```


<a name="msportalfx-test-scenarios-code-coverage"></a>
<a name="msportalfx-test-scenarios-code-coverage"></a>
### Code Coverage
<a name="msportalfx-test-scenarios-code-coverage-interop-how-to-run-net-code-from-your-tests"></a>
<a name="msportalfx-test-scenarios-code-coverage-interop-how-to-run-net-code-from-your-tests"></a>
#### Interop, how to run .NET code from your tests
edge.js


<a name="msportalfx-test-scenarios-contributing"></a>
<a name="msportalfx-test-scenarios-contributing"></a>
### Contributing

<a name="msportalfx-test-scenarios-contributing-to-enlist"></a>
<a name="msportalfx-test-scenarios-contributing-to-enlist"></a>
#### To enlist

git clone https://github.com/azure/msportalfx-test.git

<a name="msportalfx-test-scenarios-contributing-to-build-the-source"></a>
<a name="msportalfx-test-scenarios-contributing-to-build-the-source"></a>
#### To build the source

Use Visual Studio or Visual Studio Code to build

1. Run ./scripts/Setup.cmd

<a name="msportalfx-test-scenarios-contributing-to-setup-the-tests"></a>
<a name="msportalfx-test-scenarios-contributing-to-setup-the-tests"></a>
#### To setup the tests

1. To run the tests you need:

- Create a dedicated test subscription that is used for tests only
- A user that has access to the test subscription only
- An AAD App and service principal with access
- Have run `setup.cmd` in the portal repo or have run `powershell.exe -ExecutionPolicy Unrestricted -file "%~dp0\Setup-OneCloud.ps1" -DeveloperType Shell %*`

Once you have the first two use the following to create the AAD application and service principal.

```powershell 
    msportalfx-test\scripts\Create-AdAppAndServicePrincipal.ps1 
        -TenantId "someguid" 
        -SubscriptionId "someguid" 
        -ADAppName "some ap name" 
        -ADAppHomePage "https://somehomepage" 
        -ADAppIdentifierUris "https://someidentiferuris" 
        -ADAppPassword $someAdAppPassword 
        -SPRoleDefinitionName "Reader" 
```

**Note**: Don't forget to store the password you use below in key vault, secret store or other. You will not be able to retrieve it using the commandlets. 

You will use the details of the created service principal in the next steps.  

For more detail on [AAD Applications and Service Principals] see (https://azure.microsoft.com/en-us/documentation/articles/resource-group-authenticate-service-principal/#authenticate-with-password---powershell).  

1. Open **test\config.json** and enter appropriate values for:
    ```
            "aadAuthorityUrl": "https://login.windows.net/TENANT_ID_HERE",
            "aadClientId": "AAD_CLIENT_ID_HERE",
            "subscriptionId": "SUBSCRIPION_ID_HERE",
            "subscriptionName": "SUBSCRIPTION_NAME_HERE",
    ```

1. The account that corresponds to the specified credentials should have at least contributor access to the subscription specified in the **config.json** file. The account must be a Live Id account. It cannot be an account that requires two factor authentication (like most @microsoft.com accounts). 

1. Install the Portal SDK from [Aux Docs](https://auxdocs.azurewebsites.net/en-us/downloads), then open Visual Studio and create a new Portal Extension from File --> New Project --> Azure Portal --> Azure Portal Extension. Name this project **LocalExtension** so that the extension itself is named LocalExtension, which is what many of the tests expect. Then hit CTRL+F5 to host the extension in IIS Express.

1. The *Can Find Grid Row* and the *Can Choose A Spec* tests require special configuration described in the tests themselves.

1. Many of the tests currently rely on the CloudService extension. We are working to remove this dependency.

<a name="msportalfx-test-scenarios-to-run-the-tests"></a>
<a name="msportalfx-test-scenarios-to-run-the-tests"></a>
### To run the tests

Open a command prompt in this directory and run:

```
	npm install --no-optional
	npm test
```

<a name="msportalfx-test-scenarios-to-run-the-tests-authoring-documents"></a>
<a name="msportalfx-test-scenarios-to-run-the-tests-authoring-documents"></a>
#### Authoring documents
- When adding a document create a new *.md file in /docs e.g /docs/foo.md
- Author the document using [markdown syntax](https://daringfireball.net/projects/markdown/syntax)
- Inject content from your documents into the master template in /docs/TEMPLATE.md using [gitdown syntax](https://github.com/gajus/gitdown) E.g


    ```
    {"gitdown": "include", "file": "./foo.md"}
    ```


- To ensure all code samples remain up to date we extended gitdown syntax to support code injection. To reference source code in your document directly from a *.ts file use the include-section extension E.g


    ```
    {"gitdown": "include-section", "file": "../test/BrowseResourceBladeTests.ts", "section": "tutorial-browse-context-menu#step2"}
    ```


    this will find all content in ../test/BrowseResourceBladeTests.ts that is wrapped in comments //tutorial-browse-context-menu#step2 and will inject them directly into the document. see /docs/tutorial-browse-context-menu.md for a working example

<a name="msportalfx-test-scenarios-to-run-the-tests-generating-the-docs"></a>
<a name="msportalfx-test-scenarios-to-run-the-tests-generating-the-docs"></a>
#### Generating the docs
You can generate the documentation in one of two ways

- As part of pack the `docs` script from package.json is run to ensure that all docs are up to date

    ```
    npm pack
    ```

-  Or, While you are writing docs you may want to check that your composition or jsdoc for API ref is generating as expected to do this you can execute run the following 

    ```
    npm run docs
    ```

the output of the composed TEMPLATE.md will be written to ./README.md and the generated API reference from your jsdocs will be written to /docs/apiref.md

<a name="msportalfx-test-scenarios-to-run-the-tests-to-submit-your-contribution"></a>
<a name="msportalfx-test-scenarios-to-run-the-tests-to-submit-your-contribution"></a>
#### To submit your contribution
Submit a pull request to the repo [http://aka.ms/msportalfx-test](http://aka.ms/msportalfx-test)

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

<a name="msportalfx-test-scenarios-to-run-the-tests-questions"></a>
<a name="msportalfx-test-scenarios-to-run-the-tests-questions"></a>
#### Questions?

Send an email to ibizadiscuss@microsoft.com

<a name="msportalfx-test-scenarios-api-reference"></a>
<a name="msportalfx-test-scenarios-api-reference"></a>
### API Reference

[View thet API Reference](http://aka.ms/msportalfx-test/api)

Generated on 2017-05-11
