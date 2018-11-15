---
ID: 1920
post_title: Announcing TypeScript 3.1
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-3-1/
published: true
post_date: 2018-10-08 23:22:17
---
<div style="font-size: 16px">
  Today we're announcing the release of TypeScript 3.1! If you haven't heard of TypeScript, it's a language that builds on top of modern JavaScript and adds static type-checking. When you write TypeScript code, you can use a tool like the TypeScript compiler to remove type-specific constructs, and rewrite any newer ECMAScript code to something that older browsers & runtimes can understand. Additionally, using types means that your tools can analyze your code more easily, and provide you with a rock-solid editing experience giving you things like code-completion, go-to-definition, and quick fixes. On top of that, it's all free, open-source, and cross-platform, so if you're interested in learning more, <a href="https://www.typescriptlang.org/" rel="nofollow">check out our website</a>. If you just want to get started with the latest version of TypeScript, you can get it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a> or via npm by running <div class="highlight highlight-source-ts">
    <pre><span class="pl-smi">npm</span> <span class="pl-smi">install</span> <span class="pl-k">-</span><span class="pl-smi">g</span> <span class="pl-smi">typescript</span></pre>
  </div> You can also get editor support for 
  
  <ul>
    <li>
      <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/3.1.1-TS-release-dev14-update3-20180926.1/TypeScript_SDK.exe" rel="nofollow">Visual Studio 2017</a> (for version 15.2 or later).
    </li>
    <li>
      <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/3.1.1-TS-release-dev14-update3-20180926.1/TypeScript_Dev14Full.exe" rel="nofollow">Visual Studio 2015</a> (which requires update 3).
    </li>
    <li>
      For Visual Studio Code <a href="https://code.visualstudio.com/insiders/" rel="nofollow">by installing the Insiders release</a> (until the next release of VS Code).
    </li>
    <li>
      <a href="https://packagecontrol.io/packages/TypeScript" rel="nofollow">Sublime Text 3 via PackageControl</a>.
    </li>
  </ul>
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a> may have different update schedules, but should all have excellent TypeScript support soon as well. Let's take a look at what this release of TypeScript brings us. Feel free to use the list below to jump around this post a bit. <ul>
    <li>
      <a href="#mappable-tuple-and-array-types">Mappable tuple and array types</a>
    </li>
    <li>
      <a href="#easier-properties-on-function-declarations">Easier properties on function declarations</a>
    </li>
    <li>
      <a href="#version-redirects-for-TypeScript-via-typesVersions">Version redirects for TypeScript via <code style="color: #a31515">typesVersions</code></a>
    </li>
    <li>
      <a href="#refactor-from-then-to-await">Refactor from <code style="color: #a31515">.then()</code> to <code style="color: #a31515">await</code></a>
    </li>
    <li>
      <a href="#breaking-changes">Breaking changes</a>
    </li>
  </ul>
  
  <h2 id="mappable-tuple-and-array-types">
    Mappable tuple and array types
  </h2> Mapping over values in a list is one of the most common patterns in programming. As an example, let's take a look at the following JavaScript code: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span>) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">elements</span>.<span class="pl-en">map</span>(<span class="pl-v">x</span> <span class="pl-k">=&gt;</span> <span class="pl-c1">String</span>(<span class="pl-smi">x</span>));
}</pre>
  </div> The 
  
  <code style="color: #a31515">stringifyAll</code> function takes any number of values, converts each element to a string, places each result in a new array, and returns that array. If we want to have the most general type for <code style="color: #a31515">stringifyAll</code>, we'd declare it as so: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> <span style="color: #0000ff">unknown</span>[])<span class="pl-k">:</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">string</span>&gt;;</pre>
  </div> That basically says, "this thing takes any number of elements, and returns an array of 
  
  <code style="color: #a31515">string</code>s"; however, we've lost a bit of information about <code style="color: #a31515">elements</code> in that transformation. Specifically, the type system doesn't remember the number of elements user passed in, so our output type doesn't have a known length either. We can do something like that with overloads: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [])<span class="pl-k">:</span> <span style="color: #0000ff">string</span>[];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>];
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> stringifyAll(<span class="pl-k">...</span><span class="pl-v">elements</span><span class="pl-k">:</span> [<span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>, <span style="color: #0000ff">unknown</span>])<span class="pl-k">:</span> [<span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>, <span style="color: #0000ff">string</span>];
<span style="color: #148A14">// ... etc</span></pre>
  </div>
  
  <em>Ugh</em>. And we didn't even cover taking <em>four</em> elements yet. You end up special-casing all of these possible overloads, and you end up with what we like to call the "death by a thousand overloads" problem. Sure, we could use conditional types instead of overloads, but then you'd have a bunch of nested conditional types. <em>If only</em> there was a way to uniformly map over each of the types here... Well, TypeScript already has something that sort of does that. TypeScript has a concept called a mapped object type which can generate new types out of existing ones. For example, given the following <code style="color: #a31515">Person</code> type, <div class="highlight highlight-source-ts">
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

<span style="color: #0000ff">function</span> stringifyPerson(<span class="pl-v">person</span><span class="pl-k">:</span> <span style="color: #267F99">Person</span>) {
    <span style="color: #0000ff">const</span> result <span class="pl-k">=</span> {} <span style="color: #0000ff">as</span> <span style="color: #267F99">StringyPerson</span>;
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">const</span> prop <span style="color: #0000ff">in</span> <span class="pl-smi">person</span>) {
        <span class="pl-smi">result</span>[<span class="pl-smi">prop</span>] <span class="pl-k">=</span> <span class="pl-c1">String</span>(<span class="pl-smi">person</span>[<span class="pl-smi">prop</span>]);
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">result</span>;
}</pre>
  </div> Though notice that 
  
  <code style="color: #a31515">stringifyPerson</code> is pretty general. We can abstract the idea of <code style="color: #a31515">Stringify</code>-ing types using a mapped object type over the properties of any given type: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Stringify</span>&lt;<span style="color: #267F99">T</span>&gt; <span class="pl-k">=</span> {
    [<span style="color: #267F99">K</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]<span class="pl-k">:</span> <span style="color: #0000ff">string</span>
};</pre>
  </div> For those unfamiliar, we read this as "for every property named 
  
  <code style="color: #a31515">K</code> in <code style="color: #a31515">T</code>, produce a new property of that name with the type <code style="color: #a31515">string</code>. and rewrite our function to use that: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> stringifyProps&lt;<span style="color: #267F99">T</span>&gt;(<span class="pl-v">obj</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> result <span class="pl-k">=</span> {} <span style="color: #0000ff">as</span> <span style="color: #267F99">Stringify</span>&lt;<span style="color: #267F99">T</span>&gt;;
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">const</span> prop <span style="color: #0000ff">in</span> <span class="pl-smi">obj</span>) {
        <span class="pl-smi">result</span>[<span class="pl-smi">prop</span>] <span class="pl-k">=</span> <span class="pl-c1">String</span>(<span class="pl-smi">obj</span>[<span class="pl-smi">prop</span>]);
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">result</span>;
}

<span class="pl-en">stringifyProps</span>({ hello: <span style="color: #09885A">100</span>, world: <span style="color: #0000ff">true</span> }); <span style="color: #148A14">// has type `{ hello: string, world: string }`</span></pre>
  </div> Seems like we have what we want! However, if we tried changing the type of 
  
  <code style="color: #a31515">stringifyAll</code> to return a <code style="color: #a31515">Stringify</code>: <div class="highlight highlight-source-ts">
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
  
  <code style="color: #a31515">string</code>? Well, let's try to iterate on our coordinates. <div class="highlight highlight-source-ts">
    <pre>   <span class="pl-smi">stringyCoordinates</span>.<span class="pl-c1">forEach</span>(<span class="pl-v">x</span> <span class="pl-k">=&gt;</span> <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span class="pl-smi">x</span>));
<span style="color: #148A14">// ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~</span>
<span style="color: #148A14">// Cannot invoke an expression whose type lacks a call signature. Type 'String' has no compatible call signatures.</span></pre>
  </div> Huh? What's causing this gross error message? Well our 
  
  <code style="color: #a31515">Stringify</code> mapped type not only mapped our tuple members, it also mapped over the methods of <code style="color: #a31515">Array</code>, as well as the <code style="color: #a31515">length</code> property! So <code style="color: #a31515">forEach</code> and <code style="color: #a31515">length</code> both have the type <code style="color: #a31515">string</code>! While technically consistent in behavior, the majority of our team felt that this use-case should just work. Rather than introduce a new concept for mapping over a tuple, mapped object types now just "do the right thing" when iterating over tuples and arrays. This means that if you're already using existing mapped types like <code style="color: #a31515">Partial</code> or <code style="color: #a31515">Required</code> from <code style="color: #a31515">lib.d.ts</code>, they automatically work on tuples and arrays now. While very general, you might note that this functionality means that TypeScript is now better-equipped to express functions similar to <code style="color: #a31515">Promise.all</code>. While that specific change hasn't made its way into this release, it may be coming in the near future. <h2 id="easier-properties-on-function-declarations">
    Easier properties on function declarations
  </h2> In JavaScript, functions are just objects. This means we can tack properties onto them as we please: 
  
  <div class="highlight highlight-source-js">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> <span class="pl-en">readFile</span>(<span class="pl-smi">path</span>) {
    <span style="color: #148A14">// ...</span>
}

<span class="pl-smi">readFile</span>.<span class="pl-en">async</span> <span class="pl-k">=</span> <span style="color: #0000ff">function</span> (<span class="pl-smi">path</span>, <span class="pl-smi">callback</span>) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> TypeScript's traditional approach to this has been an extremely versatile construct called 
  
  <code style="color: #a31515">namespace</code>s (a.k.a. "internal modules" if you're old enough to remember). In addition to organizing code, namespaces support the concept of <em>value-merging</em>, where you can add properties to classes and functions in a declarative way: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> readFile() {
    <span style="color: #148A14">// ...</span>
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">namespace</span> <span class="pl-en">readFile</span> {
    <span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> async() {
        <span style="color: #148A14">// ...</span>
    }
}</pre>
  </div> While perhaps elegant for their time, the construct hasn't aged well. ECMAScript modules have become the preferred format for organizing new code in the broader TypeScript & JavaScript community, and namespaces are TypeScript-specific. Additionally, namespaces don't merge with 
  
  <code style="color: #a31515">var</code>, <code style="color: #a31515">let</code>, or <code style="color: #a31515">const</code> declarations, so code like the following (which is motivated by <code style="color: #a31515">defaultProps</code> from React): <div class="highlight highlight-source-ts">
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
  
  <code style="color: #a31515">const</code> declaration that's initialized with a function, the type-checker will analyze the containing scope to track any added properties. That means that both of the examples - both our <code style="color: #a31515">readFile</code> as well as our <code style="color: #a31515">FooComponent</code> examples - work without modification in TypeScript 3.1! As an added bonus, this functionality in conjunction with TypeScript 3.0's support for <code style="color: #a31515">JSX.LibraryManagedAttributes</code> makes migrating an untyped React codebase to TypeScript <em>significantly</em> easier, since it understands which attributes are optional in the presence of <code style="color: #a31515">defaultProps</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// TypeScript understands that both are valid:</span>
<span class="pl-k">&lt;</span><span class="pl-smi">FooComponent</span> <span class="pl-k">/</span><span class="pl-k">&gt;</span>
<span class="pl-k">&lt;</span><span class="pl-smi">FooComponent</span> <span class="pl-smi">name</span><span class="pl-k">=</span><span style="color: #a31515"><span class="pl-pds">"</span>Nathan<span class="pl-pds">"</span></span> <span class="pl-k">/</span><span class="pl-k">&gt;</span></pre>
  </div>
  
  <h2 id="version-redirects-for-typescript-via-typesversions">
    Version redirects for TypeScript via <code style="color: #a31515;font-size: 29px">typesVersions</code>
  </h2> Many TypeScript and JavaScript users love to use the bleeding-edge features of the languages and tooling. While we love the exuberance that drives this, it can sometimes create a difficult situation for library maintainers where maintainers are forced to choose between supporting new TypeScript features and not breaking older versions of TypeScript. As an example, if you maintain a library which uses the 
  
  <code style="color: #a31515">unknown</code> type from TypeScript 3.0, any of your consumers using earlier versions will be broken. There unfortunately isn't a way to provide types for pre-3.0 versions of TypeScript while also providing types for 3.0 and later. That is, until now. When using Node module resolution in TypeScript 3.1, when TypeScript cracks open a <code style="color: #a31515">package.json</code> file to figure out which files it needs to read, it first looks at a new field called <code style="color: #a31515">typesVersions</code>. A <code style="color: #a31515">package.json</code> with a <code style="color: #a31515">typesVersions</code> field might look like this: <div class="highlight highlight-source-json">
    <pre>{
  <span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>package-name<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>version<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>1.0<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>types<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>./index.d.ts<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>typesVersions<span class="pl-pds">"</span></span>: {
    <span style="color: #a31515"><span class="pl-pds">"</span>&gt;=3.1<span class="pl-pds">"</span></span>: { <span style="color: #a31515"><span class="pl-pds">"</span>*<span class="pl-pds">"</span></span>: [<span style="color: #a31515"><span class="pl-pds">"</span>ts3.1/*<span class="pl-pds">"</span></span>] }
  }
}</pre>
  </div> This 
  
  <code style="color: #a31515">package.json</code> tells TypeScript to check whether the current version of TypeScript is running. If it's 3.1 or later, it figures out the path you've imported relative to the package, and reads from the package's <code style="color: #a31515">ts3.1</code> folder. That's what that <code style="color: #a31515">{ "*": ["ts3.1/*"] }</code> means - if you're familiar with path mapping today, it works exactly like that. So in the above example, if we're importing from <code style="color: #a31515">"package-name"</code>, we'll try to resolve from <code style="color: #a31515">[...]/node_modules/package-name/ts3.1/index.d.ts</code> (and other relevant paths) when running in TypeScript 3.1. If we import from <code style="color: #a31515">package-name/foo</code>, we'll try to look for <code style="color: #a31515">[...]/node_modules/package-name/ts3.1/foo.d.ts</code> and <code style="color: #a31515">[...]/node_modules/package-name/ts3.1/foo/index.d.ts</code>. What if we're not running in TypeScript 3.1 in this example? Well, if none of the fields in <code style="color: #a31515">typesVersions</code> get matched, TypeScript falls back to the <code style="color: #a31515">types</code> field, so here TypeScript 3.0 and earlier will be redirected to <code style="color: #a31515">[...]/node_modules/package-name/index.d.ts</code>. <h3 id="matching-behavior">
    Matching behavior
  </h3> The way that TypeScript decides on whether a version of the compiler & language matches is by using Node's 
  
  <a href="https://github.com/npm/node-semver#ranges">semver ranges</a>. <h3 id="multiple-fields">
    Multiple fields
  </h3>
  
  <code style="color: #a31515">typesVersions</code> can support multiple fields where each field name is specified by the range to match on. <div class="highlight highlight-source-json">
    <pre>{
  <span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>package-name<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>version<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>1.0<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>types<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>./index.d.ts<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>typesVersions<span class="pl-pds">"</span></span>: {
    <span style="color: #a31515"><span class="pl-pds">"</span>&gt;=3.2<span class="pl-pds">"</span></span>: { <span style="color: #a31515"><span class="pl-pds">"</span>*<span class="pl-pds">"</span></span>: [<span style="color: #a31515"><span class="pl-pds">"</span>ts3.2/*<span class="pl-pds">"</span></span>] },
    <span style="color: #a31515"><span class="pl-pds">"</span>&gt;=3.1<span class="pl-pds">"</span></span>: { <span style="color: #a31515"><span class="pl-pds">"</span>*<span class="pl-pds">"</span></span>: [<span style="color: #a31515"><span class="pl-pds">"</span>ts3.1/*<span class="pl-pds">"</span></span>] }
  }
}</pre>
  </div> Since ranges have the potential to overlap, determining which redirect applies is order-specific. That means in the above example, even though both the 
  
  <code style="color: #a31515">&gt;=3.2</code> and the <code style="color: #a31515">&gt;=3.1</code> matchers support TypeScript 3.2 and above, reversing the order could have <em>totally</em> different behavior, so the above sample would not be equivalent to the following. <div class="highlight highlight-source-js">
    <pre>{
  <span style="color: #a31515"><span class="pl-pds">"</span>name<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>package-name<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>version<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>1.0<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>types<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>./index.d.ts<span class="pl-pds">"</span></span>,
  <span style="color: #a31515"><span class="pl-pds">"</span>typesVersions<span class="pl-pds">"</span></span><span class="pl-k">:</span> {
    <span style="color: #148A14">// NOTE: this won't work!</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>&gt;=3.1<span class="pl-pds">"</span></span><span class="pl-k">:</span> { <span style="color: #a31515"><span class="pl-pds">"</span>*<span class="pl-pds">"</span></span><span class="pl-k">:</span> [<span style="color: #a31515"><span class="pl-pds">"</span>ts3.1/*<span class="pl-pds">"</span></span>] },
    <span style="color: #a31515"><span class="pl-pds">"</span>&gt;=3.2<span class="pl-pds">"</span></span><span class="pl-k">:</span> { <span style="color: #a31515"><span class="pl-pds">"</span>*<span class="pl-pds">"</span></span><span class="pl-k">:</span> [<span style="color: #a31515"><span class="pl-pds">"</span>ts3.2/*<span class="pl-pds">"</span></span>] }
  }
}</pre>
  </div>
  
  <h2 id="refactor-from-then-to-await">
    Refactor from <code style="color: #a31515;font-size: 29px">.then()</code> to <code style="color: #a31515;font-size: 29px">await</code>
  </h2> The editing experience we know and love for TypeScript is actually built directly on top of the compiler itself, and so our team is directly responsible for bringing you features like refactorings in tools like Visual Studio, VS Code, and others. Thanks to a lot of fantastic work from Elizabeth Dinella who interned with us this past summer, TypeScript 3.1 now has a refactoring to convert functions that return promises constructed with chains of 
  
  <code style="color: #a31515">.then()</code> and <code style="color: #a31515">.catch()</code> calls to <code style="color: #a31515">async</code> functions that leverage <code style="color: #a31515">await</code>. You can see this in action in the video below. <video src="https://msdnshared.blob.core.windows.net/media/2018/09/3-1-thenToAwait.mp4" width="678" autoplay muted loop> </video> <h2 id="breaking-changes">
    Breaking Changes
  </h2> Our team always strives to avoid introducing breaking changes, but unfortunately there are some to be aware of for TypeScript 3.1. 
  
  <h3 id="vendor-specific-declarations-removed">
    Vendor-specific declarations removed
  </h3> TypeScript 3.1 now generates parts of 
  
  <code style="color: #a31515">lib.d.ts</code> (and other built-in declaration file libraries) using Web IDL files provided from the WHATWG DOM specification. While this means that <code style="color: #a31515">lib.d.ts</code> will be easier to keep up-to-date, many vendor-specific types have been removed. We've covered this in more detail <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">on our wiki</a>. <h3 id="differences-in-narrowing-functions">
    Differences in narrowing functions
  </h3> Using the 
  
  <code style="color: #a31515">typeof foo === "function"</code> type guard may provide different results when intersecting with relatively questionable union types composed of <code style="color: #a31515">{}</code>, <code style="color: #a31515">Object</code>, or unconstrained generics. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff">unknown</span> <span class="pl-k">|</span> (() <span class="pl-k">=&gt;</span> <span style="color: #0000ff">string</span>)) {
    <span style="color: #0000ff">if</span> (<span style="color: #0000ff">typeof</span> <span class="pl-smi">x</span> <span class="pl-k">===</span> <span style="color: #a31515"><span class="pl-pds">"</span>function<span class="pl-pds">"</span></span>) {
        <span style="color: #0000ff">let</span> a <span class="pl-k">=</span> <span class="pl-en">x</span>()
    }
}</pre>
  </div> You can read more on 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes#narrowing-functions-now-intersects--object-and-unconstrained-generic-type-parameters">the breaking changes section of our wiki</a>. <h2 id="whats-next">
    What's next
  </h2> You can take a look at 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our roadmap</a> for things we haven't covered here, as well as to get a sense of what's coming up for the next release of TypeScript. As an example of something we haven't covered much here, TypeScript 3.1 also brought a series of error message improvements which should give helpful tips, and which will do a better job of accurately explaining where things might've gone wrong when you hit an error. Additionally, as an example of something new in store, 3.2 will provide strictly-typed <code style="color: #a31515">call</code>/<code style="color: #a31515">bind</code>/<code style="color: #a31515">apply</code> on function types. So keep an eye on the roadmap to see how things develop. As always, we hope this release continues to make you happier and more productive. If you have any feedback, feel free to <a href="https://github.com/Microsoft/TypeScript/issues">file an issue</a> or <a href="https://twitter.com/typescriptlang" rel="nofollow">reach out over Twitter</a>. Thanks, and happy hacking!
</div>