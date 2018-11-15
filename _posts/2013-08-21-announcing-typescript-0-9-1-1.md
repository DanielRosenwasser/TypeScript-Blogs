---
ID: 163
post_title: Announcing TypeScript 0.9.1.1
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-9-1-1/
published: true
post_date: 2013-08-21 09:26:00
---
Today, we’re releasing TypeScript 0.9.1.1, a hotfix release of [TypeScript 0.9.1][1].  The 0.9.1.1 release addresses 25 issues discovered by the community and includes fixes for compiler crashes, memory consumption, type system correctness, and more.

This release also temporarily adds back in the flags to allow “import x = module(…)” and the ‘bool’ type (--allowimportmodule and --allowbool, respectively), to help large codebases that have yet to upgrade.  This is the last release to support these deprecated syntax forms, so please update your codebases to get them ready for 1.0.

The updated downloads are available for [Visual Studio, NPM and source][2] through the TypeScript website.  Let us know what you think on the [discussion forums][3] and [issue tracker][4].

*Note to users of previous versions of TypeScript - there is a list of breaking changes with older versions listed [here][5]. *

 

 [1]: http://blogs.msdn.com/b/typescript/archive/2013/08/06/announcing-0-9-1.aspx
 [2]: http://www.typescriptlang.org/#Download
 [3]: http://typescript.codeplex.com/discussions
 [4]: http://typescript.codeplex.com/workitem/list/basic
 [5]: http://typescript.codeplex.com/wikipage?title=Known%20breaking%20changes%20between%200.8%20and%200.9&referringTitle=Documentation