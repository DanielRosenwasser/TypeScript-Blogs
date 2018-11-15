---
ID: 223
post_title: 'Working on TypeScript 0.9: Generics, Overload on Constants and Compiler Performance'
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/working-on-typescript-0-9-generics-overload-on-constants-and-compiler-performance/
published: true
post_date: 2013-03-25 15:50:00
---
The upcoming TypeScript 0.9 release will represent the most significant changes to TypeScript since the first public release last October, bringing highly requested new language features as well as a significant re-design of the TypeScript compiler. In this post, we’ll give a first quick look at a few of the top investments for the 0.9 release.  Stay tuned for further updates in the coming weeks.

# Generics

Generics has been the [most highly requested][1] feature over the last 6 month, and has always been something we planned to include in TypeScript 1.0.

To understand why this feature seems to be so important, it’s useful to first recognize that many common JavaScript APIs really are *already* generic. The simplest examples are the Array methods in the ECMAScript 5 core libraries. Take for example this use of the Array#map method:

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">var</span></span> lengthOfSecond = [<span class="str"><span style="color: #006080">"a"</span></span>, <span class="str"><span style="color: #006080">"bb"</span></span>, <span class="str"><span style="color: #006080">"ccc"</span></span>].map(x =&gt; x.length)[1];</pre>

  
The ‘map’ method is generic – it can operate on any kind of array, and the kind of array it returns is based on the return type of the callback function it is passed.

Today, these relationships aren’t fully captured in the type of Array#map, and TypeScript infers the ‘lengthOfSecond’ variable above to be ‘any’. If these relationships were part of the type of the Array#map method, TypeScript would be able to infer that the type of ‘lengthOfSecond’ is ‘number’ in the example above without any further type annotation, leading to better tooling and static analysis without any extra work by the author of this code.

It turns out that arrays were special-cased in TypeScript 0.8 to enable part of this to be expressed, but that mechanism wasn’t made available to other libraries that do similar things, like [underscore.js][2], [d3][3], and others.

In TypeScript 0.9, generics will allow us to capture the true intended typing of these libraries. 

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">interface</span></span> Array&lt;T&gt; {
        <span class="rem"><span style="color: #008000">// ...</span></span>
        map&lt;U&gt;(callbackfn: (value: T, index: <span style="color: #0000ff">number</span>, array: T[]) =&gt; U): U[];<br />        <span class="rem"><span style="color: #008000">// ...<br /></span></span>    }</pre>

  
Another example is [knockout.js][4]. Knockout has a notion of observables, which can be created over any type of value and used for databinding and change notification. A simple piece of code might look like:

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">var</span></span> person = {
        name: ko.observable(<span class="str"><span style="color: #006080">"Bob"</span></span>),
        age: ko.observabel(37)
    }

    <span class="kwrd"><span style="color: #0000ff">var</span></span> x = person.name().length + person.age();
    person.name(<span class="str">"<span style="color: #006080">Robert</span>"</span>);
</pre>

  
Here ko.observable is a function which takes some type T and returns an Observable<T> which is a function that can be called with no arguments to get the value of type T, or called with one argument of type T to set the value. Without generics, we would have to say that the get and set functionality produce ‘any’. But with generics, we can capture the intended API shape:

<pre class="csharpcode"><span style="color: #0000ff">module</span>&nbsp;ko {<br />&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;<span style="color: #0000ff">export&nbsp;</span><span class="kwrd" style="color: #0000ff">interface</span> Observable&lt;T&gt; {<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (): T;<br />&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; (value: T):&nbsp;<span style="color: #0000ff">any</span>; <br />&nbsp; &nbsp; &nbsp; &nbsp; }<br /><span style="color: #0000ff"> &nbsp; <br />&nbsp;</span><span style="color: #0000ff">&nbsp; &nbsp; &nbsp; &nbsp;export</span>&nbsp;<span class="kwrd" style="color: #0000ff">function</span> observable&lt;T&gt;(value: T): Observable&lt;T&gt;;<br />&nbsp;&nbsp; &nbsp;}</pre>

  
These examples give a sense of what we expect generics to look like in TypeScript. Here’s a few other design principles that have guided the way we’ve been thinking about generics so far:

*   Generics, like the rest of the TypeScript type system, should be a compile-time only concept that disappears on compilation and generates no artifacts in the resulting JavaScript.
*   Classes and interfaces should be able to be generic.
*   References to the type without generic arguments should be the same as instantiating with ‘any’, so that ‘Array’ is the same as ‘Array<any>’.
*   Call and construct signatures in types should be able to be generic. Callsites can provide, or have inferred, the type arguments. Methods, functions and arrows and classes should all be able to describe generic signatures.

For full details, check out the current draft spec in the ‘develop’ branch on <a href="http://typescript.codeplex.com" target="_blank">typescript.codeplex.com</a>, and stop by the <a href="http://typescript.codeplex.com/discussions" target="_blank">forums</a> for any thoughts, feedback, questions or discussion.

# Overload on Constants

A very common pattern in JavaScript APIs is to take a string parameter whose value implies something about the type that will be returned. Examples include the ‘createElement’ method in the DOM:

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">var</span></span> canv = document.createElement(<span class="str"><span style="color: #006080">'canvas'</span></span>);
    canv.getContext(<span class="str"><span style="color: #006080">'2d'</span></span>);
</pre>

  
Because we passed the string ‘canvas’, this API will return a HTMLCanvasElement, which will allow us to call ‘getContext’. In TypeScript 0.8, the code above would have required an additional cast though, because the connection between ‘canvas’ and HTMLCanvasElement wasn’t available to the TypeScript compiler.

Overload on constants is a TypeScript 0.9 feature which allows functions taking strings to have additional overloaded signatures that take specific string constants as parameters, and may have more specific types elsewhere in the signature. For ‘createElement’, this might look like:

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">interface</span></span> Document {
        createElement(tagName: <span class="kwrd"><span style="color: #0000ff">string</span></span>): HTMLElement;
        createElement(tagName: <span class="str"><span style="color: #006080">'canvas'</span></span>): HTMLCanvasElement;
        createElement(tagName: <span class="str"><span style="color: #006080">'div'</span></span>): HTMLDivElement;
        createElement(tagName: <span class="str"><span style="color: #006080">'span'</span></span>): HTMLSpanElement;
        <span class="rem"><span style="color: #008000">// + 100 more</span></span>
    }
</pre>

  
If you call this function with a string literal argument, you’ll get the more specific type. If you call it with a string expression other than a literal value (i.e. an identifier, the result of a function call, etc.) you’ll get the default ‘HTMLElement’.

Even straightforward cases like ‘createElement’ are fairly common in JavaScript, but there are some more interesting cases where this feature comes in handy. Take for example the ‘addEventListener’ pattern in DOM (or similar ‘on’ pattern in node.js):

<pre class="csharpcode">document.addEventListener(<span class="str"><span style="color: #006080">'mousemove'</span></span>, ev =&gt; {
        ev.clientX;
    });
</pre>

  
In TypeScript 0.8, the type of ‘ev’ would be inferred to be ‘Event’, and the code above would report an error because the ‘clientX’ property doesn’t exist on all ‘Event’s. But the ‘mousemove’ event will always provide a ‘MouseEvent’ argument, so there is an association between ‘mousemove’ as the first parameter to addEventListener, and the parameter type of the callback that is the second parameter to addEventListener.

<pre class="csharpcode"><span class="kwrd"><span style="color: #0000ff">interface</span></span> EventTarget {
        addEventListener(type: <span class="kwrd"><span style="color: #0000ff">string</span></span>, listener: (evt: Event) =&gt; <span class="kwrd"><span style="color: #0000ff">void</span></span>): <span class="kwrd"><span style="color: #0000ff">void</span></span>;
        addEventListener(type: <span class="str"><span style="color: #006080">'mousemove'</span></span>, listener: (evt: MouseEvent) =&gt; <span class="kwrd"><span style="color: #0000ff">void</span></span>): <span class="kwrd"><span style="color: #0000ff">void</span></span>;
        addEventListener(type: <span class="str"><span style="color: #006080">'mouseup'</span></span>, listener: (evt: MouseEvent) =&gt; <span class="kwrd"><span style="color: #0000ff">void</span></span>): <span class="kwrd"><span style="color: #0000ff">void</span></span>;
        addEventListener(type: <span class="str"><span style="color: #006080">'blur'</span></span>, listener: (evt: FocusEvent) =&gt; <span class="kwrd"><span style="color: #0000ff">void</span></span>): <span class="kwrd"><span style="color: #0000ff">void</span></span>;
        <span class="rem"><span style="color: #008000">// + 100 more</span></span>
    }
</pre>

 

# Compiler Architecture

Along with the new language work, TypeScript 0.9 brings a new compiler architecture designed to better handle scaling up to the very large (100kloc and beyond) projects that we’ve seen TypeScript applied to over the last 6 months. To provide rich interactive tooling over projects of this size, the TypeScript compiler needs to be able to do only a relatively small amount of new work when a change is made to the source code.

To accomplish this, we’ve been working on a couple of foundational changes to the TypeScript compiler:

*   A new pull-based type checker which allows requests from IDE services like “what should I show in a completion list here” to do minimal incremental work.
*   A new parser which is more closely aligned with the spec’d TypeScript grammar.

This work is also helping with language correctness, bringing the compiler more fully in line with the language specification.

# Path to TypeScript 0.9.0

Work is well underway on all of these features and [many more][5] for 0.9.0. Because this release will include a compiler with many significant changes, we’re hoping to get out an early alpha preview for early adopters to take for a spin in the next month or so. Until then, check out the spec drafts in the ‘develop’ branch, join us in the forums, and track progress at [typescript.codeplex.com][6].

 [1]: http://typescript.codeplex.com/workitem/185
 [2]: http://underscorejs.org/
 [3]: http://d3js.org/
 [4]: http://knockoutjs.com/
 [5]: http://typescript.codeplex.com/workitem/list/advanced?keyword=&status=Open%2b%28not%2bclosed%29&type=All&priority=All&release=TypeScript%2b0.9.0&assignedTo=All&component=All&sortField=Votes&sortDirection=Descending&page=0
 [6]: http://typescript.codeplex.com/