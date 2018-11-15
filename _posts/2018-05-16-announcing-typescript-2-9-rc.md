---
ID: 1455
post_title: Announcing TypeScript 2.9 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-9-rc/
published: true
post_date: 2018-05-16 20:34:56
---
Today we're excited to announce and get some early feedback with TypeScript 2.9's Release Candidate. To get started with the RC, you can access it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a>, or use npm with the following command: <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@rc</pre>
</div> You can also get editor support by 

*   <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.9.0-TS-release-dev14update3-20180514.1/TypeScript_Dev14Full.exe" rel="nofollow">Downloading for Visual Studio 2015</a> (with <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs" rel="nofollow">Update 3</a>)
*   <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.9.0-TS-release-dev14update3-20180514.1/TypeScript_SDK.exe" rel="nofollow">Downloading for Visual Studio 2017</a> (for version 15.2 or later)
*   Following directions for <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">Visual Studio Code</a> and [Sublime Text][1]. Let's jump into some highlights of the Release Candidate! 

## Support for symbols and numeric literals in <code style="color: #a31515;font-size: 29px">keyof</code> and mapped object types TypeScript's 

<code style="color: #a31515">keyof</code> operator is a useful way to query the property names of an existing type. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    age<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #148A14">// Equivalent to the type</span>
<span style="color: #148A14">//  "name" | "age"</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">PersonPropertiesNames</span> <span class="pl-k">=</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">Person</span>;</pre>
</div> Unfortunately, because 

<code style="color: #a31515">keyof</code> predates TypeScript's ability to reason about <code style="color: #a31515">unique symbol</code> types, <code style="color: #a31515">keyof</code> never recognized symbolic keys. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">const</span> baz <span class="pl-k">=</span> <span class="pl-c1">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>baz<span class="pl-pds">"</span></span>);

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Thing</span> {
    foo<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    bar<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    [<span class="pl-smi">baz</span>]<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>; <span style="color: #148A14">// this is a computed property type</span>
}

<span style="color: #148A14">// Error in TypeScript 2.8 and earlier!</span>
<span style="color: #148A14">// `typeof baz` isn't assignable to `"foo" | "bar"`</span>
<span style="color: #0000ff">let</span> x<span class="pl-k">:</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">Thing</span> <span class="pl-k">=</span> <span class="pl-smi">baz</span>;</pre>
</div> TypeScript 2.9 changes the behavior of 

<code style="color: #a31515">keyof</code> to factor in both unique symbols as well as numeric literal types. As such, the above example now compiles as expected. <code style="color: #a31515">keyof Thing</code> now boils down to the type <code style="color: #a31515">"foo" | "bar" | typeof baz</code>. With this functionality, mapped object types like <code style="color: #a31515">Partial</code>, <code style="color: #a31515">Required</code>, or <code style="color: #a31515">Readonly</code> also recognize symbolic and numeric property keys, and no longer drop properties named by symbols: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Partial</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">K</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">K</span>]
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Thing</span> {
    foo<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    bar<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    [<span class="pl-smi">baz</span>]<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">PartialThing</span> <span class="pl-k">=</span> <span style="color: #267F99">Partial</span>&lt;<span style="color: #267F99">Thing</span>&gt;;

<span style="color: #148A14">// This now works correctly and is equivalent to</span>
<span style="color: #148A14">//</span>
<span style="color: #148A14">//   interface PartialThing {</span>
<span style="color: #148A14">//       foo?: string;</span>
<span style="color: #148A14">//       bar?: number;</span>
<span style="color: #148A14">//       [baz]?: boolean;</span>
<span style="color: #148A14">//   }</span></pre>
</div> Unfortunately this is a breaking change for any usage where users believed that for any type 

<code style="color: #a31515">T</code>, <code style="color: #a31515">keyof T</code> would always be assignable to a <code style="color: #a31515">string</code>. Because symbol- and numeric-named properties invalidate this assumption, we expect some minor breaks which we believe to be easy to catch. In such cases, there are several possible workarounds. If you have code that's really meant to only operate on string properties, you can use <code style="color: #a31515">Extract&lt;keyof T, string&gt;</code> to restrict <code style="color: #a31515">symbol</code> and <code style="color: #a31515">number</code> inputs: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> useKey&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">K</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Extract</span>&lt;<span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>, <span style="color: #0000ff">string</span>&gt;&gt;(<span class="pl-v">obj</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>, <span class="pl-v">k</span><span class="pl-k">:</span> <span style="color: #267F99">K</span>) {
    <span style="color: #0000ff">let</span> propName<span class="pl-k">:</span> <span style="color: #0000ff">string</span> <span class="pl-k">=</span> <span class="pl-smi">k</span>;
    <span style="color: #148A14">// ...</span>
}</pre>
</div> If you have code that's more broadly applicable and can handle more than just 

<code style="color: #a31515">string</code>s, you should be able to substitute <code style="color: #a31515">string</code> with <code style="color: #a31515">string | number | symbol</code>, or use the built-in type alias <code style="color: #a31515">PropertyKey</code>. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> useKey&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">K</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>&gt;(<span class="pl-v">obj</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>, <span class="pl-v">k</span><span class="pl-k">:</span> <span style="color: #267F99">K</span>) {
    <span style="color: #0000ff">let</span> propName<span class="pl-k">:</span> <span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">symbol</span> <span class="pl-k">=</span> <span class="pl-smi">k</span>; 
    <span style="color: #148A14">// ...</span>
}</pre>
</div> Alternatively, users can revert to the old behavior under the 

<code style="color: #a31515">--keyofStringsOnly</code> compiler flag, but this is meant to be used as a transitionary flag. 
## <code style="color: #a31515;font-size: 29px">import()</code> types One long-running pain-point in TypeScript has been the inability to reference a type in another module, or the type of the module itself, without including an import at the top of the file. In some cases, this is just a matter of convenience - you might not want to add an import at the top of your file just to describe a single type's usage. For example, to reference the type of a module at an arbitrary location, here's what you'd have to write before TypeScript 2.9: 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">import</span> <span class="pl-c1">*</span> <span style="color: #0000ff">as</span> <span class="pl-smi">_foo</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>foo<span class="pl-pds">"</span></span>;

<span style="color: #0000ff">export</span> <span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">let</span> foo<span class="pl-k">:</span> <span style="color: #0000ff">typeof</span> <span class="pl-en">_foo</span> <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>foo<span class="pl-pds">"</span></span>);
}</pre>
</div> In other cases, there are simply things that users can't achieve today - for example, referencing a type within a module in the global scope is impossible today. This is because a file with any imports or exports is considered a module, so adding an import for a type in a global script file will automatically turn that file into a module, which drastically changes things like scoping rules and strict module within that file. That's why TypeScript 2.9 is introducing the new 

<code style="color: #a31515">import(...)</code> type syntax. Much like ECMAScript's proposed <code style="color: #a31515">import(...)</code> expressions, import types use the same syntax, and provide a convenient way to reference the type of a module, or the types which a module contains. <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// foo.ts</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    age<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #148A14">// bar.ts</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> greet(<span class="pl-v">p</span><span class="pl-k">:</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>).<span style="color: #267F99">Person</span>) {
    <span style="color: #0000ff">return</span> <span style="color: #a31515"><span class="pl-pds">`</span></span>
<span style="color: #a31515">        Hello, I'm ${<span class="pl-smi">p</span>.<span style="color: #0000ff">name</span>}, and I'm ${<span class="pl-smi">p</span>.<span class="pl-smi">age</span>} years old.</span>
<span style="color: #a31515">    <span class="pl-pds">`</span></span>;
}</pre>
</div> Notice we didn't need to add a top-level import specify the type of 

<code style="color: #a31515">p</code>. We could also rewrite our example from above where we awkwardly needed to reference the type of a module: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">let</span> foo<span class="pl-k">:</span> <span style="color: #0000ff">typeof</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>) <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>);
}</pre>
</div> Of course, in this specific example 

<code style="color: #a31515">foo</code> could have been inferred, but this might be more useful with something like [the TypeScript language server plugin API][2]. 
## Breaking changes

### <code style="color: #a31515;font-size: 26px">keyof</code> types include symbolic/numeric properties As mentioned above, key queries/

<code style="color: #a31515">keyof</code> types now include names that are <code style="color: #a31515">symbol</code>s and <code style="color: #a31515">number</code>s, which can break some code that assumes <code style="color: #a31515">keyof T</code> is assignable to <code style="color: #a31515">string</code>. Users can avoid this by using the <code style="color: #a31515">--keyofStringsOnly</code> compiler option: <div class="highlight highlight-source-js">
  <pre><span style="color: #148A14">// tsconfig.json</span>
{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span><span class="pl-k">:</span> {
        <span style="color: #a31515"><span class="pl-pds">"</span>keyofStringsOnly<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>
    }
}</pre>
</div>

### Trailing commas not allowed on rest parameters

[#22262][3] This break was added for conformance with ECMAScript, as trailing commas are not allowed to follow rest parameters in the specification. 
### Unconstrained type parameters are no longer assignable to <code style="color: #a31515;font-size: 26px">object</code> in <code style="color: #a31515;font-size: 26px">strictNullChecks</code>

[#24013][4] The following code now errors: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> f&lt;<span style="color: #267F99">T</span>&gt;(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> y<span class="pl-k">:</span> <span style="color: #0000ff">object</span> <span class="pl-k">|</span> <span style="color: #0000ff">null</span> <span class="pl-k">|</span> <span style="color: #0000ff">undefined</span> <span class="pl-k">=</span> <span class="pl-smi">x</span>;
}</pre>
</div> Since generic type parameters can be substituted with any primitive type, this is a precaution TypeScript has added under 

<code style="color: #a31515">strictNullChecks</code>. To fix this, you can add a constraint on <code style="color: #a31515">object</code>: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// We can add an upper-bound constraint here.</span>
<span style="color: #148A14">//           vvvvvvvvvvvvvvv</span>
<span style="color: #0000ff">function</span> f&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">object</span>&gt;(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> y<span class="pl-k">:</span> <span style="color: #0000ff">object</span> <span class="pl-k">|</span> <span style="color: #0000ff">null</span> <span class="pl-k">|</span> <span style="color: #0000ff">undefined</span> <span class="pl-k">=</span> <span class="pl-smi">x</span>;
}</pre>
</div>

### <code style="color: #a31515;font-size:26px">never</code> can no longer be iterated over

[#22964][5] Values of type <code style="color: #a31515">never</code> can no longer be iterated over, which may catch a good class of bugs. Users can avoid this behavior by using a type assertion to cast to the type <code style="color: #a31515">any</code> (i.e. <code style="color: #a31515">foo as any</code>). 
## What's next? We try to keep our plans easily discoverable 

[on the TypeScript roadmap][6] for everything else that's coming in 2.9 and beyond. TypeScript 2.9 proper should arrive towards the end of the month, but to make that successful, we need all the help we can get, so download the RC today and let us know what you think! Feel free to [drop us a line on GitHub if you run into any problems][7], and let others know how you feel about this RC on Twitter and in the comments below!

 [1]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [2]: https://github.com/Microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin#setup-and-initialization
 [3]: https://github.com/Microsoft/TypeScript/pull/22262
 [4]: https://github.com/Microsoft/TypeScript/issues/24013
 [5]: https://github.com/Microsoft/TypeScript/pull/22964
 [6]: https://github.com/Microsoft/TypeScript/wiki/Roadmap
 [7]: https://github.com/Microsoft/TypeScript/issues/new