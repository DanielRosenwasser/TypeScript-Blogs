---
ID: 461
post_title: 'Announcing TypeScript 1.6 Beta: React/JSX, better error checking, and more'
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-6-beta-reactjsx-better-error-checking-and-more/
published: true
post_date: 2015-09-02 13:00:00
---
Today, we’re making a beta of the upcoming TypeScript 1.6 available.  There are a [bunch of new features][1] coming in the 1.6 release, and we wanted to give you a preview of these features and time to give us [feedback][2].  

You can get this for [Visual Studio 2015][3], [Visual Studio 2013][4], [NPM][5], and [source][6].

# React/JSX support

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/8512.react.png" alt="" width="200" height="179" />][7]

One of the key philosophies of TypeScript is to let you write TypeScript anywhere you can develop using JavaScript.  While we’ve worked with teams such as Dojo, Aurelia, and Angular to ensure using TypeScript is as easy as using JavaScript, there was still an important library that that presented a [difficulty][8] for TypeScript developers: React.  This was due to the lack of support for JSX, a popular way of writing DOM and native components in JS.  React heavily leverages JSX in everyday code.  Unfortunately, the syntax for JSX conflicted with the cast syntax that TypeScript already used.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/6177.Renaming.gif" alt="" border="0" />][9]

*Refactoring JSX members in TypeScript (click to watch animation)  
*

In 1.6, we’ve introduced a [new .tsx file extension][10].  This extension does two things: it enables JSX inside of TypeScript files, and it makes the new ‘as’ operator the default way to cast.  With this, we’ve added full support for working with React/JSX in TypeScript in a type-safe way. 

# Catching more errors

Starting with 1.6, we’re tightening up some of our object checking rules.  In the beta, you’ll see that objects need to [match more closely when they are assigned][11].  For example:

<span style="font-family: courier new,courier"><span style="color: #0000ff">var</span> x: { foo: <span style="color: #0000ff">number</span> };</span>  
<span style="font-family: courier new,courier">x = { foo: 1, baz: 2 };  <span style="color: #008000">// Error, excess property 'baz', but not caught before 1.6</span></span>

<span style="font-family: courier new,courier"><span style="color: #0000ff">var</span> y: { foo: <span style="color: #0000ff">number</span>, bar?: <span style="color: #0000ff">number</span> };</span>  
<span style="font-family: courier new,courier">y = { foo: 1, baz: 2 };  <span style="color: #008000">// Error, excess or misspelled property 'baz', also not caught before 1.6</span></span>

When fields are optional, it’s easy to accidentally pass mistyped fields or miss when a refactoring has left excess fields.  This change has already helped to find dozens (if not hundreds) of real-world bugs in early adopter code.  As part of this change, we found [bugs in DefinitelyTyped][12] (including the tests that are used to validate the hand-written .d.ts files):

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/4722.definitelytyped_fixes.png" alt="" border="0" />][13]

*Examples of the errors caught with the new rules*

While this change may show where bugs have been hiding previously, you may not be ready to tackle a new set of compiler errors in your existing code right away, or you may not want to change the way your code behaves.  There’s a [good list][14] of ways to work around this assignment check, depending on the capability you want your code to have.

**EDIT: **You can also suppress this warning by passing the [--suppressExcessPropertyErrors][15] compiler option.

# Improved module resolution

Previously, TypeScript used a hybrid approach for resolving both CommonJS and RequireJS.  Unfortunately, this meant that module resolution didn’t feel at home in either style, and instead made you structure projects in a way that didn’t feel natural for that particular module loader.

We’re working on a [set of improvements][16] to make module resolution more natural.  In the 1.6 beta, you’ll see the start of that work.  We invite you to [read more][16] on what we’re working on and send us your [feedback][16].

# And more

We’ve continued to improve ES6 support with added support for [class expressions][17] and the start of support for [generators][18].  You can see the full list of new features on our [roadmap][1].  We’d love to hear your [feedback][18].

 [1]: https://github.com/Microsoft/TypeScript/wiki/Roadmap
 [2]: https://github.com/Microsoft/TypeScript/issues
 [3]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS1.6-Beta-D14OOB.23301.00/TypeScript_Full.exe
 [4]: http://download.microsoft.com/download/4/4/3/443F86B7-A89F-48E6-AC96-0AAC2A910A29/TS1.6-Beta-VUOOB.40831.00/TypeScript_Dev12.exe
 [5]: https://www.npmjs.com/package/typescript
 [6]: https://github.com/Microsoft/TypeScript/releases/tag/v1.6.0-beta
 [7]: https://devblogs.microsoft.com/00/00/01/56/67/8512.react.png
 [8]: https://www.youtube.com/watch?v=9PTa9-PPVAc
 [9]: https://devblogs.microsoft.com/00/00/01/56/67/6177.Renaming.gif
 [10]: https://github.com/Microsoft/TypeScript/pull/3564
 [11]: https://github.com/Microsoft/TypeScript/pull/3823
 [12]: https://github.com/borisyankov/DefinitelyTyped/pull/5151
 [13]: https://devblogs.microsoft.com/00/00/01/56/67/4722.definitelytyped_fixes.png
 [14]: http://stackoverflow.com/questions/31816061/why-am-i-getting-an-error-object-literal-may-only-specify-known-properties
 [15]: https://github.com/Microsoft/TypeScript/pull/4484
 [16]: https://github.com/Microsoft/TypeScript/issues/2338
 [17]: https://github.com/Microsoft/TypeScript/issues/497
 [18]: https://github.com/Microsoft/TypeScript/issues/2873