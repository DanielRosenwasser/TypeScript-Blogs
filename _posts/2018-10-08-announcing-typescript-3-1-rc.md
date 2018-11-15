---
ID: 1917
post_title: Announcing TypeScript 3.1 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-3-1-rc/
published: true
post_date: 2018-10-08 23:21:42
---
<div style="font-size: 16px">
  Today we're happy to announce the availability of the release candidate (RC) of TypeScript 3.1. Our intent with the RC is to gather any and all feedback so that we can ensure our final release is as pleasant as possible. If you'd like to give it a shot now, you can get the RC <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a>, or use npm with the following command: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript@rc</pre>
  </div> You can also get editor support by 
  
  <ul>
    <li>
      <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/3.1.0-TS-release-dev14update3-20180911.1/TypeScript_SDK.exe" rel="nofollow">Downloading for Visual Studio 2017</a> (for version 15.2 or later)
    </li>
    <li>
      <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/3.1.0-TS-release-dev14update3-20180911.1/TypeScript_Dev14Full.exe" rel="nofollow">Downloading for Visual Studio 2015</a> (with <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs" rel="nofollow">Update 3</a>)
    </li>
    <li>
      Following directions for <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a>.
    </li>
  </ul> Let's look at what's coming in TypeScript 3.1! 
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#mappable-tuple-and-array-types" id="user-content-mappable-tuple-and-array-types" class="anchor"></a>Mappable tuple and array types
  </h2> Mapping over values in a list is one of the most common patterns in programming. As an example, let's take a look at the following JavaScript code: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span>) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">elements</span>.<span class="pl-en">map</span>(<span class="pl-v">x</span> <span class="pl-k">=&gt;</span> <span style="color: #0000ff">String</span>(<span class="pl-smi">x</span>));
}</pre>
  </div> The 
  
  <code style="color: #a31515"></code>stringifyAll function takes any number of values, converts each element to a string, places each result in a new array, and returns that array. If we want to have the most general type for <code style="color: #a31515"></code>stringifyAll, we'd declare it as so: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> <span style="color: #0000ff">unknown</span>[])<span class="pl-k">:</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">string</span>&gt;;</pre>
  </div> That basically says, "this thing takes any number of elements, and returns an array of 
  
  <code style="color: #a31515"></code>strings"; however, we've lost a bit of information about <code style="color: #a31515"></code>elements in that transformation. Specifically, the type system doesn't remember the number of elements user passed in, so our output type doesn't have a known length either. We can do something like that with overloads: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [])<span class="pl-k">:</span> <span style="color: #0000ff">string</span>[];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>];
<span style="color: #148A14">// ... etc</span></pre>
  </div>
  
  <em>Ugh</em>. And we didn't even cover taking <em>four</em> elements yet. You end up special-casing all of these possible overloads, and you end up with what we like to call the "death by a thousand overloads" problem. Sure, we could use conditional types instead of overloads, but then you'd have a bunch of nested conditional types. <em>If only</em> there was a way to uniformly map over each of the types here... Well, TypeScript already has something that sort of does that. TypeScript has a concept called a mapped object type which can generate new types out of existing ones. For example, given the following <code style="color: #a31515"></code>Person type, <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Person</span> {
    name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    age<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    isHappy<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
}</pre>
  </div> we might want to convert each property to a string as above: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">StringyPerson</span> {
    name<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    age<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    isHappy<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">function</span> stringifyPerson(<span class="pl-v">p</span><span class="pl-k">:</span> <span style="color: #267F99">Person</span>) {
    <span style="color: #0000ff">const</span> result <span class="pl-k">=</span> {} <span style="color: #0000ff">as</span> <span style="color: #267F99">StringyPerson</span>;
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">const</span> prop <span style="color: #0000ff">in</span> <span class="pl-smi">p</span>) {
        <span class="pl-smi">result</span>[<span class="pl-smi">prop</span>] <span class="pl-k">=</span> <span style="color: #0000ff">String</span>(<span class="pl-smi">p</span>[<span class="pl-smi">prop</span>]);
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">result</span>;
}</pre>
  </div> Though notice that 
  
  <code style="color: #a31515"></code>stringifyPerson is pretty general. We can abstract the idea of <code style="color: #a31515"></code>Stringify-ing types using a mapped object type over the properties of any given type: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Stringify</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">K</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #0000ff">string</span>
};</pre>
  </div> For those unfamiliar, we read this as "for every property named 
  
  <code style="color: #a31515"></code>K in <code style="color: #a31515"></code>T, produce a new property of that name with the type <code style="color: #a31515"></code>string. and rewrite our function to use that: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> stringifyProps&lt;<span style="color: #267F99">T</span>&gt;(<span class="pl-v">p</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> result <span class="pl-k">=</span> {} <span style="color: #0000ff">as</span> <span style="color: #267F99">Stringify</span>&lt;<span style="color: #267F99">T</span>&gt;;
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">const</span> prop <span style="color: #0000ff">in</span> <span class="pl-smi">p</span>) {
        <span class="pl-smi">result</span>[<span class="pl-smi">prop</span>] <span class="pl-k">=</span> <span style="color: #0000ff">String</span>(<span class="pl-smi">p</span>[<span class="pl-smi">prop</span>]);
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">result</span>;
}

<span class="pl-en">stringifyProps</span>({ hello: <span style="color: #09885A">100</span>, world: <span style="color: #0000ff">true</span> }); <span style="color: #148A14">// has type `{ hello: string, world: string }`</span></pre>
  </div> Seems like we have what we want! However, if we tried changing the type of 
  
  <code style="color: #a31515"></code>stringifyAll to return a <code style="color: #a31515"></code>Stringify: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">unknown</span>[]&gt;(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>)<span class="pl-k">:</span> <span style="color: #267F99">Stringify</span>&lt;<span style="color: #267F99">T</span>&gt;;</pre>
  </div> And then tried calling it on an array or tuple, we'd only get something that's 
  
  <em>almost</em> useful prior to TypeScript 3.1. Let's give it a shot on an older version of TypeScript like 3.0: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> stringyCoordinates <span class="pl-k">=</span> <span class="pl-en">stringifyAll</span>(<span style="color: #09885A">100</span>, <span style="color: #0000ff">true</span>);

<span style="color: #148A14">// No errors!</span>
<span style="color: #0000ff">let</span> first<span class="pl-k">:</span> <span style="color: #0000ff">string</span> <span class="pl-k">=</span> <span class="pl-smi">stringyCoordinates</span>[<span style="color: #09885A">0</span>];
<span style="color: #0000ff">let</span> second<span class="pl-k">:</span> <span style="color: #0000ff">string</span> <span class="pl-k">=</span> <span class="pl-smi">stringyCoordinates</span>[<span style="color: #09885A">1</span>];</pre>
  </div> Looks like our tuple indexes have been mapped correctly! Let's check the grab the length now and make sure that's right: 
  
  <div class="highlight highlight-source-ts">
    <pre>   <span style="color: #0000ff">let</span> len<span class="pl-k">:</span> <span style="color: #09885A">2</span> <span class="pl-k">=</span> <span class="pl-smi">stringyCoordinates</span>.<span class="pl-c1">length</span>
<span style="color: #148A14">//     ~~~</span>
<span style="color: #148A14">// Type 'string' is not assignable to type '2'.</span></pre>
  </div> Uh. 
  
  <code style="color: #a31515"></code>string? Well, let's try to iterate on our coordinates. <div class="highlight highlight-source-ts">
    <pre>   <span class="pl-smi">stringyCoordinates</span>.<span class="pl-c1">forEach</span>(<span class="pl-v">x</span> <span class="pl-k">=&gt;</span> <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span class="pl-smi">x</span>));
<span style="color: #148A14">// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~</span>
<span style="color: #148A14">// Cannot invoke an expression whose type lacks a call signature. Type 'String' has no compatible call signatures.</span></pre>
  </div> Huh? What's causing this gross error message? Well our 
  
  <code style="color: #a31515"></code>Stringify mapped type not only mapped our tuple members, it also mapped over the methods of <code style="color: #a31515"></code>Array, as well as the <code style="color: #a31515"></code>length property! So <code style="color: #a31515"></code>forEach and <code style="color: #a31515"></code>length both have the type <code style="color: #a31515"></code>string! While technically consistent in behavior, the majority of our team felt that this use-case should just work. Rather than introduce a new concept for mapping over a tuple, mapped object types now just "do the right thing" when iterating over tuples and arrays. This means that if you're already using existing mapped types like <code style="color: #a31515"></code>Partial or <code style="color: #a31515"></code>Required from <code style="color: #a31515"></code>lib.d.ts, they automatically work on tuples and arrays now. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#properties-on-function-declarations" id="user-content-properties-on-function-declarations" class="anchor"></a>Properties on function declarations
  </h2> In JavaScript, functions are just objects. This means we can tack properties onto them as we please: 
  
  <div class="highlight highlight-source-js">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> <span class="pl-en">readFile</span>(<span class="pl-smi">path</span>) {
    <span style="color: #148A14">// ...</span>
}

<span class="pl-smi">readFile</span>.<span class="pl-en">async</span> <span class="pl-k">=</span> <span style="color: #0000ff">function</span> (<span class="pl-smi">path</span>, <span class="pl-smi">callback</span>) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> TypeScript's traditional approach to this has been an extremely versatile construct called 
  
  <code style="color: #a31515"></code>namespaces (a.k.a. "internal modules" if you're old enough to remember). In addition to organizing code, namespaces support the concept of <em>value-merging</em>, where you can add properties to classes and functions in a declarative way: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> readFile() {
    <span style="color: #148A14">// ...</span>
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">namespace</span> <span class="pl-en">readFile</span> {
    <span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> async() {
        <span style="color: #148A14">// ...</span>
    }
}</pre>
  </div> While perhaps elegant for their time, the construct hasn't aged well. ECMAScript modules have become the preferred format for organizing new code in the broader TypeScript & JavaScript community, and namespaces are TypeScript-specific. Additionally, namespaces don't merge with 
  
  <code style="color: #a31515"></code>var, <code style="color: #a31515"></code>let, or <code style="color: #a31515"></code>const declarations, so code like the following (which is motivated by <code style="color: #a31515"></code>defaultProps from React): <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> FooComponent <span class="pl-k">=</span><span class="pl-k">&gt;</span> ({ <span class="pl-v">name</span> }) <span class="pl-k">=&gt;</span> (
    &lt;<span class="pl-en">div</span>&gt;<span class="pl-v">Hello</span><span class="pl-k">!</span> <span class="pl-v">I</span> <span class="pl-v">am</span> {<span class="pl-smi">name</span>}<span class="pl-k">&lt;</span><span class="pl-k">/</span><span class="pl-v">div</span><span class="pl-k">&gt;</span>
);

<span class="pl-smi">FooComponent</span>.<span class="pl-smi">defaultProps</span> <span class="pl-k">=</span> {
    name: <span style="color: #a31515"><span class="pl-pds">"</span>(anonymous)<span class="pl-pds">"</span></span>,
};</pre>
  </div> can't even simply be converted to 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> FooComponent <span class="pl-k">=</span><span class="pl-k">&gt;</span> ({ <span class="pl-v">name</span> }) <span class="pl-k">=&gt;</span> (
    &lt;<span class="pl-en">div</span>&gt;<span class="pl-v">Hello</span><span class="pl-k">!</span> <span class="pl-v">I</span> <span class="pl-v">am</span> {<span class="pl-smi">name</span>}<span class="pl-k">&lt;</span><span class="pl-k">/</span><span class="pl-v">div</span><span class="pl-k">&gt;</span>
);

<span style="color: #148A14">// Doesn't work!</span>
<span style="color: #0000ff">namespace</span> <span style="color: #267F99">FooComponent</span> {
    <span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> defaultProps <span class="pl-k">=</span> {
        name: <span style="color: #a31515"><span class="pl-pds">"</span>(anonymous)<span class="pl-pds">"</span></span>,
    };
}</pre>
  </div> All of this collectively can be frustrating since it makes migrating to TypeScript harder. Given all of this, we felt that it would be better to make TypeScript a bit "smarter" about these sorts of patterns. In TypeScript 3.1, for any function declaration or 
  
  <code style="color: #a31515"></code>const declaration that's initialized with a function, the type-checker will analyze the containing scope to track any added properties. That means that both of the examples - both our <code style="color: #a31515"></code>readFile as well as our <code style="color: #a31515"></code>FooComponent examples - work without modification in TypeScript 3.1! As an added bonus, this functionality in conjunction with TypeScript 3.0's support for <code style="color: #a31515"></code>JSX.LibraryManagedAttributes makes migrating an untyped React codebase to TypeScript <em>significantly</em> easier, since it understands which attributes are optional in the presence of <code style="color: #a31515"></code>defaultProps: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// TypeScript understands that both are valid:</span>
<span class="pl-k">&lt;</span><span class="pl-smi">FooComponent</span> <span class="pl-k">/</span><span class="pl-k">&gt;</span>
<span class="pl-k">&lt;</span><span class="pl-smi">FooComponent</span> <span class="pl-smi">name</span><span class="pl-k">=</span><span style="color: #a31515"><span class="pl-pds">"</span>Nathan<span class="pl-pds">"</span></span> <span class="pl-k">/</span><span class="pl-k">&gt;</span></pre>
  </div>
  
  <h2>
    <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#breaking-changes" id="user-content-breaking-changes" class="anchor"></a>Breaking Changes
  </h2> Our team always strives to avoid introducing breaking changes, but unfortunately there are some to be aware of for TypeScript 3.1. 
  
  <h3>
    <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#vendor-specific-declarations-removed" id="user-content-vendor-specific-declarations-removed" class="anchor"></a>Vendor-specific declarations removed
  </h3> TypeScript 3.1 now generates parts of 
  
  <code style="color: #a31515"></code>lib.d.ts (and other built-in declaration file libraries) using Web IDL files provided from the WHATWG DOM specification. While this means that <code style="color: #a31515"></code>lib.d.ts will be easier to keep up-to-date, many vendor-specific types have been removed. We've covered this in more detail <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">on our wiki</a>. <h3>
    <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#differences-in-narrowing-functions" id="user-content-differences-in-narrowing-functions" class="anchor"></a>Differences in narrowing functions Using the <code style="color: #a31515"></code>typeof foo === "function" type guard may provide different results when intersecting with relatively questionable union types composed of <code style="color: #a31515"></code>{}, <code style="color: #a31515"></code>Object, or unconstrained generics. <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff">unknown</span> <span class="pl-k">|</span> (() <span class="pl-k">=&gt;</span> <span style="color: #0000ff">string</span>)) {
    <span style="color: #0000ff">if</span> (<span style="color: #0000ff">typeof</span> <span class="pl-smi">x</span> <span class="pl-k">===</span> <span style="color: #a31515"><span class="pl-pds">"</span>function<span class="pl-pds">"</span></span>) {
        <span style="color: #0000ff">let</span> a <span class="pl-k">=</span> <span class="pl-en">x</span>()
    }
}</pre>
    </div> You can read more on 
    
    <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes#narrowing-functions-now-intersects--object-and-unconstrained-generic-type-parameters">the breaking changes section of our wiki</a>. <h2>
      <a href="https://gist.github.com/DanielRosenwasser/554b6b95e65c4dde526f2bcb2ee9129b#going-forward" id="user-content-going-forward" class="anchor"></a>Going forward
    </h2> We're looking forward to hearing about your experience with the RC. As always, keep an eye on 
    
    <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our roadmap</a> to get the whole picture of the release as we stabilize. We expect to ship our final release in just a few weeks, so give it a shot now!
  </h3>
</div>