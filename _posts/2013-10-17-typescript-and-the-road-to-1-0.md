---
ID: 133
post_title: TypeScript and the Road to 1.0
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescript-and-the-road-to-1-0/
published: true
post_date: 2013-10-17 04:05:00
---
With TypeScript, we have focused on bridging the wildly popular JavaScript language with types for easy error detection, while bringing forward features from the [future version of JavaScript][1] in a way that’s compatible with modern browsers.  We’re now focused on getting to TypeScript 1.0, and in this post we’ll give an update on our plans to get us there.

# Visual Studio 2013 RC and RTM

We shipped TypeScript 0.9.1.1 as part of Visual Studio 2013 RC to get feedback from a broader range of Visual Studio users.  Given TypeScript’s continued evolution toward 1.0, we’ve decided to keep TypeScript as a separate download in Visual Studio, while continuing to make it easily available for all Visual Studio users. In Visual Studio 2013 RTM, you can install the latest version using the “Install the Latest TypeScript for Visual Studio” link provided in the new project templates.  Using this link will allow you to stay up to date with TypeScript as it evolves to 1.0.  The updates are also available [directly from the web][2].

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/0412.New_Project.png" alt="" border="0" />][3]

# Reliability and Correctness

As a community, we’ve grown quickly.  There are now dozens of large applications written by teams in Microsoft, including Bing, Xbox, [TFS][4], [So.cl][5], as well as in projects such as [Adobe Digital Publishing Suite for Windows 8.1][6], [Zud.io][7], [Away3D][8], and [Turbulenz][9].  Millions of lines of code are now active in the wild, which reflects a serious indication of the level of commitment and energy that that the community is investing in TypeScript. 

## Responding to Your Feedback

With the 0.9 release series, we delivered key new language features and a compiler re-architected for scalability. While these releases have opened up a lot of new capabilities, the feedback from our users is that these releases were not at the level of quality for teams who depend on TypeScript for their development. 

**Our goal is to ensure that the compiler is reliable. **

Users should be able depend on the TypeScript tools to build large applications without worrying about the tools crashing or consuming large amounts of memory.  We’ve already made progress on this front, with over 60 stability-related issues already fixed since our last public release in the develop branch.

## Backward Compatibility

Another important step for 1.0, and for the future of projects that use TypeScript, is that the compiler needs to correctly implement the language specification.  This will allow users to be able to write code and trust that once we get to 1.0, future compiler upgrades will continue to work with their existing code.  To get to this point, we will need to introduce a few last breaking changes as we complete the language spec and update the compiler and associated tooling.  We will be sure to document these changes and the code they affect.

# The Road to 1.0

We are planning the last milestones to get us to 1.0.  Each of these releases will be driven by quality, and we will ship each update when we’re confident we’ve addressed the key issues needed to enable development teams that depend on TypeScript to confidently move forward.  These milestones are also covered in our [updated roadmap][10].

**0\.9.5**

<p style="padding-left: 30px">
  The focus of 0.9.5 is resolving user-reported issues and setting a high quality bar.  This is also when we expect to have the 1.0 TypeScript Spec candidate ready.  This spec candidate will detail the complete TypeScript 1.0 language, and we’ll also document what parts of the implementation still need to be completed to finish alignment as well as enumerate the breaking changes from the previous version.
</p>

**1\.0RC**

<p style="padding-left: 30px">
  The 1.0RC includes fixes to any remaining reliability issues and completes the last of the compiler alignment with the 1.0 language spec.
</p>

**1\.0**

<p style="padding-left: 30px">
  We ship 1.0!  At this point we will be both aligned with the spec and ready to fully support development long-term by maintaining backward compatibility in each future release.
</p>

We have a clear picture of the road ahead to 1.0, and we’re looking forward to completing this work, making each release leading up to 1.0 even more reliable and robust than the last.  If you’d like to help us out, you can [get the latest ‘develop’ source][11] to keep up to date with the changes, and [report any issues][12] you find.  Lastly, but certainly not least, a big “thank you!” to the TypeScript community who has helped us make TypeScript what it is today.

 [1]: http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts
 [2]: http://www.microsoft.com/en-us/download/details.aspx?id=34790
 [3]: https://devblogs.microsoft.com/00/00/01/56/67/0412.New_Project.png
 [4]: http://blogs.msdn.com/b/bharry/archive/2012/10/24/typescript-a-real-world-story-of-adoption-in-tfs.aspx
 [5]: http://blog.fuselabs.org/post/37196309164/good-morning-its-a-big-day-for-us
 [6]: http://blogs.adobe.com/digitalpublishing/2013/06/dps-coming-to-windows-8-1.html
 [7]: http://zud.io
 [8]: http://away3d.com/
 [9]: https://turbulenz.com/
 [10]: http://typescript.codeplex.com/wikipage?title=Roadmap&referringTitle=Home
 [11]: https://typescript.codeplex.com/SourceControl/latest
 [12]: https://typescript.codeplex.com/WorkItem/Create