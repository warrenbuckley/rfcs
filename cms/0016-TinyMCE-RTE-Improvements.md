# RFC Name

Request for Contribution (RFC) 0016 : TinyMCE RTE Improvements

## Code of conduct

Please read and respect the [RFC Code of Conduct](https://github.com/umbraco/rfcs/blob/master/CODE_OF_CONDUCT.md)

## Intended Audience

The intended audience for this RFC is:
* Developers, implementors & editors

## Summary

This RFC is to give specific details on how we plan to improve the existing implementation of the TinyMCE version 4 rich text editor.


## Motivation
This has been a follow up RFC from #5 & #15 where it has been discusssed that the rich text editor could be improved and enhanced to help give content editors a better experience when authoring or ammending content in the CMS.

The motivation from #15 isn't really much different from this one: We would like to provide a Rich Text Editor experience that would enable content creators to work faster and simpler than our current implementation. We want the rich text editor to be fully configurable at the data type level instead of relying on a global configuration file like it currently does. The rich text editor should handle drag and drop and copy and paste of images nicely along with copy/pasting of Word documents. Common elements should be supported out of the box such as headers, quotes without having to add custom styles to a stylesheet. We also want to improve the current OEmbed functionality to include things like social media posts.


## Detailed Design

The Rich Text Editor improvements will be based on the current rich text editor library that the Umbraco CMS uses which will be the latest version of TinyMCE 4.x.

The following improvements to the TinyMCE editor are planned and in another round we can do further iterative improvements.

**Implement pasting along with drag n drop of images**<br/>
We have verified with a CodePen sample to ensure we could get TinyMCE to support this. We will allow a prevalue configuration of the editor in Umbraco to set a folder location of where to store pasted and dragged images in the media library. A future iteration could improve this further and prompt the user on where they would like to store the image/s in the media library.

**Reduce inital load time and file size of TinyMCE into the browser**<br/>
We will prototype various ways to improve the perceived initial load time of TinyMCE. We will be looking at minification and compression techniques along with loading some of it's assets in behind the scenes before the editor is rendered.

**Improve the loading experience** <br/>
Use Tiny's `init_inistance_callback` property to remove Umbraco's own loader as opposed to the current implementation of checking it the TinyMCE library is loaded.

**Improve the experience with pasting from Word documents**<br/>
Use Tiny's event `before_paste` to notify us when content is being pasted into the editor. This event already notifies us if the content was copied from Word, so we can use the pasted content to some JavaScript open source library or C# library to help clean up the HTML markup & formatting and thus remove any flicker associated with the editor loading.

We would like to allow developers to enhance this themselves by replacing this functionality if they choose too with already existing plugins such as the paid plugin by [TinyMCE PowerPaste](https://apps.tiny.cloud/products/powerpaste/)

**Improve moving un-editable elements**<br/>
For non-editable elements inserted into the rich text editor (such as macros, or OEmbed content) we want to allow for moving these elements in a nicer way by allowing copy/pasting or moving of these elements.


## Drawbacks

There should be no drawbacks to this, as this project can be iterative and add improvements and enhancements to the existing editor that we all currently use.


## Alternatives

We did consider Trix from the previous user testing results from content editors that we recieved, however after comparing speed, extensibility, documentation of the editors. We found that it would be more beneficial to improve and enhance what we already have as opposed to re-implementing everything again in Trix and having to mainatin two editors side by side, as removing TinyMCE would be considered a breaking change. Below are notes on some of the findings & comparissons.

**Cleaner HTML output** <br/>
Trix was propsoed it had a cleaner HTML output, however from testing the editor side by side with our TinyMCE implementation when using the `Enter` key, Trix would generate markup as a `<div>` as opposed to TinyMCE's more semantically correct `<p>` paragraph.

**Loading and Rendering**<br/>
Trix is a more leightweight library and is able to load very quickly, however when considering the functionality and additions that the TinyMCE editor can currently do. It is an unfair comparisson.

We are aware that TinyMCE takes longer to do its initial load into the backoffice for the first request due to the size of the library, but subsequent requests regardless if TinyMCE uses an IFrame or not it is fractionaly slower than rendering the editor and its contents than Trix. As mentioned above we aim to reduce the initial file size to help with the first render of a TinyMCE editor.

One thing from our testing we noticed that the current implementation of TinyMCE when testing with a slow/throttled network connection will load several times. This is due to an implemntation flaw where we currently check that the TinyMCE library is loaded. Due to the way TinyMCE has been developed checking the presence of the main library is loaded is not the best way to know if an editor is ready. We plan to fix this as part of our improvements as TinyMCE gives us a callback function to notify us when it has fully loaded and initilased the editor to help the jarring experience of several loading phases.


## Out of Scope

**Upgrading to TinyMCE 5** <br/>
After some initital research & findings, that it would cause breaking changes for any Umbraco sites who have written their own TinyMCE plugins/buttons and that upgrading to TinyMCE version 5 would give us little benefit, apart from the underlying APIs that is uses has been made neater.


## Unresolved Issues

* Finding an OpenSource library to help cleanup pasted Word HTML, either JavaScript or C# that does the best job


## Related RFCs

* https://github.com/umbraco/rfcs/pull/15#issuecomment-514550647
* https://github.com/umbraco/rfcs/pull/5


## Contributors

This RFC was compiled by:

* Warren Buckley (Umbraco HQ)