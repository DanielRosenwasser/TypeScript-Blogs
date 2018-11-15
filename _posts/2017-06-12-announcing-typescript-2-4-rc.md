---
ID: 1076
post_title: Announcing TypeScript 2.4 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-4-rc/
published: true
post_date: 2017-06-12 19:33:22
---
Today we're releasing our RC of TypeScript 2.4. To get started with the latest stable version of TypeScript, you can grab it [through NuGet][1], or use the following command with npm: <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@rc</pre>
</div> Visual Studio 2015 users (who have 

[Update 3][2]) [can install TypeScript from here][3], and Visual Studio 2017 users using Update 2 will be able to get TypeScript [by simply installing it from here][4]. To get it working elsewhere, you can easily configure [Visual Studio Code][5] and [our Sublime Text plugin][6] to pick up whatever version you need. [Other editors][7] may have different approaches to swapping TypeScript in. 
## <a href="https://gist.github.com/DanielRosenwasser/f5e7cbee0241b8edc1830233b160b46c#dynamic-import-expressions" id="user-content-dynamic-import-expressions" class="anchor"></a>Dynamic Import Expressions TypeScript 2.4 is bringing support for ECMAScript's new 

`import()` calls. These calls import a module and return a promise to that module. For example, imagine a webpage that allows you to create and edit images. When you're working with one file, the page will allow you to download that file immediately; but if you're working on multiple images, you can save all of them as a `.zip` file. You might have a utility library to create a zip file, but since downloading multiple files isn't that common, you'd want to load that functionality lazily. `import()` expressions let you load a module on the fly as a `Promise` like so: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> getZipFile(<span class="pl-v">name</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>, <span class="pl-v">files</span><span class="pl-k">:</span> <span style="color: #267f99">File</span>[])<span class="pl-k">:</span> <span style="color: #267f99">Promise</span>&lt;<span style="color: #267f99">File</span>&gt; {
    <span style="color: #0000ff">const</span> zipUtil <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">'</span>./utils/create-zip-file<span class="pl-pds">'</span></span>);
    <span style="color: #0000ff">const</span> zipContents <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span class="pl-smi">zipUtil</span>.<span class="pl-en">getContentAsBlob</span>(<span class="pl-smi">files</span>);
    <span style="color: #0000ff">return</span> <span style="color: #0000ff">new</span> <span style="color: #267f99">File</span>(<span class="pl-smi">zipContents</span>, <span class="pl-smi">name</span>);
}</pre>
</div> This feature is so incredibly useful in situations where you want to conditionally import modules. Projects that use bundlers like Webpack can operate on these 

`import()` calls and split code into smaller bundles that can be lazily loaded. What all of this means in the end is 
*   you can send less JS over the wire for more common scenarios
*   your users can get faster page load times for critical content

## <a href="https://gist.github.com/DanielRosenwasser/f5e7cbee0241b8edc1830233b160b46c#safer-callback-parameter-checking" id="user-content-safer-callback-parameter-checking" class="anchor"></a>Safer callback parameter checking

<span>When checking whether two functions are assignable to one another, TypeScript checks whether their parameters are </span>*bidirectionally*<span> assignable. We call this </span>*function parameter bivariance*<span>. There are a number of reasons for this, but </span>[it mainly stems from TypeScript's structural nature][8]<span> and how we've tried to match people's intuition.</span> Our experience has been that users generally don't run into issues with this all that much; however, we did start to see that this model broke down on containers which handed internal data off through callbacks - specifically, Promises. For example, a `Promise<Animal>` was assignable to `Promise<Dog>`, which is incorrect. You can see similar behavior in the following sample: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267f99">Animal</span> { animalStuff<span class="pl-k">:</span> <span style="color: #0000ff">any</span> }
<span style="color: #0000ff">interface</span> <span style="color: #267f99">Dog</span> <span style="color: #0000ff">extends</span> <span style="color: #267f99">Animal</span> { bark()<span class="pl-k">:</span> <span style="color: #0000ff">void</span> }

<span style="color: #0000ff">interface</span> <span style="color: #267f99">BasicCollection</span>&lt;<span style="color: #267f99">T</span>&gt; {
    forEach(<span class="pl-en">callback</span><span class="pl-k">:</span> (<span class="pl-v">value</span><span class="pl-k">:</span> <span style="color: #267f99">T</span>) <span class="pl-k">=&gt;</span> <span style="color: #0000ff">void</span>)<span class="pl-k">:</span> <span style="color: #0000ff">void</span>;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> animalCollection<span class="pl-k">:</span> <span style="color: #267f99">BasicCollection</span>&lt;<span style="color: #267f99">Animal</span>&gt;;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> dogCollection<span class="pl-k">:</span> <span style="color: #267f99">BasicCollection</span>&lt;<span style="color: #267f99">Dog</span>&gt;;

<span style="color: #148a14">// This should be an error, but TypeScript 2.3 and below allow it.</span>
<span class="pl-smi">dogCollection</span> <span class="pl-k">=</span> <span class="pl-smi">animalCollection</span>;</pre>
</div> To solve this issue, TypeScript 2.4 now tightens things up and compares the parameters which are callbacks specially. When checking callbacks, TypeScript will be strict about checking parameters contravariantly with respect to the current check. Parameter bivariance still applies otherwise, but we found this to be an effective safety check without drastically changing the type system. Since this may be a bit technical, you can read more 

[on the original pull request][9]. The short story is that you'll see much better checking when using Promises, Observables, and anywhere else you've been using callbacks. Be aware though - this new check can introduce new type-checking errors in existing codebases. For more details, [see our Breaking Changes section][10]. 
## <a href="https://gist.github.com/DanielRosenwasser/f5e7cbee0241b8edc1830233b160b46c#weak-types" id="user-content-weak-types" class="anchor"></a>Weak types Back in TypeScript 1.6, we added 

[a check for excess properties in object literals][11]. This check looked for unexpected properties in object literals, and it happened to catch a large class of bugs. The only shortcoming of the check was that if you didn't immediately pass your object literal to something of the appropriate type, the check wouldn't be triggered. In TypeScript 2.4, we're adding a similar check for what we call *weak types*. Any type that contains only optional properties is considered a *weak* type since it provides few restrictions on what can be assigned to it. For example, this `Options` type is a weak type: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267f99">Options</span> {
    data<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>,
    timeout<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>,
    maxRetries<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>,
}</pre>
</div> In TypeScript 2.4, it's now an error to assign anything to a weak type when there's no overlap in properties. For example: 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> sendMessage(<span class="pl-v">options</span><span class="pl-k">:</span> <span style="color: #267f99">Options</span>) {
    <span style="color: #148a14">// ...</span>
}

<span style="color: #0000ff">const</span> opts <span class="pl-k">=</span> {
    payload: <span style="color: #a31515"><span class="pl-pds">"</span>hello world!<span class="pl-pds">"</span></span>,
    retryOnFail: <span style="color: #0000ff">true</span>,
}

<span style="color: #148a14">// Error!</span>
<span class="pl-en">sendMessage</span>(<span class="pl-smi">opts</span>);
<span style="color: #148a14">// No overlap between the type of 'opts' and 'Options' itself.</span>
<span style="color: #148a14">// Maybe we meant to use 'data'/'maxRetries' instead of 'payload'/'retryOnFail'.</span></pre>
</div> You can think of this as TypeScript "toughening up" the weak guarantees of these types to catch what would otherwise be silent bugs. Since this is a breaking change, you may need to know about the workarounds which are the same as those for strict object literal checks: 

1.  Declare the properties if they really do exist.
2.  Add an index signature to the weak type (i.e. `[propName: string]: {}`).
3.  Use a type assertion (i.e. `opts as Options`).

## <a href="https://gist.github.com/DanielRosenwasser/f5e7cbee0241b8edc1830233b160b46c#string-enums" id="user-content-string-enums" class="anchor"></a>String enums Enums were an early feature TypeScript provided that have been handy for marking a set of well-known related numeric values. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">enum</span> <span style="color: #267f99">E</span> {
    <span class="pl-smi">A</span> <span class="pl-k">=</span> <span style="color: #09885A">1</span>,
    <span class="pl-smi">B</span> <span class="pl-k">=</span> <span style="color: #09885A">2</span>,
    <span class="pl-smi">C</span> <span class="pl-k">=</span> <span style="color: #09885A">3</span>,
}</pre>
</div> In TypeScript 1.8, we also released string literal types which could be used in a union. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> setStatus(<span class="pl-v">status</span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>ready<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>running<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>finished<span class="pl-pds">"</span></span>) {
    <span style="color: #148a14">// ...</span>
}

<span class="pl-en">setStatus</span>(<span style="color: #a31515"><span class="pl-pds">"</span>ready<span class="pl-pds">"</span></span>);

<span class="pl-en">setStatus</span>(<span style="color: #a31515"><span class="pl-pds">"</span>stopped<span class="pl-pds">"</span></span>);
<span style="color: #148a14">//        ~~~~~~~~~</span>
<span style="color: #148a14">// Error: Type</span>
<span style="color: #148a14">//</span>
<span style="color: #148a14">//   '"stopped"'</span>
<span style="color: #148a14">//</span>
<span style="color: #148a14">// is not assignable to type</span>
<span style="color: #148a14">//</span>
<span style="color: #148a14">//   '"ready" | "running" | "finished"'.</span></pre>
</div> There were benefits to both constructs, but each group of users wanted 

*some* functionality from the other. For example, string literal types serialize very well over transfer protocols and during debugging - after all, they're just strings. But this isn't the case for numeric enums, which end up as plain numbers without obvious meaning. But string literal unions can be cumbersome for more advanced scenarios. If you want a well-known set of constants for each string, then you typically have to declare them yourself. That's why for TypeScript 2.4, we're introducing string enums! <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">enum</span> <span style="color: #267f99">ActionType</span> {
    <span class="pl-smi">AddUser</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>ADD_USER<span class="pl-pds">"</span></span>,
    <span class="pl-smi">DeleteUser</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>DELETE_USER<span class="pl-pds">"</span></span>,
    <span class="pl-smi">RenameUser</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>RENAME_USER<span class="pl-pds">"</span></span>,

    <span style="color: #148a14">// Aliases</span>
    <span class="pl-smi">RemoveUser</span> <span class="pl-k">=</span> <span class="pl-smi">DeleteUser</span>,
}</pre>
</div> String enums have a simple representation, but come with the caveat that they don't create a reverse-mapping. In other words, you can't index into 

`ActionType` with the string `"ADD_USER"` (by writing `ActionType["ADD_USER"]`) to get the name of `AddUser`. 
## <a href="https://gist.github.com/DanielRosenwasser/f5e7cbee0241b8edc1830233b160b46c#have-fun" id="user-content-have-fun" class="anchor"></a>Have fun! You can see 

[the full list of what's new in 2.4 here on our wiki][12]. Then give TypeScript 2.4 RC a try and let us know what you think! If you're having an awesome experience, use the [#iHeartTypeScript][13] hashtag on Twitter and let us know. If you run into any places you think we could improve, we're [always all-ears on suggestions][14].

 [1]: https://www.nuget.org/packages/Microsoft.TypeScript.Compile
 [2]: https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs
 [3]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.4.0-TS-release-dev14update3-20170608.3/TypeScript_Dev14Full.exe
 [4]: http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.4.0-TS-release-dev14update3-20170608.3/TypeScript_SDK.exe
 [5]: https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions
 [6]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [7]: https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support
 [8]: https://github.com/Microsoft/TypeScript/wiki/FAQ#why-are-function-parameters-bivariant
 [9]: https://github.com/Microsoft/TypeScript/pull/15104
 [10]: https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes#stricter-variance-in-callback-parameters
 [11]: https://www.typescriptlang.org/docs/handbook/interfaces.html#excess-property-checks
 [12]: https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript
 [13]: https://twitter.com/intent/tweet?text=%23iHeartTypeScript
 [14]: https://github.com/Microsoft/TypeScript/issues/new