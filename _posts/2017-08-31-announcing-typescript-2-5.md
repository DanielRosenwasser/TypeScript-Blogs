---
ID: 1145
post_title: Announcing TypeScript 2.5
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-5/
published: true
post_date: 2017-08-31 16:44:32
---
<div style="font-size: 16px">
  Today we're happy to bring you TypeScript 2.5! If you've read<span> </span><a href="https://blogs.msdn.microsoft.com/typescript/2017/08/17/announcing-typescript-2-5-rc/">our RC announcement</a>, we've got a few new items that we're proud to highlight! If you're not familiar with TypeScript, it's a typed superset of JavaScript. More simply put, it's just JavaScript with optional static types. Static types can make it easier to maintain your code by catching bugs early on, making it easier to navigate your projects, giving accurate code completion, and providing handy fixes for when you do make mistakes. By making types optional, you can get the flexibility of plain JavaScript when you need it, all while TypeScript also gives you the option to tighten things up to bring you more type safety. You can<span> </span><a href="http://www.typescriptlang.org/">learn more about TypeScript on our website</a>. To start using TypeScript, you can grab it<span> </span><a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild">through NuGet</a><span> </span>or use the following command with npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> Visual Studio 2015 users (who have
  
  <span> </span><a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>)<span> </span><a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.5.2-TS-release-dev14update3-20170830.5/TypeScript_Dev14Full.exe">can install TypeScript 2.5 from here</a>, and Visual Studio 2017 users using<span> </span><a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes-v15.2">version 15.2 or later</a><span> </span>will be able to get TypeScript<span> </span><a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.5.2-TS-release-dev14update3-20170830.5/TypeScript_SDK.exe">by simply installing it from here</a>. Visual Studio 2017 users should be sure to <a href="https://github.com/Microsoft/TypeScript/wiki/Updating-TypeScript-in-Visual-Studio-2017">read up on how you can configure your project to target specific versions of TypeScript</a>. While TypeScript 2.5 will be available for other editors soon, in the meantime you can configure<span> </span><a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a><span> </span>and<span> </span><a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a><span> </span>to use a newer version.<span> </span><a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a><span> </span>may have different approaches to getting TypeScript 2.5 running. Let's look at what TypeScript 2.5 brings! <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#the-extract-function-and-extract-method-refactorings" id="user-content-the-extract-function-and-extract-method-refactorings" class="anchor"></a>The<span> </span><em>Extract Function</em><span> </span>and<span> </span><em>Extract Method</em><span> </span>refactorings
  </h2> Our team is always in search of ways to bring more powerful tools to the TypeScript and JavaScript world. That's why with TypeScript 2.5 we've invested a lot into implementing
  
  <span> </span><em>extract method</em><span> </span>and<span> </span><em>extract function</em>: two new refactorings that make complex rewrites trivial. <video width="100%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/08/boxBlur3.1.mp4"> A series of extract function refactorings applied to a large unwieldy function with four nested for-loops. </video> If you're using Visual Studio Code, this refactoring will be available in the upcoming release (though you can try it now by<span> </span><a href="https://code.visualstudio.com/insiders">using VS Code Insiders releases</a>). This feature is still fairly new so we expect that there will still be room for improvement, but we're excited to hear your feedback so we can polish things out. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#new-quick-fixes" id="user-content-new-quick-fixes" class="anchor"></a>New quick fixes
  </h2> We've also added a few quick fixes for when TypeScript can guess a little bit at what you meant to write. One new quick fix will get triggered when you try to use JSDoc-style types in TypeScript. If you're in the habit of writing types like these, you might be surprised to find out that they're not valid in TypeScript, but TypeScript is happy to push us in the right direction. 
  
  <img src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/08/accidentalJsdoc.gif" alt="Quick fixes correcting JSDoc-style types to TypeScript-style types." /> We've also added a quick fix for when you try to reference the type of a property off of another type incorrectly. For example, for the following code <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Foo</span> {
    bar<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}</pre>
  </div> We might want to declare a variable named
  
  <span> </span><code style="color: #a31515">xyz</code><span> </span>whose type is tied to to the type of<span> </span><code style="color: #a31515">bar</code>. The correct way to write this would be using an indexed access type: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Get the type of the property named 'bar' off of 'Foo'.</span>
<span style="color: #0000ff">var</span> xyz<span class="pl-k">:</span> <span style="color: #267F99">Foo</span>[<span style="color: #a31515"><span class="pl-pds">"</span>bar<span class="pl-pds">"</span></span>]</pre>
  </div> but we might accidentally write
  
  <span> </span><code style="color: #a31515">var xyz: Foo.bar</code>. TypeScript now can now suggest the correct one in many cases. <img src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/08/propReference.gif" alt="A quick fix that corrects `Foo.bar` to `Foo['bar']`." /> <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#jsdoc-type-assertion-support-in-javascript-files" id="user-content-jsdoc-type-assertion-support-in-javascript-files" class="anchor"></a>JSDoc type assertion support in JavaScript files
  </h2> In TypeScript 2.4, we introduced the ability to get type-checking in JavaScript files so that users can more easily migrate to TypeScript, and have an easier experience with certain more lightweight projects. Taking advantage of this is as simple as adding a
  
  <span> </span><code style="color: #a31515">// @ts-check</code><span> </span>at the top of your<span> </span><code style="color: #a31515">.js</code><span> </span>file, or turning on the<span> </span><code style="color: #a31515">checkJs</code><span> </span>flag in yout<span> </span><code style="color: #a31515">tsconfig.json</code>'s<span> </span><code style="color: #a31515">compilerOptions</code>. One thing that it lacked was the ability to "cast" or "assert" the type of an expression. This is important for situations where you know a little more than the type-checker and need to tell it so. To come up with a trivial example, let's take the following JavaScript code: <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// @ts-check</span>

<span style="color: #0000ff">var</span> foo <span class="pl-k">=</span> <span>Math</span>.<span>random</span>() <span class="pl-k">?</span> <span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span> <span class="pl-k">:</span> <span style="color: #09885A">100</span>;

<span class="pl-smi">foo</span>.<span>toUpperCase</span>();
<span style="color: #148A14">//  ~~~~~~~~~~~</span>
<span style="color: #148A14">//  Error! Property 'toUpperCase' does not exist on type 'string | number'.</span></pre>
  </div> TypeScript correctly indicates that we might be calling a method that doesn't exist on
  
  <span> </span><code style="color: #a31515">number</code>s. If we wanted to get around this so we can easily get our runtime error, we could write a JSDoc type assertion: <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// Works!</span>
<span style="color: #0000ff">var</span> bar <span class="pl-k">=</span> <span style="color: #148A14">/** <span class="pl-k">@type</span> <span class="pl-en">{string}</span> */</span> (foo);
<span class="pl-smi">bar</span>.<span>toUpperCase</span>();</pre>
  </div> The syntax is
  
  <span> </span><code style="color: #a31515">/** @type {YOUR_TYPE_HERE} */ (someParenthesizedExpression)</code>. Keep in mind that if you've enabled JavaScript checking on a file, invalid type assertions will still get caught: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">var</span> clearlyNumber <span class="pl-k">=</span> <span style="color: #148A14">/** <span class="pl-k">@type</span> <span class="pl-en">{string}</span> */</span> (<span style="color: #09885A">100</span>);
<span style="color: #148A14">//                      ~~~~~~~~~~~~~~</span>
<span style="color: #148A14">// Error! Type 'number' cannot be converted to type 'string'.</span></pre>
  </div>
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#optional-catch-clauses" id="user-content-optional-catch-clauses" class="anchor"></a>Optional<span> </span><code style="color: #a31515;font-size: 29px">catch</code><span> </span>clauses
  </h2> Thanks to work by
  
  <span> </span><a href="https://github.com/tinganho">Tingan Ho</a>, TypeScript 2.5 brings a new ECMAScript-bound feature for making catch clauses optional. Much of the time, you'll find yourself writing a<span> </span><code style="color: #a31515">try</code>/<code style="color: #a31515">catch</code><span> </span>but not really caring about the thrown error. For example: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> contents;
<span style="color: #0000ff">try</span> {
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-smi">fs</span>.<span class="pl-en">readFileSync</span>(<span style="color: #a31515"><span class="pl-pds">"</span>.config_file<span class="pl-pds">"</span></span>).<span>toString</span>(<span style="color: #a31515"><span class="pl-pds">'</span>utf8<span class="pl-pds">'</span></span>);
}
<span style="color: #0000ff">catch</span> (<span class="pl-smi">unusedError</span>) {
    <span style="color: #148A14">// File might not exist, just fall back to some defaults.</span>
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-en">createDefaultContents</span>();
}</pre>
  </div> Notice that
  
  <span> </span><code style="color: #a31515">unusedError</code><span> </span>is never referenced in the above example. Barring philosophical issues about whether it's appropriate to ignore the error, we can make our code a little cleaner by taking advantage of the fact that the<span> </span><code style="color: #a31515">catch</code><span> </span>variable is now optional. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> contents;
<span style="color: #0000ff">try</span> {
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-smi">fs</span>.<span class="pl-en">readFileSync</span>(<span style="color: #a31515"><span class="pl-pds">"</span>.config_file<span class="pl-pds">"</span></span>).<span>toString</span>(<span style="color: #a31515"><span class="pl-pds">'</span>utf8<span class="pl-pds">'</span></span>);
}
<span style="color: #0000ff">catch</span> {
    <span style="color: #148A14">// File might not exist, just fall back to some defaults.</span>
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-en">createDefaultContents</span>();
}</pre>
  </div>
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#deduplicated-and-redirected-packages" id="user-content-deduplicated-and-redirected-packages" class="anchor"></a>Deduplicated and redirected packages
  </h2> When importing using the
  
  <span> </span><code style="color: #a31515">Node</code><span> </span>module resolution strategy in TypeScript 2.5, the compiler will now check whether files originate from "identical" packages. If a file originates from a package with a<span> </span><code style="color: #a31515">package.json</code><span> </span>containing the same<span> </span><code style="color: #a31515">name</code><span> </span>and<span> </span><code style="color: #a31515">version</code><span> </span>fields as a previously encountered package, then TypeScript will redirect itself to the top-most package. This helps resolve problems where two packages might contain identical declarations of classes, but which contain<span> </span><code style="color: #a31515">private</code><span> </span>members that cause them to be structurally incompatible. As a nice bonus, this can also reduce the memory and runtime footprint of the compiler and language service by avoiding loading<span> </span><code style="color: #a31515">.d.ts</code><span> </span>files from duplicate packages. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#the---preservesymlinks-compiler-flag" id="user-content-the---preservesymlinks-compiler-flag" class="anchor"></a>The<span> </span><code style="color: #a31515;font-size: 29px">--preserveSymlinks</code><span> </span>compiler flag
  </h2> TypeScript 2.5 brings the
  
  <span> </span><code style="color: #a31515">preserveSymlinks</code><span> </span>flag, which parallels the behavior of<span> </span><a href="https://nodejs.org/api/cli.html#cli_preserve_symlinks">the<span> </span><code style="color: #a31515">--preserve-symlinks</code><span> </span>flag in Node.js</a>. This flag also exhibits the opposite behavior to Webpack's<span> </span><code style="color: #a31515">resolve.symlinks</code><span> </span>option (i.e. setting TypeScript's<span> </span><code style="color: #a31515">preserveSymlinks</code><span> </span>to<span> </span><code style="color: #a31515">true</code><span> </span>parallels setting Webpack's<span> </span><code style="color: #a31515">resolve.symlinks</code><span> </span>to<span> </span><code style="color: #a31515">false</code>, and vice-versa). In this mode, references to modules and packages (e.g.<span> </span><code style="color: #a31515">import</code>s and<span> </span><code style="color: #a31515">/// &lt;reference type="..." /&gt;</code><span> </span>directives) are all resolved relative to the location of the symbolic link file, rather than relative to the path that the symbolic link resolves to. For a more concrete example, we'll defer to<span> </span><a href="https://nodejs.org/api/cli.html#cli_preserve_symlinks">the documentation on the Node.js website</a>. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/4f13fd3d6ad448c05fb25ab2d250d6cf#enjoy" id="user-content-enjoy" class="anchor"></a>Enjoy!
  </h2> We hope our work in TypeScript 2.5 will make you happier and more productive. If it did, let us know on Twitter with the
  
  <span> </span><a href="https://twitter.com/intent/tweet?text=%23iHeartTypeScript">#iHeartTypeScript</a><span> </span>hashtag. Out team appreciates any sort of feedback on how we can improve. Feel free to let us know about any issues you run into or helpful ideas you think might make TypeScript even better to use<span> </span><a href="https://github.com/Microsoft/TypeScript/issues">on our GitHub issue tracker</a>. As for what's next, we've caught most of what's new on this blog post, but you can always check out our<span> </span><a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">what's new in TypeScript</a><span> </span>page on our wiki for some more details, and keep an eye on<span> </span><a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our Roadmap</a><span> </span>that highlights our current plans and is frequently updated. Thanks for reading and happy hacking!
</div>