---
ID: 93
post_title: Announcing TypeScript 1.0.1
author: Ryan Cavanaugh
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-0-1/
published: true
post_date: 2014-05-13 11:12:00
---
Today, we’re announcing the release of TypeScript 1.0.1. This release addresses user-reported issues around stability, performance, and Visual Studio 2012 compatibility. The upgrade is available as part of the [Visual Studio 2013 Update 2][1], a [Visual Studio 2012 power tool][2], [npm package][3], and as [source][4].  
  
We’re also announcing the support plan for TypeScript going forward. We’re hard at work on new features, including many leveraging capabilities in upcoming versions of Visual Studio.  <span>We will be moving our development efforts from Visual Studio 2012 to more recent releases, starting with Visual Studio 2013 Update 2.</span> The TypeScript 1.0.1 release will be the final release supported on Visual Studio 2012. This will allow us to make the next TypeScript a more full-featured development experience in future releases.

Changelog:

*   Fixes for [compiler performance regression][5], [compiler crash for self-referencing modules][6], and [incorrect type for MutationObserver][7]
*   Additionally, fixes for compiler correctness around super calls and lambdas, codegen where parentheses were too aggressively removed, and general compiler reliability
*   Visual Studio: Fixes to HighDPI, debugger reliability, and nav bar reliability

 [1]: http://www.microsoft.com/en-us/download/details.aspx?id=42666
 [2]: http://visualstudiogallery.msdn.microsoft.com/ac357f1e-9847-46ac-a4cf-520325beaec1
 [3]: https://www.npmjs.org/package/typescript
 [4]: https://typescript.codeplex.com/releases/view/122175
 [5]: https://typescript.codeplex.com/workitem/2243
 [6]: https://typescript.codeplex.com/workitem/2336
 [7]: https://typescript.codeplex.com/workitem/1598