---
ID: 263
post_title: Announcing TypeScript 0.8.2
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-8-2/
published: true
post_date: 2013-01-21 13:50:00
---
We’re happy to announce the [0\.8.2 preview release][1] of TypeScript.  This release has been focused on  addressing key usability feedback and filling in tooling experiences.  Along the way, we’ve also incorporated some of the highest-rated feature requests and bugfixes.

## JSDoc Support

Being able to see API documentation comments during Intellisense and parameter help has been a highly requested feature.  In fact, it’s currently the [2nd most-requested][2] feature in the issue tracker.

With 0.8.2, the TypeScript compiler and tools now support JSDoc comments.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/1200.JSDoc_in_VS.png" alt="" border="0" />][3]

In the TypeScript implementation, because types are already part of the system, we allow the JSDoc type annotation to be elided, as in the example above.

You can now document a variety of language constructs (including classes, modules, interfaces, and functions) with comments that become part of the information displayed to the user.  We’ve also started extending lib.d.ts, the default JS and DOM API library, with JSDoc comments.

This capability is also part of the language service, so cross-platform tools can take advantage of it.

## Compile-on-Save

Many of the themes in this release center around simplifying workflow.  We’ve focused on a few key areas with the aim of streamlining developing in TypeScript.

Languages like TypeScript which compile to JavaScript add a build step into the development cycle.  Enabling a compile-on-save mode of use minimizes this overhead by automatically updating the compiled JS when the TypeScript source changes.  With 0.8.2, we’ve done work to further enable this in both the NPM-based compiler and in Visual Studio.

The 0.8.2 release adds initial support for compile-on-save in Visual Studio.  Just as the name implies, when enabled the project or file being edited will be compiled when the changes are saved.  For more information, please see the [wiki page for compile-on-save][4].

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/4744.Compileonsave.PNG" alt="" border="0" />][5]

 

TypeScript supports compile-on-save on the command line using the cross-platform NPM-based TypeScript compiler as well with the improved --watch compiler option.  This option allows a compile to begin watching a set of files.  Once a watched file is changed, the compiler rebuilds the corresponding output for that file.

## Various Improvements

*   We’ve further improved the Visual Studio TypeScript debugging experience in 0.8.2.  The debugger will now honor breakpoints after F5 (refresh) during a debug session.  Combined with the addition of compile-on-save, the workflow to edit and debug a web app is as easy as saving the edited changes and refreshing the browser.
*   It’s possible now to specify output directory when compiling external modules.  This gives a cleaner way to build external modules without having the output in-tree.
*   The compiler service can now be replaced in the TypeScript for Visual Studio 2012 plugin.  This makes it easier to leverage the Visual Studio TypeScript extension with newer compiler builds from CodePlex or from experimental forks.
*   We’ve moved to [Jake][6] for building the compiler and language service.  Jake is a simpler solution which should make it easier to work with and build the compiler in a multiplatform-friendly way.

 [1]: http://www.typescriptlang.org/#Download
 [2]: http://typescript.codeplex.com/workitem/178
 [3]: https://devblogs.microsoft.com/00/00/01/56/67/1200.JSDoc_in_VS.png
 [4]: http://typescript.codeplex.com/wikipage?title=Compile-on-Save
 [5]: https://devblogs.microsoft.com/00/00/01/56/67/4744.Compileonsave.PNG
 [6]: https://npmjs.org/package/jake