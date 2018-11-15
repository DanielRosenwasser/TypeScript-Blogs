---
ID: 1823
post_title: Announcing TypeScript 1.1 CTP
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-1-ctp/
published: true
post_date: 2014-10-06 09:30:00
---
Today, we’re making TypeScript 1.1 CTP immediately available for [Visual Studio “14” CTP4][1], [Visual Studio 2013][2], [npm][3], and as a [source][4] release.  1.1 CTP is the first release of TypeScript to include the new compiler core redesigned to both perform better and lay the groundwork for future language features.  You can see the real-world improvements in the graph below.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/8032.compare_1.1_to_1.0.2.png" alt="" border="0" />][5]

The 1.1 CTP compiler is compatible with projects built with the 1.0 compiler. While it's drop-in replaceable, this is still CTP-level quality, so you may encounter issues of stability or incompatibility.  If you do, please let us know by [filing an issue][6] on our GitHub page.

 

## Migrating Visual Studio projects

If you are using TypeScript in Visual Studio, you can move your TypeScript 1.0 projects to use the new 1.1 CTP tools by editing your project, as shown below.

**Step 1: Unload your project**

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/0825.unload_project.png" alt="" border="0" />][7]

**Step 2: Edit your project**

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/7242.edit_project.png" alt="" border="0" />][8]

**Step 3: Update the TypeScriptToolsVersion line to say "1.1"**

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/4456.update_version.png" alt="" border="0" />][9]

 

## Looking ahead

While this release updates the compiler, the editing experience in Visual Studio is still powered by the 1.0 compiler technology.  Our next goal is to bring this language service up to date with the rewritten compiler and finish modernizing the experience for command line compilation and IDE editing.  This will allow IntelliSense, code navigation, and other code editing tools such as Visual Studio a way to leverage the new compiler infrastructure. Until the language service has moved to the new compiler, you may see differences in error reporting. This work and the work to align with ECMAScript 6 is ongoing.  If you’d like to help with this work by contributing bug fixes, filing bugs, and trying out the latest code, please join us on our [GitHub repository][4].

 [1]: http://go.microsoft.com/fwlink/?LinkId=400850
 [2]: http://go.microsoft.com/fwlink/?LinkId=512922
 [3]: https://www.npmjs.org/package/typescript
 [4]: https://github.com/microsoft/typeScript/
 [5]: https://devblogs.microsoft.com/00/00/01/56/67/8032.compare_1.1_to_1.0.2.png
 [6]: https://github.com/microsoft/typescript/issues
 [7]: https://devblogs.microsoft.com/00/00/01/56/67/0825.unload_project.png
 [8]: https://devblogs.microsoft.com/00/00/01/56/67/7242.edit_project.png
 [9]: https://devblogs.microsoft.com/00/00/01/56/67/4456.update_version.png