---
ID: 123
post_title: Announcing TypeScript 0.9.5
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-9-5/
published: true
post_date: 2013-12-05 11:38:00
---
Today, we're happy to announce the release of TypeScript 0.9.5.  This release focuses on addressing feedback from the community around quality and stability.  In this release, we've fixed over 100 issues related to memory leaks, CPU usage, crashes, and compiler correctness.  This has helped the TypeScript experience for developing large-scale JavaScript applications become a more robust and more reliable experience.

The updated downloads are available for [Visual Studio, NPM and source][1] through the TypeScript website.  Let us know what you think on the [discussion forums][2] and [issue tracker][3].

As part of the work to address community feedback and to making the language more cohesive and predictable in preparation for our 1.0 release, there have been some changes to the TypeScript language.  In some cases, this has resulted in breaking changes with existing code, so please be sure to check out the [breaking change list][4] to see if any of the changes impact your code.

## Growing TypeScript Community

It's been an exciting time for the TypeScript community.  There are now over 200 .d.ts files available at [DefinitelyTyped][5], covering a wide range of 3rd party JavaScript libraries and services.  Equally impressive, this large catalog of typing files is maintained by a group of nearly 300 individual contributors.  DefinitelyTyped has grown to be an invaluable resource when working with TypeScript.

Recently announced is [Monaco][6], part of the new Visual Studio Online, an online source management and editing environment built with TypeScript.  [Erich Gamma spoke at JSConf][7] about the challenges of large-scale JavaScript applications like Monaco and how TypeScript helped the Monaco project grow.  Also announced is [Xbox Music][8], a rich music service available to millions of users via Xbox One, Windows 8.1, and the web, which was also built with TypeScript. 

## Build Performance

As a result of the work on memory usage and stability in this release, we've seen build times for large projects improve.  For example, Monaco and Xbox Music saw their compile times cut nearly in half by switching to 0.9.5.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/6355.compile_time_095_2.png" alt="" border="0" />][9]

 Along with the compile time improvements, both projects also saw nearly a 50% decrease in memory usage during build time and a dramatic increase in the stability of the language service and associated tools.

## Visual Studio Improvements

We've continued to polish the Visual Studio experience when developing TypeScript projects.  In 0.9.5, we've added two new features to help fill out the development experience.

### Project Property Page

In Visual Studio 2013, you will now be able to configure your projects using the new project property page.  The page allows easy control over many common project settings, including preferred module system for codegen, concatenating output, outputting .d.ts files, and managing debugging.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/7610.Project%20Property%20Page2.PNG" alt="" border="0" />][10]

### Using <span style="font-family: courier new,courier">_references.ts</span>

With the previous improvements to the Visual Studio experience, we've moved to projects implicitly referencing the .ts files contained in the project.  This cut down on having to explicitly reference your files in the project, bringing the experience much closer to C#.  Unfortunately, it also did not work well when using the option to concatenate your output .js file.

We're continuing to improve this experience.  Starting with 0.9.5, you can now add an <span style="font-family: courier new,courier">_references.ts</span> file to your project.  This file will be the first passed to the compiler, allowing you more control over the order the generated .js file when used in combination with the **Combine JavaScript output into file** option (the equivalent of using the --out commandline option).

## Looking Forward

As we mentioned in the roadmap as part of the [TypeScript and the Road to 1.0][11] post, we're excited to be putting the last bit of polish on the TypeScript language and tools.  The 0.9.5 is a key piece of that, and we'd love to hear your feedback.  Looking forward from here, the next release will be the release candidate for the 1.0 itself, which will represents the TypeScript 1.0 language and tooling and will be the most complete picture of TypeScript 1.0 before its release.

 

 [1]: http://www.typescriptlang.org/#Download
 [2]: http://typescript.codeplex.com/discussions
 [3]: http://typescript.codeplex.com/workitem/list/basic
 [4]: https://typescript.codeplex.com/wikipage?title=Known%20breaking%20changes%20between%200.8%20and%200.9&referringTitle=Documentation
 [5]: https://github.com/borisyankov/DefinitelyTyped
 [6]: https://channel9.msdn.com/Series/Visual-Studio-Online-Monaco
 [7]: https://www.youtube.com/watch?v=1B9p691U_Ms
 [8]: http://music.xbox.com/
 [9]: https://devblogs.microsoft.com/00/00/01/56/67/6355.compile_time_095_2.png
 [10]: https://devblogs.microsoft.com/00/00/01/56/67/7610.Project%20Property%20Page2.PNG
 [11]: http://blogs.msdn.com/b/typescript/archive/2013/10/17/typescript-and-the-road-to-1-0.aspx