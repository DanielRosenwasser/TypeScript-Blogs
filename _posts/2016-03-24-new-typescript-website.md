---
ID: 583
post_title: New TypeScript Website
author: Bowden Kelly
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/new-typescript-website/
published: true
post_date: 2016-03-24 18:36:58
---
Today we are thrilled to release a [new TypeScript website][1]! The new site delivers a fresh design, more up-to-date documentation, and soon a new series of quick start guides. 
## <a id="user-content-documentation" class="anchor" href="#documentation"></a>Documentation TypeScript is developed on GitHub, and as such, all of the documentation is written in markdown on 

[the TypeScript Handbook repo][2]. This is primarily done to easily allow contributions. Any community member can make edits to the documentation by simply dropping a [pull request][3] on the handbook repo. While this is great for community collaboration, previously, updating the website docs from the wiki was a very slow and manual process. The new site uses [Jekyll][4] to generate html pages from the wiki markdown pages. This means we can continue using GitHub to take community contributions on the docs, but also keep the website up-to-date with rapid updates. The new site surfaces several significant documentation changes: 
*   [Modules][5] and [namespaces][6] have received a full rewrite based on feedback from users about usage, compiling, and best practices.
*   [String literals][7], [`this` types][8], [type assertions][9], [enums][10], [`for..of` loops][11], [union types][12], [decorators][13], [triple-slash references][14], and [intersection types][15] now have their own sections detailing their usage.
*   A [What's New section][16] has been added to reflect the wiki and highlight feature sets by version number.

## <a id="user-content-quick-start-guides" class="anchor" href="#quick-start-guides"></a>Quick Start Guides One of the goals of TypeScript is to be usable anywhere that JavaScript is usable. However, given the vast combinations of JavaScript libraries, frameworks, and tools, a new user can at times be overwhelmed while making the switch. To combat this, we are starting a series of quick start guides targeting specific workflows that we frequently hear are tough to setup. So far, we have started the following on GitHub and will move them to the new website as they are completed: 

*   [React + Webpack + TypeScript][17]
*   [Knockout + TypeScript][18] Is there a specific subject or set of docs that you are having a hard time understanding? Is there a specific workflow you are having trouble adding TypeScript to? Please let us know by dropping a comment below or 

[creating an issue on GitHub][19]!

 [1]: http://typescriptlang.org
 [2]: http://github.com/microsoft/typescript-handbook
 [3]: http://github.com/microsoft/typescript-handbook/pulls
 [4]: http://jekyllrb.com/
 [5]: http://www.typescriptlang.org/docs/handbook/modules.html
 [6]: http://www.typescriptlang.org/docs/handbook/namespaces.html
 [7]: http://www.typescriptlang.org/docs/handbook/advanced-types.html#string-literal-types
 [8]: http://www.typescriptlang.org/docs/handbook/advanced-types.html#polymorphic-this-types
 [9]: http://www.typescriptlang.org/docs/handbook/basic-types.html#type-assertions
 [10]: http://www.typescriptlang.org/docs/handbook/enums.html
 [11]: http://www.typescriptlang.org/docs/handbook/iterators-and-generators.html#forof-statements
 [12]: http://www.typescriptlang.org/docs/handbook/advanced-types.html
 [13]: http://www.typescriptlang.org/docs/handbook/decorators.html
 [14]: http://www.typescriptlang.org/docs/handbook/triple-slash-directives.html
 [15]: http://www.typescriptlang.org/docs/handbook/advanced-types.html#intersection-types
 [16]: http://www.typescriptlang.org/docs/release-notes/typescript-1.8.html
 [17]: https://github.com/Microsoft/TypeScript-Handbook/blob/master/pages/tutorials/React%20%26%20Webpack.md
 [18]: https://github.com/Microsoft/TypeScript-Handbook/blob/master/pages/tutorials/Knockout.md
 [19]: https://github.com/Microsoft/TypeScript-Handbook/issues