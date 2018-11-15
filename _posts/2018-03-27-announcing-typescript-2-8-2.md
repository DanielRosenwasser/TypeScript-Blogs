---
ID: 1681
post_title: Announcing TypeScript 2.8
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-8-2/
published: true
post_date: 2018-03-27 16:38:56
---
TypeScript 2.8 is here and brings a few features that we think you'll love *unconditionally*! If you're not familiar with TypeScript, it's a language that adds optional static types to JavaScript. Those static types help make guarantees about your code to avoid typos and other silly errors. They can also help provide nice things like code completions and easier project navigation thanks to tooling built around those types. When your code is run through the TypeScript compiler, you're left with clean, readable, and standards-compliant JavaScript code, potentially rewritten to support much older browsers that only support ECMAScript 5 or even ECMAScript 3. To learn more about TypeScript, [check out our documentation][1]. If you can't wait any longer, you can download TypeScript via <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">NuGet</a> or by running <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript</pre>
</div> You can also get editor support for 

*   [Visual Studio 2015][2] (requires update 3)
*   <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.8.1-TS-release-dev14update3-20180323.2/TypeScript_SDK.exe" rel="nofollow">Visual Studio 2017</a> (requires 15.2 or later)
*   <a href="https://packagecontrol.io/packages/TypeScript" rel="nofollow">Sublime Text 3 via PackageControl</a>
*   Visual Studio Code with the next release, or <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">by following instructions here</a>.

[Other editors][3] may have different update schedules, but should all have excellent TypeScript support soon as well. To get a quick glance at what we're shipping in this release, we put this handy list together to navigate our blog post: 
*   [Conditional types][4]
*   [Declaration-only emit][5]
*   [<code style="color: #a31515">@jsx</code> pragma comments][6]
*   [<code style="color: #a31515">JSX</code> now resolved within factory functions][7]
*   [Granular control on mapped type modifiers][8]
*   [Organize imports][9]
*   [Fixing uninitialized properties][10] We also have some minor 

[breaking changes][11] that you should keep in mind if upgrading. But otherwise, let's look at what new features come with TypeScript 2.8! 
## <a name="conditional-types"></a>Conditional types Conditional types are a new construct in TypeScript that allow us to choose types based on other types. They take the form 

<div class="highlight highlight-source-ts">
  <pre><span class="pl-c1">A</span> <span class="pl-smi">extends</span> <span class="pl-c1">B</span> <span class="pl-k">?</span> <span class="pl-c1">C</span> <span class="pl-k">:</span> <span class="pl-c1">D</span></pre>
</div> where 

<code style="color: #a31515">A</code>, <code style="color: #a31515">B</code>, <code style="color: #a31515">C</code>, and <code style="color: #a31515">D</code> are all types. You should read that as "when the type <code style="color: #a31515">A</code> is assignable to <code style="color: #a31515">B</code>, then this type is <code style="color: #a31515">C</code>; otherwise, it's <code style="color: #a31515">D</code>. If you've used conditional syntax in JavaScript, this will feel familiar to you. Let's take two specific examples: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Animal</span> {
    live()<span class="pl-k">:</span> void;
}
<span style="color: #0000ff">interface</span> <span style="color: #267F99">Dog</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Animal</span> {
    woof()<span class="pl-k">:</span> void;
}

<span style="color: #148A14">// Has type 'number'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span> <span class="pl-k">=</span> <span style="color: #267F99">Dog</span> <span class="pl-en">extends</span> <span style="color: #267F99">Animal</span> ? <span style="color: #0000ff">number</span> : <span style="color: #0000ff">string</span>;

<span style="color: #148A14">// Has type 'string'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">RegExp</span> <span class="pl-en">extends</span> <span style="color: #267F99">Dog</span> ? <span style="color: #0000ff">number</span> : <span style="color: #0000ff">string</span>;</pre>
</div> You might wonder why this is immediately useful. We can tell that 

<code style="color: #a31515">Foo</code> will be <code style="color: #a31515">number</code>, and <code style="color: #a31515">Bar</code> will be <code style="color: #a31515">string</code>, so we might as well write that out explicitly. But the real power of conditional types comes from using them with generics. For example, let's take the following function: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Id</span> { id<span class="pl-k">:</span> <span style="color: #0000ff">number</span>, <span style="color: #148A14">/* other fields */</span> }
<span style="color: #0000ff">interface</span> <span style="color: #267F99">Name</span> { name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>, <span style="color: #148A14">/* other fields */</span> }

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel(<span class="pl-v">id</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>)<span class="pl-k">:</span> <span style="color: #267F99">Id</span>;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel(<span class="pl-v">name</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>)<span class="pl-k">:</span> <span style="color: #267F99">Name</span>;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel(<span class="pl-v">name</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>)<span class="pl-k">:</span> <span style="color: #267F99">Id</span> <span class="pl-k">|</span> <span style="color: #267F99">Name</span>;</pre>
</div> These overloads for 

<code style="color: #a31515">createLabel</code> describe a single JavaScript function that makes a choice based on the types of its inputs. Note two things: 
1.  If a library has to make the same sort of choice over and over throughout its API, this becomes cumbersome.
2.  We have to create three overloads: one for each case when we're *sure* of the type, and one for the most general case. For every other case we'd have to handle, the number of overloads would grow exponentially. Instead, we can use a conditional type to smoosh both of our overloads down to one, and create a type alias so that we can reuse that logic. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">IdOrName</span>&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; <span class="pl-k">=</span>
    <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">number</span> ? <span style="color: #267F99">Id</span> : <span style="color: #267F99">Name</span>;

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt;(<span class="pl-v">idOrName</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>)<span class="pl-k">:</span>
    <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">number</span> ? <span style="color: #267F99">Id</span> : <span style="color: #267F99">Name</span>;

<span style="color: #0000ff">let</span> a <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span>typescript<span class="pl-pds">"</span></span>);   <span style="color: #148A14">// Name</span>
<span style="color: #0000ff">let</span> b <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #09885A">2.8</span>);            <span style="color: #148A14">// Id</span>
<span style="color: #0000ff">let</span> c <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span style="color: #0000ff">as</span> any);      <span style="color: #148A14">// Id | Name</span>
<span style="color: #0000ff">let</span> d <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span style="color: #0000ff">as</span> never);    <span style="color: #148A14">// never</span></pre>
</div> Just like how JavaScript can make decisions at runtime based on the characteristics of a value, conditional types let TypeScript make decisions in the type system based on the characteristics of other types. As another example, we could also write a type called 

<code style="color: #a31515">Flatten</code> that flattens array types to their element types, but leaves them alone otherwise: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// If we have an array, get the type when we index with a 'number'.</span>
<span style="color: #148A14">// Otherwise, leave the type alone.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Flatten</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> any[] ? <span style="color: #267F99">T</span>[<span style="color: #0000ff">number</span>] : <span style="color: #267F99">T</span>;</pre>
</div>

### Inferring within conditional types Conditional types also provide us with a way to infer from types we compare against in the true branch using the 

<code style="color: #a31515">infer</code> keyword. For example, we could have inferred the element type in <code style="color: #a31515">Flatten</code> instead of fetching it out manually: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// We also could also have used '(infer U)[]' instead of 'Array&lt;infer U&gt;'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Flatten</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">Array</span>&lt;<span class="pl-en">infer</span> <span style="color: #267F99">U</span>&gt; ? <span style="color: #267F99">U</span> : <span style="color: #267F99">T</span>;</pre>
</div> Here, we've declaratively introduced a new generic type variable named 

<code style="color: #a31515">U</code> instead of specifying how to retrieve the element type of <code style="color: #a31515">T</code>. This frees us from having to think about how to get the types we're interested in. 
### Distributing on unions with conditionals When conditional types act on a single type parameter, they distribute across unions. So in the following example, 

<code style="color: #a31515">Bar</code> has the type <code style="color: #a31515">string[] | number[]</code> because <code style="color: #a31515">Foo</code> is applied to the union type <code style="color: #a31515">string | number</code>. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> any ? <span style="color: #267F99">T</span>[] : never;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Foo distributes on 'string | number' to the type</span>
<span style="color: #148A14"> *</span>
<span style="color: #148A14"> *    (string extends any ? string[] : never) |</span>
<span style="color: #148A14"> *    (number extends any ? number[] : never)</span>
<span style="color: #148A14"> * </span>
<span style="color: #148A14"> * which boils down to</span>
<span style="color: #148A14"> *</span>
<span style="color: #148A14"> *    string[] | number[]</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;;</pre>
</div> In case you ever need to avoid distributing on unions, you can surround each side of the 

<code style="color: #a31515">extends</code> keyword with square brackets: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> [<span style="color: #267F99">T</span>] <span class="pl-en">extends</span> [any] ? <span style="color: #267F99">T</span>[] : never;

<span style="color: #148A14">// Boils down to Array&lt;string | number&gt;</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;;</pre>
</div> While conditional types can be a little intimidating at first, we believe they'll bring a ton of flexibility for moments when you need to push the type system a little further to get accurate types. 

### New built-in helpers TypeScript 2.8 provides several new type aliases in 

<code style="color: #a31515">lib.d.ts</code> that take advantage of conditional types: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// These are all now built into lib.d.ts!</span>

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Exclude from T those types that are assignable to U</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Exclude</span>&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">U</span> ? never : <span style="color: #267F99">T</span>;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Extract from T those types that are assignable to U</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Extract</span>&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">U</span> ? <span style="color: #267F99">T</span> : never;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Exclude null and undefined from T</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">NonNullable</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> null <span class="pl-k">|</span> undefined ? never : <span style="color: #267F99">T</span>;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Obtain the return type of a function type</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">ReturnType</span>&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> any[]) <span class="pl-k">=&gt;</span> any&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> any[]) <span class="pl-k">=&gt;</span> <span class="pl-en">infer</span> <span style="color: #267F99">R</span> ? <span style="color: #267F99">R</span> : any;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Obtain the return type of a constructor function type</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">InstanceType</span>&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">new</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> any[]) <span class="pl-k">=&gt;</span> any&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">new</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> any[]) <span class="pl-k">=&gt;</span> <span class="pl-en">infer</span> <span style="color: #267F99">R</span> ? <span style="color: #267F99">R</span> : any;</pre>
</div> While 

<code style="color: #a31515">NonNullable</code>, <code style="color: #a31515">ReturnType</code>, and <code style="color: #a31515">InstanceType</code> are relatively self-explanatory, <code style="color: #a31515">Exclude</code> and <code style="color: #a31515">Extract</code> are a bit more interesting. <code style="color: #a31515">Extract</code> selects types from its first argument that are assignable to its second argument: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// string[] | number[]</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span> <span class="pl-k">=</span> <span style="color: #267F99">Extract</span>&lt;<span style="color: #0000ff">boolean</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>[] <span class="pl-k">|</span> <span style="color: #0000ff">number</span>[], any[]&gt;;</pre>
</div>

<code style="color: #a31515">Exclude</code> does the opposite; it removes types from its first argument that are not assignable to its second: <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// boolean</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">Exclude</span>&lt;<span style="color: #0000ff">boolean</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>[] <span class="pl-k">|</span> <span style="color: #0000ff">number</span>[], any[]&gt;;</pre>
</div>

## <a name="declaration-only-emit"></a>Declaration-only emit Thanks to 

[a pull request][12] from [Manoj Patel][13], TypeScript now features an <code style="color: #a31515">--emitDeclarationOnly</code> flag which can be used for cases when you have an alternative build step for emitting JavaScript files, but need to emit declaration files separately. Under this mode no JavaScript files nor sourcemap files will be generated; just <code style="color: #a31515">.d.ts</code> files that can be used for library consumers. One use-case for this is when using alternate compilers for TypeScript such as Babel 7. For an example of repositories taking advantage of this flag, check out [urql from Formidable Labs][14], or take a look at [our Babel starter repo][15]. 
## <a name="jsx-pragma-comments"></a><code style="color: #a31515;font-size: 29px">@jsx</code> pragma comments Typically, users of JSX expect to have their JSX tags rewritten to 

<code style="color: #a31515">React.createElement</code>. However, if you're using libraries that have a React-like factory API, such as Preact, Stencil, Inferno, Cycle, and others, you might want to tweak that emit slightly. Previously, TypeScript only allowed users to control the emit for JSX at a global level using the <code style="color: #a31515">jsxFactory</code> option (as well as the deprecated <code style="color: #a31515">reactNamespace</code> option). However, if you needed to mix any of these libraries in the same application, you'd have been out of luck using JSX for both. Luckily, TypeScript 2.8 now allows you to set your JSX factory on a file-by-file basis by adding an <code style="color: #a31515">// @jsx</code> comment at the top of your file. If you've used the same functionality in Babel, this should look slightly familiar. <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">/** @jsx dom */</span>
<span style="color: #0000ff">import</span> { <span class="pl-smi">dom</span> } <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>./renderer<span class="pl-pds">"</span></span>
&lt;<span class="pl-en">h</span>&gt;&lt;/<span class="pl-en">h</span>&gt;</pre>
</div> The above sample imports a function named 

<code style="color: #a31515">dom</code>, and uses the <code style="color: #a31515">jsx</code> pragma to select <code style="color: #a31515">dom</code> as the factory for all JSX expressions in the file. TypeScript 2.8 will rewrite it to the following when compiling to CommonJS and ES5: <div class="highlight highlight-source-js">
  <pre><span style="color: #0000ff">var</span> renderer_1 <span class="pl-k">=</span> <span>require</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./renderer<span class="pl-pds">"</span></span>);
<span class="pl-smi">renderer_1</span>.<span class="pl-en">dom</span>(<span style="color: #a31515"><span class="pl-pds">"</span>h<span class="pl-pds">"</span></span>, null);</pre>
</div>

## <a name="jsx-namespace-resolution"></a><code style="color: #a31515;font-size: 29px">JSX</code> is resolved via the JSX Factory Currently, when TypeScript uses JSX, it looks up a global 

<code style="color: #a31515">JSX</code> namespace to look up certain types (e.g. "what's the type of a JSX component?"). In TypeScript 2.8, the compiler will try to look up the <code style="color: #a31515">JSX</code> namespace based on the location of your JSX factory. For example, if your JSX factory is <code style="color: #a31515">React.createElement</code>, TypeScript will try to first resolve <code style="color: #a31515">React.JSX</code>, and then resolve <code style="color: #a31515">JSX</code> from within the current scope. This can be helpful when mixing and matching different libraries (e.g. React and Preact) or different versions of a specific library (e.g. React 14 and React 16), as placing the JSX namespace in the global scope can cause issues. Going forward, we recommend that new JSX-oriented libraries avoid placing <code style="color: #a31515">JSX</code> in the global scope, and instead export it from the same location as the respective factory function. However, for backward compatibility, TypeScript will continue falling back to the global scope when necessary. 
## <a name="granular-control-mapped-type-modifiers"></a> Granular control on mapped type modifiers TypeScript's mapped object types are an incredibly powerful construct. One handy feature is that they allow users to create new types that have modifiers set for all their properties. For example, the following type creates a new type based on 

<code style="color: #a31515">T</code> and where every property in <code style="color: #a31515">T</code> becomes <code style="color: #a31515">readonly</code> and optional (<code style="color: #a31515">?</code>). <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// Creates a type with all the properties in T,</span>
<span style="color: #148A14">// but marked both readonly and optional.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">ReadonlyAndPartial</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    <span style="color: #0000ff">readonly</span> [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>]
}</pre>
</div> So mapped object types can 

*add* modifiers, but up until this point, there was no way to *remove* modifiers from <code style="color: #a31515">T</code>. TypeScript 2.8 provides a new syntax for removing modifiers in mapped types with the <code style="color: #a31515">-</code> operator, and a new more explicit syntax for adding modifiers with the <code style="color: #a31515">+</code> operator. For example, <div class="highlight highlight-source-ts">
  <pre>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Mutable</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    -<span style="color: #0000ff">readonly</span> [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>]
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Foo</span> {
    <span style="color: #0000ff">readonly</span> abc<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    def<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}

<span style="color: #148A14">// 'abc' is no longer read-only, but 'def' is still optional.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">TotallyMutableFoo</span> <span class="pl-k">=</span> <span style="color: #267F99">Mutable</span>&lt;<span style="color: #267F99">Foo</span>&gt;</pre>
</div> In the above, 

<code style="color: #a31515">Mutable</code> removes <code style="color: #a31515">readonly</code> from each property of the type that it maps over. Similarly, TypeScript now provides a new <code style="color: #a31515">Required</code> type in <code style="color: #a31515">lib.d.ts</code> that removes optionality from each property: <div class="highlight highlight-source-ts">
  <pre>
<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Make all properties in T required</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Required</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]-?<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>];
}</pre>
</div> The 

<code style="color: #a31515">+</code> operator can be handy when you want to call out that a mapped type is adding modifiers. For example, our <code style="color: #a31515">ReadonlyAndPartial</code> from above could be defined as follows: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">ReadonlyAndPartial</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    +<span style="color: #0000ff">readonly</span> [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]+?<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>];
}</pre>
</div>

## <a name="organize-imports"></a>Organize imports TypeScript's language service now provides functionality to organize imports. This feature will remove any unused imports, sort existing imports by file paths, and sort named imports as well. 

![][16] 
## <a name="uninitialized-properties"></a>Fixing uninitialized properties TypeScript 2.7 introduced extra checking for uninitialized properties in classes. Thanks to 

[a pull request][17] by [Wenlu Wang][18] TypeScript 2.8 brings some helpful quick fixes to make it easier to add to your codebase. ![][19] 
## <a name="conditional-types"></a>Breaking changes

### Unused type parameters are checked under <code style="color: #a31515;font-size: 26px">--noUnusedParameters</code> Unused type parameters were previously reported under 

<code style="color: #a31515">--noUnusedLocals</code>, but are now instead reported under <code style="color: #a31515">--noUnusedParameters</code>. 
### <code style="color: #a31515;font-size: 26px">HTMLObjectElement</code> no longer has an <code style="color: #a31515;font-size: 26px">alt</code> attribute Such behavior is not covered by the WHATWG standard. 

## What's next? We hope that TypeScript 2.8 pushes the envelope further to provide a type system that can truly represent the nature of JavaScript as a language. With that, we believe we can provide you with an experience that continues to make you more productive and happier as you code. Over the next few weeks, we'll have a clearer picture of what's in store for TypeScript 2.9, but as always, you can keep an eye on 

[the TypeScript roadmap][20] to see what we're working on for our next release. You can also try out our nightly releases to try out the future today! For example, generic JSX elements are already out in TypeScript's recent nightly releases! Let us know what you think of this release over <a href="http://twitter.com/typescriptlang/" rel="nofollow">on Twitter</a> or in the comments below, and feel free to report issues and suggestions filing [a GitHub issue][21]. Happy Hacking!

 [1]: https://www.typescriptlang.org/docs/home.html
 [2]: https://gist.github.com/DanielRosenwasser/download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.8.1-TS-release-dev14update3-20180323.2/TypeScript_Dev14Full.exe
 [3]: https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support
 [4]: #conditional-types
 [5]: #declaration-only-emit
 [6]: #jsx-pragma-comments
 [7]: #jsx-namespace-resolution
 [8]: #granular-control-mapped-type-modifiers
 [9]: #organize-imports
 [10]: #uninitialized-properties
 [11]: https://gist.github.com/DanielRosenwasser/6f1913483c5699155ad0777dc37a59b1#breaking-changes
 [12]: https://github.com/Microsoft/TypeScript/pull/20735
 [13]: https://github.com/nojvek
 [14]: https://github.com/FormidableLabs/urql/tree/ce9fb3cc02c8530e0b70cfb31d09690dab8b02dc
 [15]: https://github.com/Microsoft/TypeScript-Babel-Starter
 [16]: http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2018/03/organizeImports-2.8.gif
 [17]: https://github.com/Microsoft/TypeScript/pull/21528
 [18]: https://github.com/Kingwl
 [19]: http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2018/03/uninitializedPropFixes-2.8.gif
 [20]: https://github.com/Microsoft/TypeScript/wiki/Roadmap
 [21]: https://github.com/Microsoft/TypeScript/issues