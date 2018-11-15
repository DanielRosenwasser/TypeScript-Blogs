---
ID: 955
post_title: Announcing TypeScript 2.2
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-2/
published: true
post_date: 2017-02-22 17:35:05
---
<div style="font-size: 16px">
  Today our team is happy to present our latest release with TypeScript 2.2! For those who haven't yet heard of it, TypeScript is a simple extension to JavaScript to add optional types along with all the new ECMAScript features. TypeScript builds on the ECMAScript standard and adds type-checking to make you way more productive through cleaner code and stronger tooling. Your TypeScript code then gets transformed into clean, runnable JavaScript that even older browsers can run. While there are a variety of ways to get TypeScript set up locally in your project, the easiest way to get started is to <a href="https://www.typescriptlang.org/play/">try it out on our site</a> or just install it from npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> If you're a Visual Studio 2015 user with update 3, you can 
  
  <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS-2.2-dev14update3-20170221.2/TypeScript_Dev14Full.exe">install TypeScript 2.2 from here</a>. You can also <a href="https://www.nuget.org/packages/Microsoft.TypeScript.Compiler">grab this release through NuGet</a>. Support in Visual Studio 2017 will come in a future update. If you'd rather not wait for TypeScript 2.2 support by default, you can configure <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">our Sublime Text plugin</a> to pick up whatever version you need. As usual, we've written up about new features on our <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">what's new page</a>, but we'd like to highlight a couple of them. <h1>
    More quick fixes
  </h1> One of the areas we focus on in TypeScript is its tooling - tooling can be leveraged in 
  
  <em>any editor</em> with a plugin system. This is one of the things that makes the TypeScript experience so powerful. With TypeScript 2.2, we're bringing even more goodness to your editor. This release introduces some more useful quick fixes (also called <em>code actions</em>) which can guide you in fixing up pesky errors. This includes <ul>
    <li>
      Adding missing imports
    </li>
    <li>
      Adding missing properties
    </li>
    <li>
      Adding forgotten <code>this.</code> to variables
    </li>
    <li>
      Removing unused declarations
    </li>
    <li>
      Implementing abstract members
    </li>
  </ul> With just a few of these, TypeScript practically writes your code for you. <video width="678" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/02/quick-fixes-TS2_2.mp4"> As you write up your code, TypeScript can give suggestions each step of the way to help out with your errors. </video> Expect similar features in the future. The TypeScript team is committed to ensuring that the JavaScript and TypeScript community gets the best tooling we can deliver. With that in mind, we also want to invite the community to take part in this process. We've seen that code actions can really delight users, and we're very open to suggestions, feedback, and 
  
  <a href="https://github.com/Microsoft/TypeScript/issues?q=is%3Aopen+is%3Aissue+label%3A%22Domain%3A+Code+Actions%22">contributions</a> in this area. <h1>
    The <code style="font-size: 29px">object</code> type
  </h1> The 
  
  <code>object</code> type is a new type in 2.2 that matches any types except for primitive types. In other words, you can assign anything to the <code>object</code> type except for <code>string</code>, <code>boolean</code>, <code>number</code>, <code>symbol</code>, and, when using <code>strictNullChecks</code>, <code>null</code> and <code>undefined</code>. <code>object</code> is distinct from the <code>{}</code> type and <code>Object</code> types in this respect due to structural compatibility. Because the empty object type (<code>{}</code>) also matches primitives, it couldn't model APIs like <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create"><code>Object.create</code></a> which truly only expect objects - not primitives. <code>object</code> on the other hand does well here in that it can properly reject being assigned a <code>number</code>. We'd like to extend our thanks to members of our community who proposed and implemented the feature, including <a href="https://github.com/fdecampredon">François de Campredon</a> and <a href="https://github.com/HerringtonDarkholme/">Herrington Darkholme</a>. <h1>
    Easier string indexing behavior
  </h1> TypeScript has a concept called 
  
  <em>index signatures</em>. Index signatures are part of a type, and tell the type system what the result of an element access should be. For instance, in the following: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Foo</span> {
    <span style="color: #148A14">// Here is a string index signature:</span>
    [<span class="pl-v">prop</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>]<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">const</span> x<span class="pl-k">:</span> <span style="color: #267F99">Foo</span>;

<span style="color: #0000ff">const</span> y <span class="pl-k">=</span> <span class="pl-smi">x</span>[<span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>];</pre>
  </div>
  
  <code>Foo</code> has a string index signature that says "whenever indexing with a <code>string</code>, the output type is a <code>boolean</code>." The core idea is that index signatures here are meant to model the way that objects often serve as maps/dictionaries in JavaScript. Before TypeScript 2.2, writing something like <code>x["propName"]</code> was the only way you could make use of a string index signature to grab a property. A little surprisingly, writing a property access like <code>x.propName</code> wasn't allowed. This is slightly at odds with the way JavaScript actually works since <code>x.propName</code> is semantically the same as <code>x["propName"]</code>. There's a reasonable argument to allow both forms when an index signature is present. In TypeScript 2.2, we're doing just that and relaxing the old restriction. What this means is that things like testing properties on a JSON object has become dramatically more ergonomic. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Config</span> {
    [<span class="pl-v">prop</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>]<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">const</span> options<span class="pl-k">:</span> <span style="color: #267F99">Config</span>;

<span style="color: #148A14">// Used to be an error, now allowed!</span>
<span style="color: #0000ff">if</span> (<span class="pl-smi">options</span>.<span class="pl-smi">debugMode</span>) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div>
  
  <h1>
    Better class support for mixins
  </h1> We've always meant for TypeScript to support the JavaScript patterns you use no matter what style, library, or framework you prefer. Part of meeting that goal involves having TypeScript more deeply understand code as it's written today. With TypeScript 2.2, we've worked to make the language understand the mixin pattern. We made a few changes that involved 
  
  <a href="https://github.com/Microsoft/TypeScript/pull/13604">loosening some restrictions on classes</a>, as well as <a href="https://github.com/Microsoft/TypeScript/pull/13743">adjusting the behavior of how intersection types operate</a>. Together, these adjustments actually allow users to express mixin-style classes in ES2015, where a class can extend anything that constructs some object type. This can be used to bridge ES2015 classes with APIs like <code>Ember.Object.extend</code>. As an example of such a class, we can write the following: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Constructable</span> <span class="pl-k">=</span> <span style="color: #0000ff">new</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff">any</span>[]) <span class="pl-k">=&gt;</span> <span style="color: #0000ff">object</span>;

<span style="color: #0000ff">function</span> <span style="color: #267F99">Timestamped</span>&lt;<span style="color: #267F99">BC</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Constructable</span>&gt;(<span style="color: #267F99">Base</span><span class="pl-k">:</span> <span style="color: #267F99">BC</span>) {
    <span style="color: #0000ff">return</span> <span style="color: #0000ff">class</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Base</span> {
        <span style="color: #0000ff">private</span> _timestamp <span class="pl-k">=</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">Date</span>();
        <span style="color: #0000ff">get</span> timestamp() {
            <span style="color: #0000ff">return</span> <span style="color: #0000ff">this</span>.<span class="pl-smi">_timestamp</span>;
        }
    };
}</pre>
  </div> and dynamically create classes 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span class="pl-en">Point</span> {
    x<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    y<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #0000ff">constructor</span>(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>) {
        <span style="color: #0000ff">this</span>.<span class="pl-c1">x</span> <span class="pl-k">=</span> <span class="pl-smi">x</span>;
        <span style="color: #0000ff">this</span>.<span class="pl-c1">y</span> <span class="pl-k">=</span> <span class="pl-smi">y</span>;
    }
}

<span style="color: #0000ff">const</span> <span style="color: #267F99">TimestampedPoint</span> <span class="pl-k">=</span> <span style="color: #267F99">Timestamped</span>(<span style="color: #267F99">Point</span>);</pre>
  </div> and even extend from those classes 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">SpecialPoint</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Timestamped</span>(<span style="color: #267F99">Point</span>) {
    z<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #0000ff">constructor</span>(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>, <span class="pl-v">z</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>) {
        <span style="color: #0000ff">super</span>(<span class="pl-smi">x</span>, <span class="pl-smi">y</span>);
        <span style="color: #0000ff">this</span>.<span class="pl-c1">z</span> <span class="pl-k">=</span> <span class="pl-smi">z</span>;
    }
}

<span style="color: #0000ff">let</span> p <span class="pl-k">=</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">SpecialPoint</span>(<span style="color: #09885A">1</span>, <span style="color: #09885A">2</span>, <span style="color: #09885A">3</span>);

<span style="color: #148A14">// 'x', 'y', 'z', and 'timestamp' are all valid properties.</span>
<span style="color: #0000ff">let</span> v <span class="pl-k">=</span> <span class="pl-smi">p</span>.<span class="pl-c1">x</span> <span class="pl-k">+</span> <span class="pl-smi">p</span>.<span class="pl-c1">y</span> <span class="pl-k">+</span> <span class="pl-smi">p</span>.<span class="pl-c1">z</span>;
<span class="pl-smi">p</span>.<span class="pl-smi">timestamp</span>.<span class="pl-c1">getMilliseconds</span>()</pre>
  </div>
  
  <h1>
    The <code style="font-size: 29px">react-native</code> JSX emit mode
  </h1> In addition to the 
  
  <code>preserve</code> and <code>react</code> options for JSX, TypeScript now introduces the <code>react-native</code> emit mode. This mode is like a combination of the two, in that it emits to <code>.js</code> files (like <code>--jsx react</code>), but leaves JSX syntax alone (like <code>--jsx preserve</code>). This new mode reflects React Native's behavior, which expects all input files to be <code>.js</code> files. It's also useful for cases where you want to just leave your JSX syntax alone but get <code>.js</code> files out from TypeScript. <h1>
    Support for <code style="font-size: 29px">new.target</code>
  </h1> With TypeScript 2.2, we've implemented ECMAScript's 
  
  <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target"><code>new.target</code> meta-property</a>. <code>new.target</code> is an ES2015 feature that lets constructors figure out if a subclass is being constructed. This feature can be handy since ES2015 doesn't allow constructors to access <code>this</code> before calling <code>super()</code>. <h1>
    What's next?
  </h1> Our team is always looking forward, and is now hard at work on TypeScript 2.3. While 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our team's roadmap</a> should give you an idea of what's to come, we're excited for our next release, where we're looking to deliver <ul>
    <li>
      default types for generics
    </li>
    <li>
      async iterator support
    </li>
    <li>
      downlevel generator support
    </li>
  </ul> Of course, that's only a preview for now. We hope TypeScript 2.2 makes you even more productive, and allows you to be even more expressive in your code. Thanks for taking the time to read through, and as always, happy hacking!
</div>