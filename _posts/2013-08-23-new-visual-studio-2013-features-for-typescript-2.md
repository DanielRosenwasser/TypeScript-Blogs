---
ID: 1666
post_title: >
  New Visual Studio 2013 Features for
  TypeScript
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/new-visual-studio-2013-features-for-typescript-2/
published: true
post_date: 2013-08-23 14:32:00
---
As users expect more from the web – more features, more device support, more interactivity – ASP.NET developers are increasingly building large JavaScript applications. Starting with 0.9.1, a key focus for TypeScript's capabilities in the Visual Studio support is enabling TypeScript to work better with ASP.NET projects.

**Adding TypeScript to a Project**

To get started with TypeScript in an ASP.NET project, just add a TypeScript file by choosing *Add – TypeScript File* in the Solution Explorer right click menu, or by using the *Add – New Item* dialog. Adding a TypeScript file automatically onfigures the project to support compiling TypeScript. You can also rename JavaScript files with the “.ts” extension to convert them into TypeScript files. Since TypeScript is a superset of JavaScript, you can gradually migrate the scripts you already have by adding type annotations and restructuring the code to use classes, interfaces, and modules.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/0172.NewTypeScriptFile.png" alt="" width="382" height="330" border="0" />][1]

**Adding Declaration Files**

One of the best parts of JavaScript development is the wealth of useful, free libraries available online. To use these JavaScript libraries seamlessly in TypeScript apps, TypeScript supports declaration files (with the extension “.d.ts”) that describe the types of objects in JavaScript files. Through the community efforts, many popular JavaScript libraries now have tooling and error detection using these declaration files.  Sites like [DefinitelyTyped][2] have become indispensable for developers by collecting these declaration files together in one place.  Contributions by the community have mirrored these packages as NuGet, making them available to Visual Studio users.

In 0.9.1, with a right click you can now choose “Search for TypeScript Typings…” This searches NuGet for TypeScript-related packages matching the name of the JavaScript file. If you install a typing package, it will automatically be referenced by all of the TypeScript files in your project. You no longer need to explicitly add reference tags in your TypeScript code to each of the declaration files.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/6013.SearchNuGetTypings.png" alt="" width="378" height="239" border="0" />][3]

**[<img src="https://devblogs.microsoft.com/00/00/01/56/67/4834.SearchNuGetTypings2.png" alt="" border="0" />][4]**

 

**Debugging**

TypeScript 0.9.1 continues to make it easy to debug TypeScript code, letting you focus on the code you've written. Because the TypeScript compiler generates source map files that relate the symbols in your TypeScript code with its JavaScript output, you can set a breakpoint in a TypeScript file, open your app in Internet Explorer, and then step through your code in Visual Studio just like ordinary JavaScript. You can examine the values of objects, add watches, view the call stack, and more. Since TypeScript is compiled to clean, readable JavaScript, it’s also straightforward to debug your code in your browser’s developer tools.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/0410.DebugTypeScript.png" alt="" border="0" />][5]

**Organizing TypeScript and JavaScript in Solutions**

By default, TypeScript files are automatically compiled when they’re saved and when your solution builds. Generated JavaScript files are placed in the same directory as their TypeScript source files. Think of the generated JavaScript files like DLLs generated from C# or VB code: they’re not included in your project, so they don’t appear in the Solution Explorer. If you want to view the JavaScript files, you can use the “Show All Files” button at the top of the Solution Explorer. The [TypeScript Playground][6] is another great tool for quickly seeing how your TypeScript will be compiled.

**Other Editors**

We’ve released the TypeScript compiler and language as [open source][7] to make it easy for the community to build great tools. While we think Visual Studio 2013 provides the best TypeScript development experience, there are a number of other editors with TypeScript support available, including Eclipse, Web Storm, Sublime Text, emacs, and vim.

We hope that these features make it seamless to use TypeScript in ASP.NET projects in Visual Studio 2013. As always, send us feedback on the [TypeScript CodePlex page][8].

 [1]: https://devblogs.microsoft.com/00/00/01/56/67/0172.NewTypeScriptFile.png
 [2]: https://github.com/borisyankov/DefinitelyTyped
 [3]: https://devblogs.microsoft.com/00/00/01/56/67/6013.SearchNuGetTypings.png
 [4]: https://devblogs.microsoft.com/00/00/01/56/67/4834.SearchNuGetTypings2.png
 [5]: https://devblogs.microsoft.com/00/00/01/56/67/0410.DebugTypeScript.png
 [6]: http://www.typescriptlang.org/Playground/
 [7]: http://typescript.codeplex.com/sourcecontrol/latest#README.txt
 [8]: http://typescript.codeplex.com/discussions