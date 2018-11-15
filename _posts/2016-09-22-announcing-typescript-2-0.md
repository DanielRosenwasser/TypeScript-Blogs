---
ID: 815
post_title: TypeScript 2.0 is now available!
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-0/
published: true
post_date: 2016-09-22 16:04:03
---
Today we're excited to announce the final release of TypeScript 2.0! TypeScript 2.0 has been a great journey for the team, with several contributions from the community and partners along the way. It brings several new [features][1] that enhance developer productivity, advances TypeScript's alignment with ECMAScript's evolution, provides wide support for JavaScript libraries and tools, and augments the language service that powers a first class editing experience across tools. To get started, you can download [TypeScript 2.0 for Visual Studio 2015][2] (which needs [Update 3][3]), grab it with NuGet, start using TypeScript 2.0 [in Visual Studio Code][4], or install it with npm: <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@2.0</pre>
</div> For Visual Studio "15" Preview users, TypeScript 2.0 will be included in the next Preview release. 

## <a href="#the-20-journey" id="user-content-the-20-journey" class="anchor"></a>The 2.0 Journey A couple of years ago we set out on this journey to version 2.0. TypeScript 1.0 had successfully shown developers the potential of JavaScript when combined with static types. Compile-time error checking saved countless hours of bug hunting, and TypeScript's editor tools gave developers a huge productivity boost as they began building larger and larger JavaScript apps. However, to be a full superset of the most popular and widespread language in the world, TypeScript still had some growing to do. TypeScript 1.1 brought a new, completely rewritten compiler that delivered a 

[4x performance boost][5]. This new compiler core allowed more flexibility, faster iteration, and provided a performance baseline for future releases. Around the same time, the TypeScript repository [migrated to GitHub][6] to encourage community engagement and provide a better platform for collaboration. TS 1.4 & 1.5 introduced a large amount of [support for ES2015/ES6][7] in order to align with the future of the JavaScript language. In addition, TypeScript 1.5 introduced support for modules and decorators, allowing [Angular 2][8] to [adopt TypeScript][9] and partner with us in the evolution of TypeScript for their needs. TypeScript 1.6-1.8 delivered substantial [type][10] [system][11] [improvements][12], with each new release lighting up additional JavaScript patterns and providing support for major JavaScript libraries. These releases also rounded out ES* support and buffed up the compiler with more advanced out-of-the-box error checking. Today we're thrilled to release version 2.0 of the TypeScript language. With this release, TypeScript delivers close ECMAScript spec alignment, wide support for JavaScript libraries and tools, and a language service that powers a first class editing experience in all major editors; all of which come together to provide an even more productive and scalable JavaScript development experience. 
## <a href="#the-typescript-community" id="user-content-the-typescript-community" class="anchor"></a>The TypeScript Community Since 1.0, TypeScript has grown not only as a language but also as a community. Last month alone, TypeScript had over 2 million npm downloads compared to just 275K in the same month last year. In addition, we've had tremendous adoption of the TypeScript nightly builds with over 2000 users participating in discussion on GitHub and 1500 users logging issues. We've also accepted PRs from over 150 users, ranging from bug fixes to prototypes and major features. 

[DefinitelyTyped][13] is another example of our community going above and beyond. Starting out as a small repository of declaration files (files that describe the shape of your JS libraries to TypeScript), it now contains over 2,000 libraries that have been written by-hand by over 2,500 individual contributors. It is currently the largest formal description of JavaScript libraries that we know of. By building up DefinitelyTyped, the TypeScript community has not only supported the usage of TypeScript with existing JavaScript libraries but also better defined our understanding of all JavaScript code. The TypeScript and greater JavaScript communities have played a major role in the success that TypeScript has achieved thus far, and whether you've contributed, tweeted, tested, filed issues, or used TypeScript in your projects, we're grateful for your continued support! <img src="https://msdnshared.blob.core.windows.net/media/2016/09/ts-2-0-npm-downloads.png" class="aligncenter" /> 
## <a href="#whats-new-in-typescript-20" id="user-content-whats-new-in-typescript-20" class="anchor"></a>What's New in TypeScript 2.0? TypeScript 2.0 brings several new features over the 1.8 release, some of which we detailed in the 2.0 

[Beta][14] and [Release Candidate][15] blog posts. Below are highlights of the biggest features that are now available in TypeScript, but you can read about [tagged unions][16], the new [`never` type][17], [`this` types][18] for functions, [glob support][19] in tsconfig, and all the other new features [on our wiki][1]. 
### <a href="#simplified-declaration-file-dts-acquisition" id="user-content-simplified-declaration-file-dts-acquisition" class="anchor"></a>Simplified Declaration File (<code style="font-size: 1em">.d.ts</code>) Acquisition Typings and tsd have been fantastic tools for the TypeScript ecosystem. Up until now, these package managers helped users get .d.ts files from DefinitelyTyped to their projects as fast as possible. Despite these tools, one of the biggest pain points for new users has been learning how to acquire and manage declaration file dependencies from these package managers. Getting and using declaration files in 2.0 is much easier. To get declarations for a library like lodash, all you need is npm: 

<div class="highlight highlight-source-shell">
  <pre>npm install --save @types/lodash</pre>
</div> The above command installs the 

[scoped package][20] `@types/lodash` which TypeScript 2.0 will automatically reference when importing `lodash` anywhere in your program. This means you don’t need any additional tools and your `.d.ts` files can travel with the rest of your dependencies in your `package.json`. It’s worth noting that both Typings and tsd will continue to work for existing projects, however 2.0-compatible declaration files may not be available through these tools. As such, we strongly recommend upgrading to the new npm workflow for TypeScript 2.0 and beyond. We'd like to thank Blake Embrey for his work on Typings and helping us bring this solution forward. 
### <a href="#non-nullable-types" id="user-content-non-nullable-types" class="anchor"></a>Non-nullable Types JavaScript has two values for "emptiness" - 

`null` and `undefined`. If `null` is [the billion dollar mistake][21], `undefined` only doubles our losses. These two values are a huge source of errors in the JavaScript world because users often forget to account for `null` or `undefined` being returned from APIs. TypeScript originally started out with the idea that types were always nullable. This meant that something with the type `number` could also have a value of `null` or `undefined`. Unfortunately, this didn’t provide any protection from `null`/`undefined` issues. In TypeScript 2.0, `null` and `undefined` have their own types which allows developers to explicitly express when `null`/`undefined` values are acceptable. Now, when something can be either a `number` or `null`, you can describe it with the union type `number | null` (which reads as "number or null"). <video autoplay loop src="https://msdnshared.blob.core.windows.net/media/2016/09/nonnullable-types-fade.mp4" style='width: 100%'> </video> Because this is a breaking change, we've added a `--strictNullChecks` mode to opt into this behavior. However, going forward it will be a general best practice to turn this flag on as it will help catch a wide range of `null`/`undefined` errors. To read more about non-nullable types, check out [the PR on GitHub][22]. 
### <a href="#control-flow-analyzed-types" id="user-content-control-flow-analyzed-types" class="anchor"></a>Control Flow Analyzed Types TypeScript has had control flow analysis since 1.8, but starting in 2.0 we've expanded it to analyze even more control flows to produce the most specific type possible at any given point. When combined with non-nullable types, TypeScript can now do much more complex checks, like definite assignment analysis. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> <span class="pl-en">f</span>(<span style="color: #000000">condition</span>: <span style="color: #267F99">boolean</span>) {
    <span style="color: #0000ff">let</span> <span style="color: #000000">result</span>: <span style="color: #267F99">number</span>;
    <span style="color: #0000ff">if</span> (condition) {
        result <span style="color: #0000ff">=</span> computeImportantStuff();
    }

    <span style="color: #148A14">// Whoops! 'result' might never have been initialized!</span>
    <span style="color: #0000ff">return</span> result;
}</pre>
</div> We’d like to thank Ivo Gabe de Wolff for contributing the initial work and providing substantial feedback on this feature. You can read more about control flow analysis on 

[the PR itself][23]. 
### <a href="#the-readonly-modifier" id="user-content-the-readonly-modifier" class="anchor"></a>The <code style="font-size: 1em">readonly</code> Modifier Immutable programming in TypeScript just got easier. Starting TypeScript 2.0, you can declare properties as read-only. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">Person</span> {
    <span style="color: #0000ff">readonly</span> <span style="color: #000000">name</span>: <span style="color: #267F99">string</span>;

    <span style="color: #0000ff">constructor</span>(<span style="color: #000000">name</span>: <span style="color: #267F99">string</span>) {
        <span style="color: #0000ff">if</span> (name.length <span style="color: #0000ff">&lt;</span> <span style="color: #267F99">1</span>) {
            <span style="color: #0000ff">throw</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">Error</span>(<span style="color: #a31515">"Empty name!"</span>);
        }

        <span style="color: #0000ff">this</span>.name <span style="color: #0000ff">=</span> name;
    }
}

<span style="color: #148A14">// Error! 'name' is read-only.</span>
<span style="color: #0000ff">new</span> <span style="color: #267F99">Person</span>(<span style="color: #a31515">"Daniel"</span>).name <span style="color: #0000ff">=</span> <span style="color: #a31515">"Dan"</span>;</pre>
</div> Any get-accessor without a set-accessor is also now considered read-only. 

## <a href="#whats-next" id="user-content-whats-next" class="anchor"></a>What’s Next TypeScript is JavaScript that scales. Starting from the same syntax and semantics that millions of JavaScript developers know today, TypeScript allows developers to use existing JavaScript code, incorporate popular JavaScript libraries, and call TypeScript code from JavaScript. TypeScript's optional static types enable JavaScript developers to use highly-productive development tools and practices like static checking and code refactoring when developing JavaScript applications. Going forward, we will continue to work with our partners and the community to evolve TypeScript’s type system to allow users to further express JavaScript in a statically typed fashion. In addition, we will focus on enhancing the TypeScript language service and set of tooling features so that developer tools become smarter and further boost developer productivity. To each and every one of you who has been a part of the journey to 2.0: thank you! Your feedback and enthusiasm have brought the TypeScript language and ecosystem to where it is today. We hope you're as excited for 2.0 and beyond as we are. If you still haven't used TypeScript, 

[give it a try][24]! We'd love to hear from you. Happy hacking! The TypeScript Team

 [1]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript
 [2]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS2.0.3-TS-release20-nightly-20160921.1/TypeScript_Dev14Full.exe
 [3]: https://msdn.microsoft.com/en-us/library/mt752379.aspx
 [4]: https://code.visualstudio.com/docs/languages/typescript#_using-newer-typescript-versions
 [5]: https://blogs.msdn.microsoft.com/typescript/2014/10/06/announcing-typescript-1-1-ctp/
 [6]: https://blogs.msdn.microsoft.com/typescript/2014/07/21/new-compiler-and-moving-to-github/
 [7]: https://blogs.msdn.microsoft.com/typescript/2015/01/16/announcing-typescript-1-4/
 [8]: https://angular.io/
 [9]: https://blogs.msdn.microsoft.com/typescript/2015/03/05/angular-2-built-on-typescript/
 [10]: https://blogs.msdn.microsoft.com/typescript/2015/09/16/announcing-typescript-1-6/
 [11]: https://blogs.msdn.microsoft.com/typescript/2015/11/30/announcing-typescript-1-7/
 [12]: https://blogs.msdn.microsoft.com/typescript/2016/02/22/announcing-typescript-1-8-2/
 [13]: https://github.com/DefinitelyTyped/DefinitelyTyped/
 [14]: https://blogs.msdn.microsoft.com/typescript/2016/07/11/announcing-typescript-2-0-beta/
 [15]: https://blogs.msdn.microsoft.com/typescript/2016/08/30/announcing-typescript-2-0-rc/
 [16]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#tagged-union-types
 [17]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#the-never-type
 [18]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#specifying-the-type-of-this-for-functions
 [19]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#glob-support-in-tsconfigjson
 [20]: https://docs.npmjs.com/misc/scope
 [21]: https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare
 [22]: https://github.com/Microsoft/TypeScript/pull/7140
 [23]: https://github.com/Microsoft/TypeScript/pull/8010
 [24]: http://www.typescriptlang.org/docs/tutorial.html