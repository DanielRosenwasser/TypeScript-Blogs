---
ID: 1197
post_title: Announcing TypeScript 2.6 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-6-rc/
published: true
post_date: 2017-10-12 19:01:18
---
<div style="font-size: 16px">
</div>TypeScript 2.6 RC is now available! To get started with the latest stable version of TypeScript, you can grab it 

[through NuGet][1], or use the following command with npm: <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@rc</pre>
</div> Visual Studio 2015 users (who have 

[Update 3][2]) can [install TypeScript 2.6 RC from here][3], and Visual Studio 2017 users using version 15.2 or later will be able to get TypeScript [by simply installing it from here][4]. You can get TypeScript 2.6 RC working with [Visual Studio Code][5] and [Sublime Text][6], though the full release of TypeScript 2.6 will be available out of the box. Let's take a look at what TypeScript 2.6 will bring! 
## Contravariant function parameters with <code style="color: #a31515;font-size: 29px">strictFunctionTypes</code> With TypeScript 2.6, we're introducing a new 

<code style="color: #a31515">--strict</code> mode flag: <code style="color: #a31515">--strictFunctionTypes</code>. TypeScript has traditionally compared parameters in a bivariant manner. There are a number of reasons for this - modeling event handlers in the DOM, allowing a simpler model for working with arrays, and more. [In TypeScript 2.4][7], we tightened these checks a bit, but a few problems still remained. For example, the following would compile with no errors: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> makeLowerCase(<span class="pl-v">s</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">s</span>.<span class="pl-c1">toLowerCase</span>();
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> foo<span class="pl-k">:</span> <span style="color: #267F99">Promise</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;;
<span class="pl-smi">foo</span>.<span class="pl-en">then</span>(<span class="pl-smi">makeLowerCase</span>); <span style="color: #148A14">// Whoops!</span></pre>
</div> While this is a very basic example, we re-evaluated our design decisions based on how JavaScript is written today and will be in the coming years, and felt that there were improvements we could make while accomodating the patterns described above. Under 

<code style="color: #a31515">--strictFunctionTypes</code>, any function type that doesn't originate from a method has its parameters compared *contravariantly*. That means that the above code will fail, because when we try to pass <code style="color: #a31515">makeLowerCase</code> with the type <code style="color: #a31515">(s: string) =&gt; string</code> to <code style="color: #a31515">then</code> which expects a function of type <code style="color: #a31515">(onfulfilled: string | number) =&gt; ...</code>, we'll flip directions and try to assess whether <code style="color: #a31515">string | number</code> is assignable to <code style="color: #a31515">string</code> which it isn't. The compromise of increasing strictness on all function types but methods also allows TypeScript to continue modeling the above use-cases (e.g. event handlers and simpler array handling). [Covariance and contravariance][8] probably deserve a a more thorough explanation. If you'd like to read a bit more, some of us on the team felt that [this article][9] gives a reasonable high-level explanation. You can also read up more on [the original pull request][10]. But the short version is that with <code style="color: #a31515">--strictFunctionTypes</code>, you'll be able to catch many common mistakes. Keep in mind that while <code style="color: #a31515">--strictFunctionTypes</code> is opt-in, it will automatically be turned on if you're operating with the <code style="color: #a31515">--strict</code> flag on. This may introduce some breaks, so to disable the check with <code style="color: #a31515">--strict</code> on, you can specify <code style="color: #a31515">--strictFunctionTypes false</code> on the command line or in your <code style="color: #a31515">tsconfig.json</code> as so: <div class="highlight highlight-source-json">
  <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>strict<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>strictFunctionTypes<span class="pl-pds">"</span></span>: <span style="color: #0000ff">false</span>
    }
}</pre>
</div>

## Error suppression comments with <code style="color: #a31515;font-size: 29px">// @ts-ignore</code> Historically, we've avoided error suppression within TypeScript because most cases where our users asked for it could be solved through more accurate declaration files, or using a type assertion to 

<code style="color: #a31515">any</code>. However, over time, we have seen two motivating examples: migrating from JavaScript to TypeScript, and overcoming type-checks that live in legacy code. For the former, the benefits are obvious: you want to start moving over to TypeScript, but you've run into a pattern that is particularly difficult to model. You could spend time trying to understand it, but maybe you'd rather get rid of it later on anyway, so you just want to avoid errors because your code will still run just fine. The latter situation is a bit less obvious. The motivation here was that within some large organizations, dependencies across projects are updated in tandem, including TypeScript itself and <code style="color: #a31515">@types</code> declaration files from Definitely Typed. If any change introduces a type-checking error, someone has to fix that to avoid breaking the build. But now a question arises: who should that be on an unmaintained project? While the error is usually useful, the reality of the situation is that the code has functioned thus far and teams have finite resources. That's why with TypeScript 2.6 we're bringing <code style="color: #a31515">// @ts-ignore</code> comments to TypeScript files. These comments suppress any errors that occur on the next line. For example, in the following code, TypeScript would ordinarily report an error about the <code style="color: #a31515">console.log</code> statement being unreachable. In TypeScript 2.6, the <code style="color: #a31515">// @ts-ignore</code> comment squelches the error entirely. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">if</span> (<span style="color: #0000ff">false</span>) {
    <span style="color: #148A14">// @ts-ignore: Unreachable code error</span>
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>);
}</pre>
</div> While this feature is motivated by pragmatism, we encourage taking type-checking errors seriously. Our guidance here is to use suppression comments 

*very sparingly*. In situations where you *do* need to use these comments, we suggest leaving a trailing explanation of why the comment is necessary like in the example above. 
## Revised tagged template emit Tagged template strings have been supported and type-checked in TypeScript for quite some time now. Let's take a simple example of a tagged template string: 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> foo(<span class="pl-v">arg</span><span class="pl-k">:</span> <span style="color: #267F99">TemplateStringsArray</span>) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">arg</span>;
}

<span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">return</span> <span class="pl-smi">foo</span> <span style="color: #a31515"><span class="pl-pds">`</span>hello world<span class="pl-pds">`</span></span>;
}</pre>
</div> When emitting to ES3/ES5, that tagged template more or less boils down to calling 

<code style="color: #a31515">foo</code> like so: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> foo(arg) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">arg</span>;
}
<span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">var</span> _a <span class="pl-k">=</span> [<span style="color: #a31515"><span class="pl-pds">"</span>hello world<span class="pl-pds">"</span></span>];
    <span class="pl-smi">_a</span>.<span class="pl-smi">raw</span> <span class="pl-k">=</span> [<span style="color: #a31515"><span class="pl-pds">"</span>hello world<span class="pl-pds">"</span></span>];
    <span class="pl-en">foo</span>(<span class="pl-smi">_a</span>);
}</pre>
</div> However, the ECMAScript specification says that every time 

<code style="color: #a31515">foo `hello world`</code> gets invoked, <code style="color: #a31515">foo</code> will get the same template object, so re-initializing <code style="color: #a31515">_a</code> in <code style="color: #a31515">bar</code> is incorrect. This is problematic because some libraries have an expectation that with the above example, any invocation of <code style="color: #a31515">bar()</code> will return the same value. In TypeScript 2.6, within a module, tagged template string objects are now cached after the first invocation. This aligns more closely with recent revisions to the ECMAScript spec, and we will be following up with this behavior to ensure it correctly works within global code as well. This also means that TypeScript users can use libraries like [lit-html][11] and [hyperHTML][12] which take advantage of the new behavior. 
## Breaking changes and deprecations There are several minor changes that may impact your codebase. While you can read up more about them in our 

[Breaking Changes][13] section, please keep these in mind when upgrading. 
*   Write only references are now considered unused under <code style="color: #a31515">--noUnusedLocals</code> and <code style="color: #a31515">--noUnusedParameters</code>.
*   In ambient contexts (e.g. declaration files, and <code style="color: #a31515">declare module</code> blocks), expressions are now disallowed in <code style="color: #a31515">default</code>exports.
*   Uninhabitable types resulting from intersections (<code style="color: #a31515">number & string</code>, <code style="color: #a31515">"foo" & 42</code>, etc.) will simplify to <code style="color: #a31515">never</code> when placed in a union.
*   Various changes have been made to DOM declarations in <code style="color: #a31515">lib.d.ts</code>. As for deprecations, the 

<code style="color: #a31515">getSymbolDisplayBuilder</code> API is now deprecated. For simple use cases, you should be able to move to <code style="color: #a31515">TypeChecker#symbolToString</code>. For more advanced use cases, we plan to bring <code style="color: #a31515">symbolToString</code> fully up to par with <code style="color: #a31515">SymbolDisplayBuilder</code> in functionality by TypeScript 2.7. In a release following TypeScript 2.7, we will be be removing <code style="color: #a31515">getSymbolDisplayBuilder</code>. 
## Other features and upcoming plans We have a more complete description of new features in our 

[What's New in TypeScript][14] wiki page, and try to keep our [Roadmap][15] up to date to provide users with an idea of what's in store. For the full release, we anticipate we'll have some more tooling-related features that you can take advantage of. Thanks for reading up on what we've got in store! We hope you enjoy this RC, and we'd love any and all feedback to ensure TypeScript 2.6 is a wonderful release.

 [1]: https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild
 [2]: https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs
 [3]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.6.0-TS-release-dev14update3-20171007.2/TypeScript_Dev14Full.exe
 [4]: http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.6.0-TS-release-dev14update3-20171007.2/TypeScript_SDK.exe
 [5]: https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions
 [6]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [7]: http://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-4.html#strict-contravariance-for-callback-parameters
 [8]: https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)
 [9]: https://www.stephanboyer.com/post/132/what-are-covariance-and-contravariance
 [10]: https://github.com/Microsoft/TypeScript/pull/18654
 [11]: https://github.com/PolymerLabs/lit-html
 [12]: https://github.com/WebReflection/hyperHTML
 [13]: https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes
 [14]: https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript
 [15]: https://github.com/Microsoft/TypeScript/wiki/Roadmap