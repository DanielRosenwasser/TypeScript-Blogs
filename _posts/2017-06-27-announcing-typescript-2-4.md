---
ID: 1095
post_title: Announcing TypeScript 2.4
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-4/
published: true
post_date: 2017-06-27 16:53:33
---
<div style="font-size: 16px">
  Today we're excited to announce the release of TypeScript 2.4! If you haven't yet heard of TypeScript, it's a superset of JavaScript that brings static types and powerful tooling to JavaScript. These static types are entirely optional and get erased away - you can gradually introduce them to your existing JavaScript code, and get around to adding them when you really need. At the same time, you can use them aggressively to your advantage to catch painful bugs, focus on more important tests that don't have to do with types, and get a complete editing experience. In the end, you can run TypeScript code through the compiler to get clean readable JavaScript. That includes ECMAScript 3, 5, 2015, and so on. To get started with the latest stable version of TypeScript, you can grab it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.Compiler">through NuGet</a>, or use the following command with npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> Visual Studio 2015 users (who have 
  
  <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>) will be able to get TypeScript <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.4.1-TS-release-dev14update3-20170626.1/TypeScript_Dev14Full.exe">by simply installing it from here</a>. Visual Studio 2017 users using <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes">Update 2</a> will be able to ge TypeScript 2.4 <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.4.1-TS-release-dev14update3-20170626.1/TypeScript_SDK.exe">from this installer</a>. Built-in support for 2.4 should be coming to other editors very soon, but you can configure <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">our Sublime Text plugin</a> to pick up any other version you need. While our <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">What's New in TypeScript</a> page as well as our <a href="https://blogs.msdn.microsoft.com/typescript/2017/06/12/announcing-typescript-2-4-rc/">2.4 RC blog post</a> may be a little more in-depth, let's go over what's here in TypeScript 2.4. <h2 id="dynamic-import-expressions">
    Dynamic <code style="color: #a31515;font-size: 29px">import()</code> expressions
  </h2> Dynamic 
  
  <code style="color: #a31515">import</code> expressions are a new feature in ECMAScript that allows you to asynchronously request a module at any arbitrary point in your program. These modules come back as <code style="color: #a31515">Promise</code>s of the module itself, and can be <code style="color: #a31515">await</code>-ed in an async function, or can be given a callback with <code style="color: #a31515">.then</code>. What this means in short that you can conditionally and lazily import other modules and libraries to make your application more efficient and resource-conscious. For example, here's an <code style="color: #a31515">async</code> function that only imports a utility library when it's needed: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> getZipFile(<span class="pl-v">name</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>, <span class="pl-v">files</span><span class="pl-k">:</span> <span style="color: #267F99">File</span>[])<span class="pl-k">:</span> <span style="color: #267F99">Promise</span>&lt;<span style="color: #267F99">File</span>&gt; {
    <span style="color: #0000ff">const</span> zipUtil <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">'</span>./utils/create-zip-file<span class="pl-pds">'</span></span>);
    <span style="color: #0000ff">const</span> zipContents <span class="pl-k">=</span> <span style="color: #0000ff">await</span> <span class="pl-smi">zipUtil</span>.<span class="pl-en">getAsBlob</span>(<span class="pl-smi">files</span>);
    <span style="color: #0000ff">return</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">File</span>(<span class="pl-smi">zipContents</span>, <span class="pl-smi">name</span>);
}</pre>
  </div> Many bundlers have support for automatically splitting output bundles (a.k.a. "code splitting") based on these 
  
  <code style="color: #a31515">import()</code>expressions, so consider using this new feature with the <code style="color: #a31515">esnext</code> module target. Note that this feature <em>won't</em> work with the <code style="color: #a31515">es2015</code> module target, since the feature is anticipated for ES2018 or later. <h2 id="string-enums">
    String enums
  </h2> TypeScript has had string literal types for quite some time now, and enums since its release. Having had some time to see how these features were being used, we revisited enums for TypeScript 2.4 to see how they could work together. This release of TypeScript now allows enum members to contain string initializers. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">enum</span> <span style="color: #267F99">Colors</span> {
    <span class="pl-smi">Red</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>RED<span class="pl-pds">"</span></span>,
    <span class="pl-smi">Green</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>GREEN<span class="pl-pds">"</span></span>,
    <span class="pl-smi">Blue</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>BLUE<span class="pl-pds">"</span></span>,
}</pre>
  </div> String enums have the benefit that they're much easier to debug with, and can also describe existing systems that use strings. Like numeric enums and string literal types, these enums can be used as tags in discriminated unions as well. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">enum</span> <span style="color: #267F99">ShapeKind</span> {
    <span class="pl-smi">Circle</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>circle<span class="pl-pds">"</span></span>,
    <span class="pl-smi">Square</span> <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>square<span class="pl-pds">"</span></span>
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Circle</span> {
    kind<span class="pl-k">:</span> <span style="color: #267F99">ShapeKind</span>.<span style="color: #267F99">Circle</span>;
    radius<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Square</span> {
    kind<span class="pl-k">:</span> <span style="color: #267F99">ShapeKind</span>.<span style="color: #267F99">Square</span>;
    sideLength<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">Shape</span> <span class="pl-k">=</span> <span style="color: #267F99">Circle</span> <span class="pl-k">|</span> <span style="color: #267F99">Square</span>;</pre>
  </div>
  
  <h2 id="improved-checking-for-generics">
    Improved checking for generics
  </h2> TypeScript 2.4 has improvements in how types are inferred when generics come into play, as well as improved checking when relating two generic function types. 
  
  <h2 id="return-types-as-inference-targets">
    Return types as inference targets
  </h2> One such improvement is that TypeScript now can let types flow through return types in some contexts. This means you can decide more freely where to put your types. For example: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> arrayMap&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt;(<span class="pl-en">f</span><span class="pl-k">:</span> (<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) <span class="pl-k">=&gt;</span> <span style="color: #267F99">U</span>)<span class="pl-k">:</span> (<span class="pl-v">a</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>[]) <span class="pl-k">=&gt;</span> <span style="color: #267F99">U</span>[] {
    <span style="color: #0000ff">return</span> <span class="pl-v">a</span> <span class="pl-k">=&gt;</span> <span class="pl-smi">a</span>.<span class="pl-en">map</span>(<span class="pl-smi">f</span>);
}

<span style="color: #0000ff">const</span> lengths<span class="pl-k">:</span> (<span class="pl-v">a</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>[]) <span class="pl-k">=&gt;</span> <span style="color: #0000ff">number</span>[] <span class="pl-k">=</span> <span class="pl-en">arrayMap</span>(<span class="pl-v">s</span> <span class="pl-k">=&gt;</span> <span class="pl-smi">s</span>.<span class="pl-c1">length</span>);</pre>
  </div> it used to be the case that 
  
  <code style="color: #a31515">s</code> would need to be explicitly annotated or its type would be inferred as <code style="color: #a31515">{}</code>. While <code style="color: #a31515">lengths</code> could be left unannotated in that case, it felt surprising to some users that information from that type wasn't used to infer the type of <code style="color: #a31515">s</code>. In TypeScript 2.4, the type system knows <code style="color: #a31515">s</code> is a <code style="color: #a31515">string</code> from the type of <code style="color: #a31515">lengths</code>, which could better fit your stylistic choices. This also means that some errors will be caught, since TypeScript can find better candidates than the default <code style="color: #a31515">{}</code> type (which is often too permissive). <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> x<span class="pl-k">:</span> <span style="color: #267F99">Promise</span>&lt;<span style="color: #0000ff">string</span>&gt; <span class="pl-k">=</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">Promise</span>(<span class="pl-v">resolve</span> <span class="pl-k">=&gt;</span> {
    <span class="pl-en">resolve</span>(<span style="color: #09885A">10</span>);
    <span style="color: #148A14">//      ~~ Now correctly errors!</span>
});</pre>
  </div>
  
  <h3 id="stricter-checking-for-generic-functions">
    Stricter checking for generic functions
  </h3> TypeScript now tries to unify type parameters when comparing two single-signature types. As a result, you'll get stricter checks when relating two generic signatures which may catch some bugs. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">A</span> <span class="pl-k">=</span> &lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>&gt;(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #267F99">U</span>) <span class="pl-k">=&gt;</span> [<span style="color: #267F99">T</span>, <span style="color: #267F99">U</span>];
<span style="color: #0000ff">type</span> <span style="color: #267F99">B</span> <span class="pl-k">=</span> &lt;<span style="color: #267F99">S</span>&gt;(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">S</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #267F99">S</span>) <span class="pl-k">=&gt;</span> [<span style="color: #267F99">S</span>, <span style="color: #267F99">S</span>];

<span style="color: #0000ff">function</span> f(<span class="pl-v">a</span><span class="pl-k">:</span> <span style="color: #267F99">A</span>, <span class="pl-v">b</span><span class="pl-k">:</span> <span style="color: #267F99">B</span>) {
    <span class="pl-smi">a</span> <span class="pl-k">=</span> <span class="pl-smi">b</span>;  <span style="color: #148A14">// Error</span>
    <span class="pl-smi">b</span> <span class="pl-k">=</span> <span class="pl-smi">a</span>;  <span style="color: #148A14">// Ok</span>
}</pre> As a temporary workaround for any breakage, you may be able to suppress some of these errors using the new 
    
    <code style="color: #a31515">--noStrictGenericChecks</code> flag.
  </div>
  
  <h3 id="strict-contravariance-for-callback-parameters">
    Strict contravariance for callback parameters TypeScript has always compared parameters in a bivariant way. There are a number of reasons for this, and for the most part it didn't appear to be a major issue until we heard more from users about the adverse effects it had with <code style="color: #a31515">Promise</code>s and <code style="color: #a31515">Observable</code>s. Relating two <code style="color: #a31515">Promise</code>s or <code style="color: #a31515">Observable</code>s should use the type arguments in a strictly <em>covariant</em> manner - a <code style="color: #a31515">Promise&lt;T&gt;</code> can only be related to a <code style="color: #a31515">Promise&lt;U&gt;</code> if <code style="color: #a31515">T</code> is relatable to <code style="color: #a31515">U</code>. However, because of parameter bivariance, along with the structural nature of TypeScript, this was previously not the case. TypeScript 2.4 now tightens up how it checks two function types by enforcing the correct directionality on callback parameter type checks. For example: <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Mappable</span>&lt;<span style="color: #267F99">T</span>&gt; {
    map&lt;<span style="color: #267F99">U</span>&gt;(<span class="pl-en">f</span><span class="pl-k">:</span> (<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">T</span>) <span class="pl-k">=&gt;</span> <span style="color: #267F99">U</span>)<span class="pl-k">:</span> <span style="color: #267F99">Mappable</span>&lt;<span style="color: #267F99">U</span>&gt;;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> a<span class="pl-k">:</span> <span style="color: #267F99">Mappable</span>&lt;<span style="color: #0000ff">number</span>&gt;;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> b<span class="pl-k">:</span> <span style="color: #267F99">Mappable</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;;

<span class="pl-smi">a</span> <span class="pl-k">=</span> <span class="pl-smi">b</span>; <span style="color: #148A14">// should fail, now does.</span>
<span class="pl-smi">b</span> <span class="pl-k">=</span> <span class="pl-smi">a</span>; <span style="color: #148A14">// should succeed, continues to do so.</span></pre>
    </div> In other words, TypeScript now catches the above bug, and since 
    
    <code>Mappable</code> is really just a simplified version of <code>Promise</code> or <code>Observable</code>, you'll see similar behavior with them too. Note that this may be a breaking change for some, but this more correct behavior will benefit the vast majority of users in the long run. <h2 id="stricter-checks-on-weak-types">
      Stricter checks on "weak types"
    </h2> TypeScript 2.4 introduces the concept of "weak types". A 
    
    <em>weak</em> type is any type that contains nothing but all-optional properties. For example, this <code>Options</code> type is a weak type: <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Options</span> {
    data<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>,
    timeout<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>,
    maxRetries<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>,
}</pre>
    </div> In TypeScript 2.4, it's now an error to assign anything to a weak type when there's no overlap in properties. That includes primitives like 
    
    <code>number</code>, <code>string</code>, and <code>boolean</code>. For example: <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">function</span> sendMessage(<span class="pl-v">options</span><span class="pl-k">:</span> <span style="color: #267F99">Options</span>) {
    <span style="color: #148A14">// ...</span>
}

<span style="color: #0000ff">const</span> opts <span class="pl-k">=</span> {
    payload: <span style="color: #a31515"><span class="pl-pds">"</span>hello world!<span class="pl-pds">"</span></span>,
    retryOnFail: <span style="color: #0000ff">true</span>,
}

<span style="color: #148A14">// Error!</span>
<span class="pl-en">sendMessage</span>(<span class="pl-smi">opts</span>);
<span style="color: #148A14">// No overlap between the type of 'opts' and 'Options' itself.</span>
<span style="color: #148A14">// Maybe we meant to use 'data'/'maxRetries' instead of 'payload'/'retryOnFail'.</span></pre>
    </div> This check also catches situations like classes that might forget to implement members of an interface: 
    
    <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Foo</span> {
    someMethod<span class="pl-k">?</span>()<span class="pl-k">:</span> <span style="color: #0000ff">void</span>;
    someOtherMethod<span class="pl-k">?</span>(<span class="pl-v">arg</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>)<span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}

<span style="color: #148A14">// Error! Did 'Dog' really need to implement 'Foo'?</span>
<span style="color: #0000ff">class</span> <span style="color: #267F99">Dog</span> <span style="color: #0000ff">implements</span> <span style="color: #267F99">Foo</span> {
    bark() {
        <span style="color: #0000ff">return</span> <span style="color: #a31515"><span class="pl-pds">"</span>woof!<span class="pl-pds">"</span></span>;
    }
}</pre>
    </div> This change to the type system may introduce some breakages, but in our exploration of existing codebases, this new check primarily catches silent errors that users weren't aware of. If you really are sure that a value should be compatible with a weak type, consider the following options: 
    
    <ol>
      <li>
        Declare properties in the weak type that are always expected to be present.
      </li>
      <li>
        Add an index signature to the weak type (i.e. <code>[propName: string]: {}</code>).
      </li>
      <li>
        Use a type assertion (i.e. <code>opts as Options</code>).
      </li>
    </ol> In the case above where the class 
    
    <code>Dog</code> tried to implement <code>Foo</code>, it's possible that <code>Foo</code> was being used to ensure code was implemented correctly later on in a derived class. You can get around this by performing class-interface merging. <div class="highlight highlight-source-ts">
      <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">Dog</span> {
    bark() {
        <span style="color: #0000ff">return</span> <span style="color: #a31515"><span class="pl-pds">"</span>woof!<span class="pl-pds">"</span></span>;
    }
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Dog</span> <span style="color: #0000ff">implements</span> <span style="color: #267F99">Foo</span> {
    <span style="color: #148A14">// 'Dog' now inherits all of the methods from 'Foo'.</span>
}
</pre>
    </div>
    
    <h2 id="enjoy">
      Enjoy!
    </h2> You can read up our full 
    
    <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">what's new in TypeScript</a> page on our wiki for some more details on this new release. To also see a full list of breaking changes, you can look at our <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">breaking changes page</a> as well. Keep in mind that any sort of constructive feedback that you can give us is always appreciated, and used as the basis of every new version of TypeScript. Any issues you run into, or ideas that you think would be helpful for the greater TypeScript community can be filed <a href="https://github.com/Microsoft/TypeScript/issues">on our GitHub issue tracker</a>. If you're enjoying TypeScript 2.4, let us know on Twitter with the <a href="https://twitter.com/intent/tweet?text=%23iHeartTypeScript">#iHeartTypeScript</a> hashtag on Twitter. Thanks for reading up on this release, and happy hacking!
  </h3>
</div>