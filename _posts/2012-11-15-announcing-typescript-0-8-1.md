---
ID: 303
post_title: Announcing TypeScript 0.8.1
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-8-1/
published: true
post_date: 2012-11-15 10:44:00
---
# <span style="color: #2e74b5"><span style="font-family: Calibri Light">Announcing TypeScript 0.8.1</span></span>

<span style="font-family: Calibri;font-size: small">We released the first public preview of <a href="http://typescriptlang.org" target="_blank">TypeScript</a> last month, and have been excited to see the great reaction from the developer community.  Since the preview, we’ve seen an influx of suggestions and bug reports, integration into various build and testing tools, and </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">declare files for a variety of existing JavaScript libraries</span>][1]<span style="font-size: small"><span style="font-family: Calibri">.  </span></span>

<span style="font-size: small"><span style="font-family: Calibri">Today, we’re happy to announce the second publically available preview of TypeScript: version 0.8.1.  This preview focuses on addressing critical bugs and adding one of the most requested features: source level debugging.</span></span>

<span style="font-size: medium"><span style="color: #2e74b5"><span style="font-family: Calibri Light">Debugging</span></span></span>

<span style="font-family: Calibri;font-size: small">TypeScript now supports source level debugging!  The </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">source map</span>][2]<span style="font-size: small"><span style="font-family: Calibri"> format has been gaining popularity as a way of debugging languages which translate to JavaScript and is supported by a variety of browsers and tools.  With version 0.8.1, the TypeScript compiler officially supports source maps.  Additionally, the new version of TypeScript for Visual Studio 2012 supports debugging using the source map format.</span></span>

<span style="font-size: small"><span style="font-family: Calibri">From the command-line, we now fully support the use of the </span>--sourcemap<span style="font-family: Calibri"> flag, which outputs a source map file corresponding to the JavaScript output.  This file will allow directly debugging the original TypeScript source in source map-enabled browsers and Visual Studio.</span></span>

<span style="font-size: small"><span style="font-family: Calibri">To enable debugging in Visual Studio select ‘Debug’ from the dropdown after creating an HTML Application with TypeScript project.</span></span>

<span style="font-size: small"><span style="font-family: Calibri"><a href="https://devblogs.microsoft.com/00/00/01/56/67/0083.debugconfig.png"><img src="https://devblogs.microsoft.com/00/00/01/56/67/0083.debugconfig.png" alt="" border="0" /></a></span></span>

<span style="font-size: small"><span style="font-family: Calibri">Once selected, you can set breakpoints and step directly in your TypeScript code.</span></span>

<span style="font-size: small"><span style="font-family: Calibri"><a href="https://devblogs.microsoft.com/00/00/01/56/67/4130.debuggreeter.png"><img src="https://devblogs.microsoft.com/00/00/01/56/67/4130.debuggreeter.png" alt="" border="0" /></a>  </span></span>

<span style="font-size: small"><span style="font-family: Calibri">You can also see your current location using the call stack.  </span></span>

 [<img src="https://devblogs.microsoft.com/00/00/01/56/67/2022.debugcallstack.png" alt="" border="0" />][3]

<span style="font-size: medium"><span style="color: #2e74b5"><span style="font-family: Calibri Light">Bug Fixes</span></span></span>

<span style="font-family: Calibri;font-size: small">This release addresses many issues found in our first public preview.  These fixes have improved compiler stability, compiler correctness, and UTF-8 support.  A big “thank you!” to everyone who has helped so far by </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">reporting bugs</span>][4]<span style="font-size: small"><span style="font-family: Calibri"> and participating in the issue tracker and forums.</span></span>  

<span style="font-size: medium"><span style="color: #2e74b5"><span style="font-family: Calibri Light">Breaking Changes</span></span></span>

<span style="font-size: small"><span style="font-family: Calibri">There are two known breaking changes since v0.8.0:</span></span> 

*   <span style="font-size: small"><span style="font-family: Calibri">Projects created from the Visual Studio templates using v0.8.0 will need to be manually updated to work with v0.8.1.  Going forward, this should not be necessary.  <a href="http://typescript.codeplex.com/discussions/403394">This post </a>contains details of the MSBuild to use when converting projects.</span></span> 
*   <span style="font-size: small"><span style="font-family: Calibri">The compiler was previously incorrectly allowing non-exported interfaces in a module to be used from outside the module.  These must now be exported explicitly per the specification. </span></span>

<span style="font-size: medium"><span style="color: #2e74b5"><span style="font-family: Calibri Light">Conclusion</span></span></span>

<span style="font-family: Calibri;font-size: small">The version 0.8.1 preview is now available for </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">Visual Studio</span>][5]<span style="font-family: Calibri;font-size: small">, </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">npm</span>][6]<span style="font-family: Calibri;font-size: small">, and </span>[<span style="color: #0563c1;font-family: Calibri;font-size: small">source</span>][7]<span style="font-size: small"><span style="font-family: Calibri">.  We’re excited to see how people will continue using, building upon, and integrating TypeScript into their JavaScript development, and we’d love to get your feedback.</span></span>

 

 [1]: https://github.com/borisyankov/DefinitelyTyped
 [2]: https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k/edit?pli=1
 [3]: https://devblogs.microsoft.com/00/00/01/56/67/2022.debugcallstack.png
 [4]: http://typescript.codeplex.com/WorkItem/Create
 [5]: http://go.microsoft.com/fwlink/?LinkID=266563
 [6]: https://npmjs.org/package/typescript
 [7]: http://typescript.codeplex.com/SourceControl/BrowseLatest