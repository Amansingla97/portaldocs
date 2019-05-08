﻿# RangeSlider

 
<a name="basics"></a>
### Basics
A RangeSlider is an element used to set a range of values. It provides a visual indication of the current selected rnage amidst the total range of content. 


<!-- TODO get an IMAGE to embed here -->

<!-- TODO get an SAMPLE CODE to embed here -->

 
<a name="when-to-use"></a>
### When to use
A RangeSlider is a good choice when you know that users think of the selected range as a relative quantity, not numeric values in fields. For example, users may want to set the low and high acceptable scale settings within an overall minimum and maximum allowable values.


 
<a name="best-practices"></a>
### Best practices

<a name="best-practices-do"></a>
#### Do

* Use a RangeSlider when you want your users to be able to set defined values (such as volume or brightness).
* Use step points (or tick marks) if you don’t want the Slider to allow arbitrary values between min and max.
* Use a Slider when the user would benefit from instant feedback on the effect of setting changes.
* Set the **infoBalloonContent** to concise, helpful text with a link to learn
* Use the **label** option to provide a helpful name for the control.

<a name="best-practices-don-t"></a>
#### Don&#39;t

* Don't use a RangeSlider when the options are not values.
* Don’t use a RangeSlider for binary settings.
* Don’t create a continuous RangeSlider if the range of values is large.
* Don’t use for a range of three values or less.


 
<a name="developer-tips-and-tricks"></a>
### Developer tips and tricks



<a name="developer-tips-and-tricks-interactive-control-and-sample-source-code"></a>
#### Interactive control and sample source code
To see the latest control, change options and get source code for your project use the interactive playground site.  [Learn more](./top-extensions-controls-playground.md).

The RangeSlider control offers different create methods and has the following entries in the interactive controls playground

*  <a href="https://ms.portal.azure.com/?Microsoft_Azure_Playground=true#blade/Microsoft_Azure_Playground/ControlsIndexBlade/RangeSlider_createSimpleRangeSlider_Playground" target="_blank">RangeSlider (SimpleRangeSlider)</a>

*  <a href="https://ms.portal.azure.com/?Microsoft_Azure_Playground=true#blade/Microsoft_Azure_Playground/ControlsIndexBlade/RangeSlider_createCustomRangeSlider_Playground" target="_blank">RangeSlider (CustomRangeSlider)</a>

 


 
<a name="related-info"></a>
### Related info

* <a href="https://www.figma.com/file/Bwn8rmUOYtnPRwA3JoQTBn/Azure-Portal-Toolkit?node-id=3492%3A393895" target="_blank">RangeSlider in Azure Portal Toolkit (Figma)</a> 

* [Azure design guidance](http://aka.ms/portalfx/design)


