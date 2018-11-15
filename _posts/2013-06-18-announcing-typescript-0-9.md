---
ID: 193
post_title: Announcing TypeScript 0.9
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-0-9/
published: true
post_date: 2013-06-18 11:00:00
---
Today, we’re excited to be releasing TypeScript 0.9.0.  This release represents the largest update to TypeScript to date, bringing significant changes to the language, compiler and tools.  These span from highly requested new language features like Generics, to a new compiler infrastructure that lays the foundation for TypeScript tools scalability, to hundreds of bug fixes and general improvements.

TypeScript, since its release in October last year, has helped teams write large applications for the web, server, and Windows desktop.  TypeScript's lightweight type system is proving itself to be an effective way of providing tooling and early error detection that makes programming these large applications easier and safer than before. 

This release marks the first step of the 0.9.x series, introducing most of the new capabilities of this series which will be further refined over the next few months based on your feedback.  With today’s release, we’ve made a big step forward on the road to TypeScript 1.0.

Check out the [downloads][1] page to take TypeScript 0.9 for a spin today, take a look at the [breaking changes][2] list for help moving code forward to the new compiler, and send us your feedback on [typescript.codeplex.com][3].

# TypeScript 0.9 Language      

TypeScript 0.9 addresses many of the most highly requested language asks we’ve heard over the last 8 months.  Many of these improvements are targeted at furthering TypeScript’s commitment to enabling accurate and faithful descriptions of JavaScript API patterns.  Through community projects like [DefinitelyTyped][4], we’ve seen a great ecosystem of library typings emerge, which has helped identify where the language changes could be most valuable.

Complete details of the TypeScript 0.9 language are available in the [TypeScript specification][5].  Highlights include:

## Generics

The most highly requested feature by 0.8 users, generics enable capturing relationships between inputs and outputs in APIs, enabling richer type checking for better error reporting and tools.  For example, generics allow the typing of the Array#map function from JavaScript to relate the element type of the array with the parameter to the callback function (‘T’ below), and the return type of the callback function with the return type of ‘map’ (‘U’ below).

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">interface</span></span> Array&lt;T&gt; {
        <span class="rem"><span style="color: #008000">// ...</span></span>
        map&lt;U&gt;(callbackfn: (value: T, index: <span style="color: #0000ff">number</span>, array: T[]) =&gt; U): U[];<br />        <span class="rem"><span style="color: #008000">// ...<br /></span></span>    }<br /><br />    <span class="kwrd"><span style="color: #0000ff">var</span></span> array: Array&lt;<span class="kwrd"><span style="color: #0000ff">string</span></span>&gt; = ["John", "Sam", "George"];<br /><br />    <span class="kwrd"><span style="color: #0000ff">var</span></span> lengths = array.map((val, idx, arr) =&gt; val.length);</pre>

In many cases, the type arguments can be inferred, as in the call to <span style="font-family: courier new,courier">array.map</span> above.

## Overloading on Constants

Many JavaScript APIs use strings parameters whose values determine the type returned by the API.  Overloading on constants enables TypeScript to capture this pattern and others like it.

<div>
  <pre class="csharpcode">&nbsp;&nbsp;&nbsp; <span class="kwrd"><span style="color: #0000ff">interface</span></span> Document {<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; createElement(tagName: <span class="kwrd"><span style="color: #0000ff">string</span></span>): HTMLElement;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; createElement(tagName: <span class="str"><span style="color: #006080">'canvas'</span></span>): HTMLCanvasElement;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; createElement(tagName: <span class="str"><span style="color: #006080">'div'</span></span>): HTMLDivElement;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; createElement(tagName: <span class="str"><span style="color: #006080">'span'</span></span>): HTMLSpanElement;<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; <span class="rem"><span style="color: #008000">// + 100 more</span></span><br />&nbsp;&nbsp;&nbsp; }</pre>
</div>

The 0.9 release uses this technique to give more accurate type information for DOM functions like <span style="font-family: courier new,courier">createElement</span>, <span style="font-family: courier new,courier">getElementsByTagName</span>, <span style="font-family: courier new,courier">addEventListener</span>, and more.

## Export =

TypeScript has built-in language support for external module loaders like [CommonJS][6] and [AMD][7], which enables TypeScript to be used in environments like Node.js on the desktop and with libraries like require.js in the browser.  Both of these module systems provide the ability to provide an explicit value for the module.  For example, a ‘Client’ class may be set as the exported value from a module, allowing the ‘import’ to directly import the class.  TypeScript 0.9 now includes “export =” to support this use case

<div>
  <pre class="csharpcode">    <span class="rem"><span style="color: #008000">// client.ts</span></span> <br />    <span class="kwrd"><span style="color: #0000ff">class</span></span> Client { <br />        <span class="kwrd"><span style="color: #0000ff">constructor</span></span>(<span class="kwrd"><span style="color: #0000ff">public</span></span> name: <span class="kwrd"><span style="color: #0000ff">string</span></span>, <span class="kwrd"><span style="color: #0000ff">public</span></span> description: <span class="kwrd"><span style="color: #0000ff">string</span></span>) { } <br />    } <br />    <span class="kwrd"><span style="color: #0000ff">export</span></span> = Client; <br /><br />    <span class="rem"><span style="color: #008000">// app.ts</span></span> <br />    <span class="kwrd"><span style="color: #0000ff">import</span></span> MyClient = <span class="kwrd"><span style="color: #0000ff">require</span></span>(<span class="str"><span style="color: #006080">'./client'</span></span>); <br />    <span class="kwrd"><span style="color: #0000ff">var</span></span> myClient = <span class="kwrd"><span style="color: #0000ff">new</span></span> MyClient(<span class="str"><span style="color: #006080">"Joe Smith"</span></span>, <span class="str"><span style="color: #006080">"My #1 client"</span></span>);</pre>
</div>

## Enums

TypeScript 0.9 includes a finalized design for enums, offering a typed way to work with finite collections of constant numeric values.

<div>
  <pre class="csharpcode">    <span class="kwrd"><span style="color: #0000ff">enum</span></span> Color { red, blue, green }<br /><br />    <span class="kwrd"><span style="color: #0000ff">var</span></span> myColor = Color.red;<br />    console.log(Color[myColor]); 
</pre>
  
  <p>
    Enums also allow converting to and from string representations, by indexing into the enum, as shown using <span style="font-family: courier new,courier">Color[myColor]</span> in the above example.
  </p>
</div>

## Declaration Merging

Using declaration merging in TypeScript 0.9, you can now grow the static-side of a class or expand a function value with new properties by following each with a module definition of the same name.  This makes it possible to express API patterns like nested classes, functions with static properties and more.

<div>
  <pre class="csharpcode">    <span class="kwrd"><span style="color: #0000ff">function</span></span> readInput(separator = readInput.defaultSeparator) {<br />        <span class="rem"><span style="color: #008000">// read input</span></span><br />    }<br />    <span class="kwrd"><span style="color: #0000ff">module</span></span> readInput {<br />        <span class="kwrd"><span style="color: #0000ff">export</span></span> <span class="kwrd"><span style="color: #0000ff">var</span></span> defaultSeparator = <span class="str"><span style="color: #006080">":"</span></span>;<br />    }
</pre>
</div>

# TypeScript 0.9 Compiler

This release includes a significantly re-engineered compiler, which lays the foundation for great tooling scalability going forward, and provides a much higher fidelity implementation of the TypeScript language specification.

The new compiler addresses over [150 issues][8] reported on CodePlex since the 0.8 release.  These include places where the compiler now catches potential errors more reliably, more accurate tools, and general improvements across the board. 

The new compiler has been designed to enable better incremental performance while editing code in IDEs (completion lists, error reporting, hover type information, etc.).  This is important to ensure that TypeScript IDEs can scale up to the 100k+ line of code codebases that we are already seeing be written in TypeScript today.  The 0.9 release marks the first big step with this new compiler architecture.  Already, 0.9 offers markedly improved editing performance for large codebases, and we expect this to continue to increase significantly as we further tune the new compiler. 

One temporary impact of these changes is that command line compiler compilation with 0.9 is somewhat slower than 0.8.  We expect to see significant improvements here in 0.9.1.

# Summary

Check out [TypeScript 0.9][1] today, watch the [discussion video on Channel 9][9], and send us feedback at [typescript.codeplex.com][10].

<div>
  <div>
    <div>
      <p>
         
      </p>
    </div>
  </div>
</div>

 [1]: http://www.typescriptlang.org/#Download
 [2]: https://typescript.codeplex.com/wikipage?title=Known%20breaking%20changes%20between%200.8%20and%200.9
 [3]: https://typescript.codeplex.com/
 [4]: https://github.com/borisyankov/DefinitelyTyped
 [5]: http://go.microsoft.com/fwlink/?LinkId=267238
 [6]: http://wiki.commonjs.org/wiki/Modules/1.1
 [7]: https://github.com/amdjs/amdjs-api/wiki/AMD
 [8]: http://typescript.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=All&assignedTo=All&component=All&sortField=Id&sortDirection=Ascending&page=0&reasonClosed=Fixed
 [9]: https://aka.ms/S2mlu9
 [10]: http://typescript.codeplex.com/