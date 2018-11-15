---
ID: 103
post_title: Announcing TypeScript 1.0
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-0/
published: true
post_date: 2014-04-02 07:45:00
---
# TypeScript 1.0

Today, we're happy to announce TypeScript 1.0. First, we want to give a big "thank you!" to everyone who has helped us over the last year and a half to meet this goal. Together, we've grown a language, tools, and a community around creating large-scale JavaScript applications. To help grow this community further, we're opening up TypeScript for contributions from the community starting today.  
   
When we introduced Typescript to the world with our first release, TypeScript 0.8 in October 2012, we couldn't have imagined how the community would take shape.Our first release began with a small set of developers regularly interacting and sharing ideas. From there, it has become a thriving community of contributors, creating large projects and sharing hundreds of .d.ts files built from thousands of contributions.  Codebases written in TypeScript have been the TypeScript project’s proving ground, with some exceeding a half a million lines of code and continuing to grow in scale and complexity.  It’s time to cut the ribbon on TypeScript 1.0.  
   
TypeScript 1.0 is available as part of [Visual Studio 2013 and Visual Studio Web Express 2013 Spring Update][1], as an [npm package][2], and as [source][3]. The TypeScript 1.0 release is also available as a [power tool for Visual Studio 2012][4].

TypeScript is now a first-class citizen of Visual Studio, and we'll continue to invest in both the language and in making Visual Studio a more productive environment for JavaScript and TypeScript development, with Intellisense, project support, and powerful code navigation features. Community-contributed plugins for editors such as Eclipse also offer a seamless editing experience with statement completion, early errors, linting, and more.

Below, you can see examples of TypeScript in Visual Studio 2013 and [Eclipse][5]:

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/4213.TypeScript_In_VS.png" alt="" border="0" />][6]

*TypeScript in Visual Studio 2013*

*[<img src="https://devblogs.microsoft.com/00/00/01/56/67/5001.TypeScript_In_Eclipse.png" alt="" border="0" />][7]*

*TypeScript in Eclipse*

**TypeScript 1.0 is a language designed with large-scale JavaScript application development in mind.  This offers a stable platform for users to build applications, and to create and share type definition files files that are compatible with future TypeScript versions.  TypeScript is also now a first-class citizen of Visual Studio, and we'll continue to invest in both the language and making Visual Studio a more productive environment for JavaScript and TypeScript development.

# Taking Pull Requests

Today, we're announcing that we will begin taking pull requests for the TypeScript compiler and language service.  
   
For now, we'll focus on fixing bugs, improving documentation, and general cleanup.  This will help us get the gears turning and work out any kinks in the submission process. This will be a learning process for us, and we will be open with the community throughout the process. Please be patient with us while we make the inevitable adjustments.  As we learn, we will continue to make TypeScript more open. We hope you'll be with us on this journey and contribute to the project.

## How can I contribute?

Great question! You can contribute by submitting a bug fix pull request today. The steps are [outlined on the TypeScript wiki][8].  
   
We're excited about taking this step to make TypeScript even more open and even more of a collaboration between the TypeScript team and the TypeScript community.

## Looking forward

Of course, our job isn't done. After we're done celebrating, we're getting back to work. The 1.0 release is just the beginning for TypeScript.   
   
Thanks again for being part of this journey with us. We look forward to the road ahead! 

 [1]: http://go.microsoft.com/fwlink/?LinkId=390521
 [2]: https://www.npmjs.org/package/typescript
 [3]: https://typescript.codeplex.com/releases/view/120282
 [4]: http://visualstudiogallery.msdn.microsoft.com/fa041d2d-5d77-494b-b0ba-8b4550792b4d
 [5]: https://github.com/palantir/eclipse-typescript
 [6]: https://devblogs.microsoft.com/00/00/01/56/67/4213.TypeScript_In_VS.png
 [7]: https://devblogs.microsoft.com/00/00/01/56/67/5001.TypeScript_In_Eclipse.png
 [8]: https://typescript.codeplex.com/wikipage?title=Guidelines%20for%20Contributors