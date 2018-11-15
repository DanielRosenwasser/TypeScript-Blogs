---
ID: 1375
post_title: Announcing TypeScript 2.8 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-8-rc/
published: true
post_date: 2018-03-15 21:08:14
---
<div id="readme" class="readme blob instapaper_body">
  <article class="markdown-body entry-content">Today we're excited to announce and get some early feedback with TypeScript 2.8's Release Candidate. To get started with the RC, you can access it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a>, or use npm with the following command: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript@rc</pre>
  </div> You can also get editor support by 
  
  <ul>
    <li>
      <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.8.0-TS-release-dev14update3-20180313.3/TypeScript_Dev14Full.exe" rel="nofollow">Downloading for Visual Studio 2015</a> (with <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs" rel="nofollow">Update 3</a>)
    </li>
    <li>
      <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.8.0-TS-release-dev14update3-20180313.3/TypeScript_SDK.exe" rel="nofollow">Downloading for Visual Studio 2017</a> (for version 15.2 or later)
    </li>
    <li>
      Following directions for <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a>.
    </li>
  </ul> Let's jump into some highlights that are available in our RC! 
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#conditional-types" id="user-content-conditional-types" class="anchor"></a>Conditional Types
  </h2> TypeScript 2.8 introduces a new construct called 
  
  <em>conditional types</em>. This new construct is based on JavaScript's conditional syntax, and takes the form <code style="color: #a31515">A extends B ? C : D</code>. You should mentally read this as "if the type <code style="color: #a31515">A</code> is assignable to <code style="color: #a31515">B</code>, then the type boils down to <code style="color: #a31515">C</code>, and otherwise becomes <code style="color: #a31515">D</code>". <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Animal</span> {
    live()<span class="pl-k">:</span> <span style="color: #0000ff">void</span>;
}
<span style="color: #0000ff">interface</span> <span style="color: #267F99">Dog</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Animal</span> {
    woof()<span class="pl-k">:</span> <span style="color: #0000ff">void</span>;
}

<span style="color: #148A14">// Has type 'number'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span> <span class="pl-k">=</span> <span style="color: #267F99">Dog</span> <span class="pl-en">extends</span> <span style="color: #267F99">Animal</span> ? <span style="color: #0000ff">number</span> : <span style="color: #0000ff">string</span>;

<span style="color: #148A14">// Has type 'string'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">RegExp</span> <span class="pl-en">extends</span> <span style="color: #267F99">Dog</span> ? <span style="color: #0000ff">number</span> : <span style="color: #0000ff">string</span>;</pre>
  </div> Conditional types help model simple choices made over based on types at runtime, but afford significantly more expressive constructs at design-time. For example, imagine the following function written with overloads: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Id</span> { id<span class="pl-k">:</span> <span style="color: #0000ff">number</span>, <span style="color: #148A14">/* other fields */</span> }
<span style="color: #0000ff">interface</span> <span style="color: #267F99">Name</span> { name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>, <span style="color: #148A14">/* other fields */</span> }

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel(<span class="pl-v">id</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>)<span class="pl-k">:</span> <span style="color: #267F99">Id</span>;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel(<span class="pl-v">name</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>)<span class="pl-k">:</span> <span style="color: #267F99">Name</span>;</pre>
  </div> These overloads describe a single JavaScript function that makes a choice based on the types of its inputs. If a library has to make the same sort of choice over and over throughout its API, this becomes cumbersome. Here, we can use a conditional type to smush both of our overloads down to one, and create a type alias so that we can reuse that logic. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">IdOrName</span>&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; <span class="pl-k">=</span>
    <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">number</span> ? <span style="color: #267F99">Id</span> : <span style="color: #267F99">Name</span>;

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createLabel&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt;(<span class="pl-v">idOrName</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>)<span class="pl-k">:</span> <span style="color: #267F99">IdOrName</span>&lt;<span style="color: #267F99">T</span>&gt;;

<span style="color: #0000ff">let</span> a <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span>daniel<span class="pl-pds">"</span></span>);      <span style="color: #148A14">// Name</span>
<span style="color: #148A14"></span><span style="color: #0000ff">let</span> b <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span class="pl-c1">26</span>);            <span style="color: #148A14">// Id</span>
<span style="color: #0000ff">let</span> c <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span style="color: #0000ff">as</span> <span style="color: #0000ff">any</span>);     <span style="color: #148A14">// Id | Name</span>
<span style="color: #0000ff">let</span> d <span class="pl-k">=</span> <span class="pl-en">createLabel</span>(<span style="color: #a31515"><span class="pl-pds">"</span><span class="pl-pds">"</span></span> <span style="color: #0000ff">as</span> <span style="color: #0000ff">never</span>);   <span style="color: #148A14">// never</span></pre>
  </div> To go further, we could also write a helper type that flattens array types to their element types, but leaves them alone otherwise: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Flatten</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">any</span>[] ? <span style="color: #267F99">T</span>[<span style="color: #0000ff">number</span>] : <span style="color: #267F99">T</span>;</pre>
  </div> Conditional types also provide us with a new way to infer types from the types we compare against using the new 
  
  <code style="color: #a31515">infer</code> keyword which introduces a new type variable. For example, we could have written <code style="color: #a31515">Flatten</code> as follows: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// We also could also have used '(infer U)[]' instead of 'Array&lt;infer U&gt;'</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Flatten</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">Array</span>&lt;<span class="pl-en">infer</span> <span style="color: #267F99">U</span>&gt; ? <span style="color: #267F99">U</span> : <span style="color: #267F99">T</span></pre>
  </div> Similarly, we could write helper functions to get returned types of function types: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">GetReturnedType</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span>
    <span style="color: #267F99">T</span> <span class="pl-en">extends</span> ((<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff">any</span>[]) <span class="pl-k">=&gt;</span> <span class="pl-en">infer</span> <span style="color: #267F99">R</span>) ? <span style="color: #267F99">R</span> : <span style="color: #267F99">T</span>;

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> foo()<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;

<span style="color: #0000ff">type</span> <span style="color: #267F99">FooReturnType</span> <span class="pl-k">=</span> <span style="color: #267F99">GetReturnedType</span>&lt;<span style="color: #0000ff">typeof</span> <span class="pl-en">foo</span>&gt;; <span style="color: #148A14">// number</span></pre>
  </div> One last thing to note is that conditional types 
  
  <em>distribute</em> if the type being checked is a type parameter that ends up being instantiated with a union type. So for example, in the following example, <code style="color: #a31515">Bar</code> has the type <code style="color: #a31515">string[] | number</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">any</span> ? <span style="color: #267F99">T</span>[] : <span style="color: #0000ff">never</span>

<span style="color: #148A14">// Has type 'string[] | number[]'.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Bar</span> <span class="pl-k">=</span> <span style="color: #267F99">Foo</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;</pre>
  </div> For convenience, TypeScript 2.8 provides several new type aliases in 
  
  <code style="color: #a31515">lib.d.ts</code> that use conditional types: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Exclude from T those types that are assignable to U</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Exclude</span>&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">U</span> ? <span style="color: #0000ff">never</span> : <span style="color: #267F99">T</span>;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Extract from T those types that are assignable to U</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Extract</span>&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #267F99">U</span> ? <span style="color: #267F99">T</span> : <span style="color: #0000ff">never</span>;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Exclude null and undefined from T</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">NonNullable</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267F99">T</span> <span class="pl-en">extends</span> <span style="color: #0000ff">null</span> <span class="pl-k">|</span> <span style="color: #0000ff">undefined</span> ? <span style="color: #0000ff">never</span> : <span style="color: #267F99">T</span>;

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Obtain the return type of a function type</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">ReturnType</span>&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff">any</span>[]) <span class="pl-k">=&gt;</span> <span style="color: #0000ff">any</span>&gt; <span class="pl-k">=</span>
    <span style="color: #267F99">T</span> <span class="pl-en">extends</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff">any</span>[]) <span class="pl-k">=&gt;</span> <span class="pl-en">infer</span> <span style="color: #267F99">R</span> ? <span style="color: #267F99">R</span> : <span style="color: #0000ff">any</span>;</pre>
  </div>
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#jsx-pragmas" id="user-content-jsx-pragmas" class="anchor"></a>JSX Pragmas
  </h2> TypeScript now supports a pragma comment for specifying the source of a JSX factory within any file. If you've used Babel, the syntax is the same, but if not, here's an example of what it looks like: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">/** @jsx dom */</span>
<span style="color: #0000ff">import</span> { <span class="pl-smi">dom</span> } <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>./renderer<span class="pl-pds">"</span></span>
&lt;<span class="pl-en">h</span>&gt;&lt;/<span class="pl-en">h</span>&gt;</pre>
  </div> The above sample imports a function named 
  
  <code style="color: #a31515">dom</code>, and uses a JSX pragma to select <code style="color: #a31515">dom</code> as the factory for all JSX expressions in the file. TypeScript 2.8 will rewrite it to the following when compiling to CommonJS and ES5: <div class="highlight highlight-source-js">
    <pre><span style="color: #0000ff">var</span> renderer_1 <span class="pl-k">=</span> <span style="color: #0000ff">require</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./renderer<span class="pl-pds">"</span></span>);
<span class="pl-smi">renderer_1</span>.<span class="pl-en">dom</span>(<span style="color: #a31515"><span class="pl-pds">"</span>h<span class="pl-pds">"</span></span>, <span style="color: #0000ff">null</span>);</pre>
  </div> This feature be useful if your project mixes different libraries that support a React-like element factory (e.g. React, Preact, Stencil, Inferno, Cycle, or even others). 
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#granular-control-on-mapped-type-modifiers" id="user-content-granular-control-on-mapped-type-modifiers" class="anchor"></a>Granular Control on Mapped Type Modifiers
  </h2> TypeScript's mapped object types provide the capability of saying every property in a resulting type is read-only or potentially optional by adding the 
  
  <code style="color: #a31515">readonly</code> or <code style="color: #a31515">?</code> modifiers as appropriate: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Creates a type with all the properties in T,</span>
<span style="color: #148A14">// but marked both readonly and optional.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">AllModifiers</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    <span style="color: #0000ff">readonly</span> [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>]
}</pre>
  </div> This is handy, but until now, mapped types could only 
  
  <em>add</em> modifiers if they weren't previously present. For homomorphic mapped types (which copy all modifiers from the original type), this can be limiting: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Foo</span> {
    <span style="color: #0000ff">readonly</span> abc<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    def<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">Props</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>]
}

<span style="color: #148A14">// All modifiers are copied over.</span>
<span style="color: #148A14">// 'abc' is read-only, and 'def' is optional.</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">IdenticalFoo</span> <span class="pl-k">=</span> <span style="color: #267F99">Props</span>&lt;<span style="color: #267F99">Foo</span>&gt;</pre>
  </div> TypeScript 2.8 allows more explicit syntax for adding and removing modifiers in mapped types with the 
  
  <code style="color: #a31515">+</code> and <code style="color: #a31515">-</code> operators. For example, <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Mutable</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
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
    <pre><span style="color: #148A14">/**</span>
<span style="color: #148A14"> * Make all properties in T required</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">Required</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]-?<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>];
}</pre>
  </div>
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#jsxelement-is-resolved-via-the-jsx-factory" id="user-content-jsxelement-is-resolved-via-the-jsx-factory" class="anchor"></a><code style="color: #a31515;font-size: 29px">JSX.Element</code> is resolved via the JSX Factory
  </h2> Currently, when TypeScript uses JSX, it looks up a global 
  
  <code style="color: #a31515">JSX</code> namespace to look up certain types (e.g. "what's the type of a JSX component?"). In TypeScript 2.8, the compiler will try to look up the <code style="color: #a31515">JSX</code> namespace based on the location of your JSX factory. For example, if your JSX factory is <code style="color: #a31515">React.createElement</code>, TypeScript will try to first resolve <code style="color: #a31515">React.JSX.Element</code>, and then resolve <code style="color: #a31515">JSX.Element</code> within the current scope. This can be helpful when mixing and matching different libraries (e.g. React and Preact) or different versions of a specific library (e.g. React 14 and React 16), as placing the JSX namespace in the global scope can cause issues. Going forward, we recommend that new JSX-oriented libraries avoid placing <code style="color: #a31515">JSX</code> in the global scope, and instead export it from the same location as the respective factory function. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#breaking-changes" id="user-content-breaking-changes" class="anchor"></a>Breaking changes
  </h2>
  
  <h3>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#unused-type-parameters-are-checked-under---nounusedparameters" id="user-content-unused-type-parameters-are-checked-under---nounusedparameters" class="anchor"></a>Unused type parameters are checked under <code style="color: #a31515;font-size: 24px">--noUnusedParameters</code>
  </h3> Unused type parameters were previously reported under 
  
  <code style="color: #a31515"></code>--noUnusedLocals, but are now instead reported under <code style="color: #a31515">--noUnusedParameters</code>. You can read more on issue <a href="https://github.com/Microsoft/TypeScript/issues/20568">#20568</a> <h3>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#htmlobjectelement-no-longer-has-an-alt-attribute" id="user-content-htmlobjectelement-no-longer-has-an-alt-attribute" class="anchor"></a><code style="color: #a31515;font-size: 24px">HTMLObjectElement</code> no longer has an <code style="color: #a31515;font-size: 29px">alt</code> attribute
  </h3> Such behavior is not covered by the WHATWG standard. You can read more on issue 
  
  <a href="https://github.com/Microsoft/TypeScript/issues/21386">#21386</a> <h2>
    <a href="https://gist.github.com/DanielRosenwasser/de16cceb91c9789da0a6127378a12a4f#whats-next" id="user-content-whats-next" class="anchor"></a>What's next?
  </h2> We try to keep our plans easily discoverable on 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">on the TypeScript roadmap</a> for everything else that's coming in 2.8 and beyond. TypeScript 2.8 proper should arrive towards the end of the month, but to make that successful, we need all the help we can get, so download the RC today and let us know what you think! Feel free to <a href="https://github.com/Microsoft/TypeScript/issues/new">drop us a line on GitHub if you run into any problems</a>, and let others know how you feel about this RC on Twitter and in the comments below! </article>
</div>