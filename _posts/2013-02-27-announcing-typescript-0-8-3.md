---
ID: 233
post_title: Announcing TypeScript 0.8.3
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-8-3/
published: true
post_date: 2013-02-27 10:00:00
---
Today, we're happy to announce the [0\.8.3 preview release][1] of TypeScript.  This release adds incremental polish and bugfixes to the TypeScript compiler, language service, and Visual Studio plugin. 

## Compiler Improvements

In the 0.8.3 preview release, we fixed a few key issues that improve compiler correctness and performance, including[ issues with 'this' typing][2], [external modules][3], and [scoping][4]. The 0.8.3 preview also includes improvements to IO buffering which should now allow large projects to compile much more quickly when compiled using Node as the JavaScript host. 

## Visual Studio Debugger Improvements

The Visual Studio support for source level debugging of TypeScript no longer requires that the files being debugged are part of the current project.  This allows Visual Studio to attach to a running instance of Internet Explorer and debug TypeScript projects remotely.  To do so, attach to the running IE instance which has the Script type, with "Attach to:" set to Script code.

After you attach to a running web application, you will see the associated files show up in the Script Documents in the Solution Explorer:

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/2273.TS_in_ScriptDocuments.PNG" alt="" border="0" />][5]

This will let you open the associated TypeScript files and debug using them directly.

## Visual Studio Editor Improvements

In this preview release, the quick info now shows a much more complete picture of the symbol being hovered.  The info during hover now includes the kind of symbol and the symbol's name in addition to the type.  Additionally, hover information is now more uniform, and in some cases even more concise, letting developers focus on the necessary information about each symbol.

 

<table border="0" cellspacing="10" cellpadding="10">
  <tbody>
    <tr>
      <td>
        <img src="https://devblogs.microsoft.com/00/00/01/56/67/7701.Hover_0.8.2_revised.png" alt="" border="0" />
      </td>
    </tr>
    
    <tr>
      <td>
         <a href="https://devblogs.microsoft.com/00/00/01/56/67/1682.Hover_0.8.3_revised.png"><img src="https://devblogs.microsoft.com/00/00/01/56/67/1682.Hover_0.8.3_revised.png" alt="" border="0" /></a>
      </td>
    </tr>
    
    <tr>
      <td style="text-align: center">
        <em> Quick Info Hover in 0.8.2 (top) and 0.8.3 (bottom)</em>
      </td>
    </tr>
  </tbody>
</table>

 

## Coming Soon 

While we've been finishing up the 0.8 train with this release, work is already underway on the upcoming 0.9 preview release.  The 0.9 preview release will include some of the larger language updates and commonly requested features.  More details coming soon.

 [1]: http://www.typescriptlang.org/#Download
 [2]: http://typescript.codeplex.com/workitem/660
 [3]: http://typescript.codeplex.com/workitem/416
 [4]: http://typescript.codeplex.com/workitem/711
 [5]: https://devblogs.microsoft.com/00/00/01/56/67/2273.TS_in_ScriptDocuments.PNG