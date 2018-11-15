---
ID: 1680
post_title: Announcing TypeScript 2.1
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-1-2/
published: true
post_date: 2016-12-07 19:01:02
---
<div style="font-size: 16px">
  We <em>spread</em> ourselves thin, but this is the moment you've been <em>await</em>ing - TypeScript 2.1 is here! For those who are unfamiliar, TypeScript is a language that brings you all the new features of JavaScript, along with optional static types. This gives you an editing experience that can't be beat, along with stronger checks against typos and bugs in your code. This release comes with features that we think will drastically reduce the friction of starting new projects, make the type-checker <em>much</em> more powerful, and give you the tools to write much more expressive code. To start using TypeScript you can use <a href="https://www.nuget.org/packages/Microsoft.TypeScript.Compiler/">NuGet</a>, or install it through npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> You can also grab 
  
  <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS2.1-dev14update3-20161206.2/TypeScript_Dev14Full.exe">the TypeScript 2.1 installer for Visual Studio 2015</a> after getting <a href="https://msdn.microsoft.com/en-us/library/mt752379.aspx">Update 3</a>. Visual Studio Code will usually just prompt you if your TypeScript install is more up-to-date, but you can also follow instructions to use TypeScript 2.1 now with <a href="https://code.visualstudio.com/docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> or <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">our Sublime Text Plugin</a>. We've written previously about some great new things 2.1 has in store, including <strong>downlevel <code>async</code>/<code>await</code></strong> and significantly <strong>improved inference</strong>, in <a href="https://blogs.msdn.microsoft.com/typescript/2016/11/08/typescript-2-1-rc-better-inference-async-functions-and-more/">our announcement for TypeScript 2.1 RC</a>, but here's a bit more about what's new in 2.1. <h2 id="async-functions">
    <a href="#async-functions" class="anchor"></a>Async Functions
  </h2> It bears repeating: downlevel 
  
  <code>async</code> functions have arrived! That means that you can use <code>async</code>/<code>await</code> and target ES3/ES5 without using any other tools. Bringing downlevel <code>async</code>/<code>await</code> to TypeScript involved rewriting our emit pipeline to use tree transforms. Keeping parity meant not just that existing emit didn't change, but that TypeScript's emit speed was on par as well. We're pleased to say that after several months of testing, neither have been impacted, and that TypeScript users should continue to enjoy a stable speedy experience. <h2 id="object-rest--spread">
    <a href="#object-rest--spread" class="anchor"></a>Object Rest & Spread
  </h2> We've been excited to deliver object rest & spread since its original proposal, and today it's here in TypeScript 2.1. Object rest & spread is a new proposal for ES2017 that makes it much easier to partially copy, merge, and pick apart objects. The feature is already used quite a bit when using libraries like Redux. With object spreads, making a shallow copy of an object has never been easier: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> copy <span class="pl-k">=</span> { <span class="pl-k">...</span><span class="pl-smi">original</span> };</pre>
  </div> Similarly, we can merge several different objects so that in the following example, 
  
  <code>merged</code> will have properties from <code>foo</code>, <code>bar</code>, and <code>baz</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> merged <span class="pl-k">=</span> { <span class="pl-k">...</span><span class="pl-smi">foo</span>, <span class="pl-k">...</span><span class="pl-smi">bar</span>, <span class="pl-k">...</span><span class="pl-smi">baz</span> };</pre>
  </div> We can even add new properties in the process: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> nowYoureHavingTooMuchFun <span class="pl-k">=</span> {
    hello: <span style="color: #09885A">100</span>,
    <span class="pl-k">...</span><span class="pl-smi">foo</span>,
    world: <span style="color: #09885A">200</span>,
    <span class="pl-k">...</span><span class="pl-smi">bar</span>,
}</pre>
  </div> Keep in mind that when using object spread operators, any properties in later spreads "win out" over previously created properties. So in our last example, if 
  
  <code>bar</code> had a property named <code>world</code>, then <code>bar.world</code> would have been used instead of the one we explicitly wrote out. Object rests are the dual of object spreads, in that they can extract any extra properties that don't get picked up when destructuring an element: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> { a, b, c, <span class="pl-k">...</span>defghijklmnopqrstuvwxyz } <span class="pl-k">=</span> <span class="pl-smi">alphabet</span>;</pre>
  </div>
  
  <h2 id="keyof-and-lookup-types">
    <a href="#keyof-and-lookup-types" class="anchor"></a><code style="font-size: 29px"></code>keyof and Lookup Types
  </h2> Many libraries take advantage of the fact that objects are (for the most part) just a map of strings to values. Given what TypeScript knows about each value's properties, there's a set of known strings (or keys) that you can use for lookups. That's where the 
  
  <code>keyof</code> operator comes in. <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #00f">string</span>;
    age<span class="pl-k">:</span> <span style="color: #00f">number</span>;
    location<span class="pl-k">:</span> <span style="color: #00f">string</span>;
}

<span style="color: #00f">let</span> propName<span class="pl-k">:</span> <span style="color: #00f"></span><span style="color: #00f">keyof</span> <span style="color: #267F99">Person</span>;</pre>
  </div> The above is equivalent to having written out 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> propName<span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>age<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>location<span class="pl-pds">"</span></span>;</pre>
  </div> This 
  
  <code>keyof</code> operator is actually called an <em>index type query</em>. It's like a query for keys on object types, the same way that <code>typeof</code> can be used as a query for types on values. The dual of this is <em>indexed access types</em>, also called <em>lookup types</em>. Syntactically, they look exactly like an element access, but are written as types: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #00f">string</span>;
    age<span class="pl-k">:</span> <span style="color: #00f">number</span>;
    location<span class="pl-k">:</span> <span style="color: #00f">string</span>;
}

<span style="color: #00f">let</span> a<span class="pl-k">:</span> <span style="color: #267F99">Person</span>[<span style="color: #a31515"><span class="pl-pds">"</span>age<span class="pl-pds">"</span></span>];</pre>
  </div> This is the same as saying that 
  
  <code>n</code> gets the type of the <code>age</code> property in <code>Person</code>. In other words: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">let</span> a<span class="pl-k">:</span> <span style="color: #00f">number</span>;</pre>
  </div> When indexing with a union of literal types, the operator will look up each property and union the respective types together. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Equivalent to the type 'string | number'</span>
<span style="color: #00f">let</span> nameOrAge<span class="pl-k">:</span> <span style="color: #267F99">Person</span>[<span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>age<span class="pl-pds">"</span></span>];</pre>
  </div> This pattern can be used with other parts of the type system to get type-safe lookups, serving users of libraries like 
  
  <a href="http://emberjs.com/">Ember</a>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">function</span> get&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">K</span> <span style="color: #00f">extends</span> <span style="color: #00f">keyof</span> <span style="color: #267F99">T</span>&gt;(<span class="pl-v">obj</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>, <span class="pl-v">propertyName</span><span class="pl-k">:</span> <span style="color: #267F99">K</span>)<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">K</span>] {
    <span style="color: #00f">return</span> <span class="pl-smi">obj</span>[<span class="pl-smi">propertyName</span>];
}

<span style="color: #00f">let</span> x <span class="pl-k">=</span> { foo: <span style="color: #09885A">10</span>, bar: <span style="color: #a31515"><span class="pl-pds">"</span>hello!<span class="pl-pds">"</span></span> };

<span style="color: #00f">let</span> foo <span class="pl-k">=</span> <span class="pl-en">get</span>(<span class="pl-smi">x</span>, <span style="color: #a31515"><span class="pl-pds">"</span>foo<span class="pl-pds">"</span></span>); <span style="color: #148A14">// has type 'number'</span>
<span style="color: #00f">let</span> bar <span class="pl-k">=</span> <span class="pl-en">get</span>(<span class="pl-smi">x</span>, <span style="color: #a31515"><span class="pl-pds">"</span>bar<span class="pl-pds">"</span></span>); <span style="color: #148A14">// has type 'string'</span>

<span style="color: #00f">let</span> oops <span class="pl-k">=</span> <span class="pl-en">get</span>(<span class="pl-smi">x</span>, <span style="color: #a31515"><span class="pl-pds">"</span>wargarbl<span class="pl-pds">"</span></span>); <span style="color: #148A14">// error!</span></pre>
  </div>
  
  <h2 id="mapped-types">
    <a href="#mapped-types" class="anchor"></a>Mapped Types
  </h2>
  
  <em>Mapped types</em> are definitely the most interesting feature in TypeScript 2.1. Let's say we have a <code>Person</code> type: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #00f">string</span>;
    age<span class="pl-k">:</span> <span style="color: #00f">number</span>;
    location<span class="pl-k">:</span> <span style="color: #00f">string</span>;
}</pre>
  </div> Much of the time, we want to take an existing type and make each of its properties entirely optional. With 
  
  <code>Person</code>, we might write the following: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">PartialPerson</span> {
    name<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #00f">string</span>;
    age<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #00f">number</span>;
    location<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #00f">string</span>;
}</pre>
  </div> Notice we had to define a completely new type. Similarly, we might want to perform a shallow freeze of an object: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">FrozenPerson</span> {
    <span style="color: #00f">readonly</span> name<span class="pl-k">:</span> <span style="color: #00f">string</span>;
    <span style="color: #00f">readonly</span> age<span class="pl-k">:</span> <span style="color: #00f">number</span>;
    <span style="color: #00f">readonly</span> location<span class="pl-k">:</span> <span style="color: #00f">string</span>;
}</pre>
  </div> Or we might want to create a related type where all the properties are 
  
  <code>boolean</code>s. <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">interface</span> <span style="color: #267F99">BooleanifiedPerson</span> {
    name<span class="pl-k">:</span> <span style="color: #00f">boolean</span>;
    age<span class="pl-k">:</span> <span style="color: #00f">boolean</span>;
    location<span class="pl-k">:</span> <span style="color: #00f">boolean</span>;
}</pre>
  </div> Notice all this repetition - ideally, much of the same information in each variant of 
  
  <code>Person</code> could have been shared. Let's take a look at how we could write <code>BooleanifiedPerson</code> with a <em>mapped type</em>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">type</span> <span style="color: #267F99">BooleanifiedPerson</span> <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #00f">in</span> <span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>age<span class="pl-pds">"</span></span> <span class="pl-k">|</span> <span style="color: #a31515"><span class="pl-pds">"</span>location<span class="pl-pds">"</span></span>]<span class="pl-k">:</span> <span style="color: #00f">boolean</span>
};</pre>
  </div> Mapped types are produced by taking a union of literal types, and computing a set of properties for a new object type. They're like 
  
  <a href="https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions">list comprehensions in Python</a>, but instead of producing new elements in a list, they produce new properties in a type. In the above example, TypeScript uses each literal type in <code>"name" | "age" | "location"</code>, and produces a property of that name (i.e. properties named <code>name</code>, <code>age</code>, and <code>location</code>). <code>P</code> gets bound to each of those literal types (even though it's not used in this example), and gives the property the type <code>boolean</code>. Right now, this new form doesn't look ideal, but we can use the <code>keyof</code> operator to cut down on the typing: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">type</span> <span style="color: #267F99">BooleanifiedPerson</span> <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #00f">in</span> <span style="color: #00f">keyof</span> <span class="pl-smi">Person</span>]<span class="pl-k">:</span> <span style="color: #00f">boolean</span>
};</pre>
  </div> And then we can generalize it: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">type</span> <span style="color: #267F99">Booleanify</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #00f">in</span> <span style="color: #00f">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #00f">boolean</span>
};

<span style="color: #00f">type</span> <span style="color: #267F99">BooleanifiedPerson</span> <span class="pl-k">=</span> <span style="color: #267F99">Booleanify</span>&lt;<span style="color: #267F99">Person</span>&gt;;</pre>
  </div> With mapped types, we no longer have to create new partial or readonly variants of existing types either. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Keep types the same, but make every property optional.</span>
<span style="color: #00f">type</span> <span style="color: #267F99">Partial</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">P</span> <span style="color: #00f">in</span> <span style="color: #00f">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>];
};

<span style="color: #148A14">// Keep types the same, but make each property to be read-only.</span>
<span style="color: #00f">type</span> <span style="color: #267F99">Readonly</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    <span style="color: #00f">readonly</span> [<span style="color: #267F99">P</span> <span style="color: #00f">in</span> <span style="color: #00f">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #267F99">T</span>[<span style="color: #267F99">P</span>];
};</pre>
  </div> Notice how we leveraged TypeScript 2.1's new indexed access types here by writing out 
  
  <code>T[P]</code>. So instead of defining a completely new type like <code>PartialPerson</code>, we can just write <code>Partial&lt;Person&gt;</code>. Likewise, instead of repeating ourselves with <code>FrozenPerson</code>, we can just write <code>Readonly&lt;Person&gt;</code>! <h2 id="partial-readonly-record-and-pick">
    <a href="#partial-readonly-record-and-pick" class="anchor"></a><code style="font-size: 29px"></code>Partial, <code style="font-size: 29px"></code>Readonly, <code style="font-size: 29px"></code>Record, and <code style="font-size: 29px"></code>Pick
  </h2> Originally, we planned to ship a type operator in TypeScript 2.1 named 
  
  <code>partial</code> which could create an all-optional version of an existing type. This was useful for performing partial updates to values, like when using <a href="https://facebook.github.io/react/">React</a>'s <code>setState</code> method to update component state. Now that TypeScript has mapped types, no special support has to be built into the language for <code>partial</code>. However, because the <code>Partial</code> and <code>Readonly</code> types we used above are so useful, they'll be included in TypeScript 2.1. We're also including two other utility types as well: <code>Record</code> and <code>Pick</code>. You can actually <a href="https://github.com/Microsoft/TypeScript/blob/fb23e6dba1e79c2c13f116299756062ee36cbf09/lib/lib.d.ts#L1366-L1392">see how these types are implemented within <code>lib.d.ts</code> itself</a>. <h2 id="easier-imports">
    <a href="#easier-imports" class="anchor"></a>Easier Imports
  </h2> TypeScript has traditionally been a bit finnicky about exactly how you can import something. This was to avoid typos and prevent users from using packages incorrectly. However, a lot of the time, you might just want to write a quick script and get TypeScript's editing experience. Unfortunately, it's pretty common that as soon as you import something you'll get an error. 
  
  <img src="http://i.imgur.com/S22oNnf.png" alt="The code &#096;import * as lodash from &quot;lodash&quot;;&#096; with an error that &#096;lodash&#096; cannot be found." /> "But I already have that package installed!" you might say. The problem is that TypeScript didn't trust the import since it couldn't find any declaration files for lodash. The fix is pretty simple: <div class="highlight highlight-source-shell">
    <pre>npm install --save @types/lodash</pre>
  </div> But this was a consistent point of friction for developers. And while you can still compile & run your code in spite of those errors, those red squiggles can be distracting while you edit. So we focused on on that one core expectation: 
  
  <blockquote>
    <em style="font-size: 18px">But I already have that package installed!</em>
  </blockquote> and from that statement, the solution became obvious. We decided that TypeScript needs to be more trusting, and in TypeScript 2.1, so long as you have a package installed, you can use it. Do be careful though - TypeScript will assume the package has the type 
  
  <code>any</code>, meaning you can do <em>anything</em> with it. If that's not desirable, you can opt in to the old behavior with <code>--noImplicitAny</code>, which we actually recommend for all new TypeScript projects. <h2 id="enjoy">
    <a href="#enjoy" class="anchor"></a>Enjoy!
  </h2> We believe TypeScript 2.1 is a full-featured release that will make using TypeScript even easier for our existing users, and will open the doors to empower new users. 2.1 has plenty more including 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript#configuration-inheritance">sharing <code>tsconfig.json</code> options</a>, better support for <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript#use-returned-values-from-super-calls-as-this">custom elements</a>, and <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript#support-for-external-helpers-library-tslib">support for importing helper functions</a>, all which you can <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">read about on our wiki</a>. As always, we'd love to hear your feedback, so give 2.1 a try and let us know how you like it! Happy hacking!
</div>