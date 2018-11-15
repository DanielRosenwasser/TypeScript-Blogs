---
ID: 1285
post_title: Announcing TypeScript 2.7
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-7/
published: true
post_date: 2018-01-31 17:31:26
---
<div id="readme" class="readme blob instapaper_body">
  <article class="markdown-body entry-content">Today we're proud to announce the release of TypeScript 2.7! If you're not familiar with TypeScript, it's a language that brings optional static types to JavaScript by building on JavaScript itself. Running TypeScript code through its compiler emits clean readable JavaScript that runs on any browser, and can also make bleeding-edge ECMAScript features you write work on older browsers. That means that you can take advantage of the design time tooling and safety of types (like code completion & navigation), while still leveraging the familiarity, community, and ubiquity of JavaScript. But if you already know what TypeScript is and want to start using 2.7, go ahead and get it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">on NuGet</a> or download it over npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> Visual Studio 2015 users (who have 
  
  <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs" rel="nofollow">Update 3</a>) can install TypeScript 2.7 <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.7.1-TS-release-dev14update3-20180130.1/TypeScript_Dev14Full.exe" rel="nofollow">from here</a>, and Visual Studio 2017 users using version 15.2 or later will be able to get TypeScript by simply <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.7.1-TS-release-dev14update3-20180130.1/TypeScript_SDK.exe" rel="nofollow">installing it from here</a>. Sublime Text Plugin users can install the new version <a href="https://packagecontrol.io/packages/TypeScript" rel="nofollow">from Package Control</a>. TypeScript 2.7 will be available for Visual Studio Code very soon, but <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">eager users can get it working pretty easily in the meantime</a>. We've got a lot in 2.7. While you can always take a look at <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">the roadmap</a>, we've put together a quick list for a bird's eye view of this release: <ul>
    <li>
      <a href="#stricter-class-property-checks">Stricter class property checks</a>
    </li>
    <li>
      <a href="#definite-assignment-assertions">Definite assignment assertions</a>
    </li>
    <li>
      <a href="#easier-ecmascript-module-interoperability">Easier ECMAScript module interoperability</a>
    </li>
    <li>
      <a href="#unique-symbol-types-and-const-named-properties"><code>unique symbol</code> types and const-named properties</a>
    </li>
    <li>
      <a href="#cleaner-output-in-watch-mode">Cleaner output in <code>--watch</code> mode</a>
    </li>
    <li>
      <a href="#prettier-pretty-output">Prettier <code>--pretty</code> output</a>
    </li>
    <li>
      <a href="#numeric-separators">Numeric Separators</a>
    </li>
    <li>
      <a href="#fixed-length-tuples">Fixed Length Tuples</a>
    </li>
    <li>
      <a href="#in-operator-narrowing-and-accurate-instanceof"><code>in</code> operator narrowing and accurate <code>instanceof</code></a>
    </li>
    <li>
      <a href="#smarter-object-literal-inference">Smarter object literal inference</a>
    </li>
  </ul> We've also put together a 
  
  <a href="#breaking-changes">breaking changes</a> section towards the end of this post which existing users should be aware of. So without further ado, let's see what this release brings! <h2 id="stricter-class-property-checks">
    Stricter class property checks
  </h2> TypeScript 2.7 introduces a new strictness flag named 
  
  <code>--strictPropertyInitialization</code>! This flag makes sure that each instance property of a class gets set in the constructor body, or by a property initializer. In a sense, it brings some of the definite assignment checks from variables to instance properties in classes. For example: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    foo<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    bar <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>;
    baz<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
<span style="color: #148A14">//  ~~~</span>
<span style="color: #148A14">//  Error! Property 'baz' has no initializer and is not assigned directly in the constructor.</span>
    <span style="color: #0000ff">constructor</span>() {
        <span style="color: #0000ff">this</span>.<span class="pl-smi">foo</span> <span class="pl-k">=</span> <span style="color: #09885A">42</span>;
    }
}</pre>
  </div> In the above, 
  
  <code>baz</code> never gets set, and TypeScript reports an error. If we truly meant for <code>baz</code> to potentially be <code>undefined</code>, we should have declared it with the type <code>boolean | undefined</code>. There are certain scenarios where properties might be initialized indirectly (perhaps by a helper method or dependency injection library). In those cases, you can convince the type system you know better by using the <em>definite assignment assertions</em> for your properties. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    <span class="pl-smi">foo</span><span class="pl-k">!</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #148A14">// ^</span>
    <span style="color: #148A14">// Notice this exclamation point!</span>
    <span style="color: #148A14">// This is the "definite assignment assertion" modifier.</span>
    <span style="color: #0000ff">constructor</span>() {
        <span style="color: #0000ff">this</span>.<span class="pl-en">initialize</span>();
    }

    initialize() {
        <span style="color: #0000ff">this</span>.<span class="pl-smi">foo</span> <span class="pl-k">=</span> <span style="color: #09885A">0</span>;
    }
}</pre>
  </div> Keep in mind that 
  
  <code>--strictPropertyInitialization</code> will be turned on along with other <code>--strict</code> mode flags, which can impact your project. You can set the <code>strictPropertyInitialization</code> setting to <code>false</code> in your <code>tsconfig.json</code>'s <code>compilerOptions</code>, or <code>--strictPropertyInitialization false</code> on the command line to turn off this checking. <h2 id="definite-assignment-assertions">
    Definite assignment assertions
  </h2> As expressive as we try to make the type system, we understand there are certain times you might know better than TypeScript. As we mentioned above, definite assignment assertions are a new syntax you can use to convince TypeScript that a property will definitely get assigned. But in addition to working on class properties, TypeScript 2.7 also allows you to use this feature on variable declarations! 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> x!<span class="pl-k">:</span> <span style="color: #0000ff">number</span>[];
<span class="pl-en">initialize</span>();
<span class="pl-smi">x</span>.<span class="pl-c1">push</span>(<span style="color: #09885A">4</span>);

<span style="color: #0000ff">function</span> initialize() {
    <span class="pl-smi">x</span> <span class="pl-k">=</span> [<span style="color: #09885A">0</span>, <span style="color: #09885A">1</span>, <span style="color: #09885A">2</span>, <span style="color: #09885A">3</span>];
}</pre>
  </div> If we hadn't added an exclamation point/bang (
  
  <code>!</code>) after <code>x</code>, TypeScript would have reported that <code>x</code> was never initialized. This can be handy in deferred-initialization, or re-initialization scenarios. <h2 id="easier-ecmascript-module-interoperability">
    Easier ECMAScript module interoperability
  </h2> Before ECMAScript modules were standardized in ES2015, the JavaScript ecosystem had several different module formats that worked in different ways. Once the standard passed, the community was left with a question of how to best interoperate with existing "legacy" module formats. TypeScript and Babel took different approaches, and even now, there really isn't a locked down standard. The short story is that if you've used Babel, Webpack, or React Native, and expected different import behaviors than you were used to, we have a new compiler option for you called 
  
  <code>--esModuleInterop</code>. Babel and Webpack allow users to import these CommonJS modules as default imports, but also provide each property on the namespace import (unless the module was marked with an <code>__esModule</code> flag). <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">import</span> <span class="pl-smi">_</span>, { <span class="pl-smi">pick</span> } <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>lodash<span class="pl-pds">"</span></span>;

<span class="pl-smi">_</span>.<span class="pl-en">pick</span>(<span class="pl-k">...</span>);
<span class="pl-en">pick</span>(<span class="pl-k">...</span>);</pre>
  </div> Because TypeScript's behavior differs, we added the 
  
  <code>--allowSyntheticDefaultImports</code> flag in TypeScript 1.8 to allow users to get this behavior for type-checking (but not emit). In general, TypeScript's view of CommonJS (and AMD) modules is that namespace imports always correspond to the shape of a CommonJS module object, and that a <code>default</code> import just corresponds to a member on that module named <code>default</code>. Under this assumption, you can create a named import <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">import</span> { <span class="pl-smi">range</span> } <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>lodash<span class="pl-pds">"</span></span>;

<span style="color: #0000ff">for</span> (<span style="color: #0000ff">let</span> i <span style="color: #0000ff">of</span> <span class="pl-en">range</span>(<span style="color: #09885A">10</span>)) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> However, ES namespace imports aren't callable, so this approach doesn't always make sense. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">import</span> <span class="pl-c1">*</span> <span style="color: #0000ff">as</span> <span class="pl-smi">express</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>express<span class="pl-pds">"</span></span>;

<span style="color: #148A14">// Should be an error in any valid implementation.</span>
<span style="color: #0000ff">let</span> app <span class="pl-k">=</span> <span class="pl-en">express</span>();</pre>
  </div> To give users the same runtime behavior as Babel or Webpack, TypeScript provides a new 
  
  <code>--esModuleInterop</code> flag when emitting to legacy module formats. Under the new <code>--esModuleInterop</code> flag, these callable CommonJS modules <em>must</em> be imported as default imports like so: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">import</span> <span class="pl-smi">express</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>express<span class="pl-pds">"</span></span>;

<span style="color: #0000ff">let</span> app <span class="pl-k">=</span> <span class="pl-en">express</span>();</pre>
  </div> We strongly suggest that Node.js users leverage this flag with a module target of 
  
  <code>commonjs</code> for libraries like express, which export a callable/constructable module. Webpack users may want to use this as well; however, your code should target <code>esnext</code> modules with a <code>moduleResolution</code> strategy of <code>node</code>. Using <code>esnext</code> modules with <code>--esModuleInterop</code> really only has the effect of turning on <code>--allowSyntheticDefaultImports</code>. <h2 id="unique-symbol-types-and-const-named-properties">
    <code style="color: #a31515;font-size: 29px">unique symbol</code> types and const-named properties
  </h2> TypeScript 2.7 understands ECMAScript symbols more deeply, allowing you to use them more flexibly. One highly-demanded use-case is being able to declare well-typed properties with symbols. For an example, take the following: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">const</span> Foo <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Foo<span class="pl-pds">"</span></span>);
<span style="color: #0000ff">const</span> Bar <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Bar<span class="pl-pds">"</span></span>);

<span style="color: #0000ff">let</span> x <span class="pl-k">=</span> {
    [<span class="pl-smi">Foo</span>]: <span style="color: #09885A">100</span>,
    [<span class="pl-smi">Bar</span>]: <span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>,
};

<span style="color: #0000ff">let</span> a <span class="pl-k">=</span> <span class="pl-smi">x</span>[<span class="pl-smi">Foo</span>]; <span style="color: #148A14">// has type 'number'</span>
<span style="color: #0000ff">let</span> b <span class="pl-k">=</span> <span class="pl-smi">x</span>[<span class="pl-smi">Bar</span>]; <span style="color: #148A14">// has type 'string'</span></pre>
  </div> As you can see, TypeScript can keep track of the fact that 
  
  <code>x</code> has properties declared using the symbols <code>Foo</code> and <code>Bar</code> since both <code>Foo</code> and <code>Bar</code> were declared as constants. TypeScript leverages this fact and gives both <code>Foo</code> and <code>Bar</code> a new kind of type: <code>unique symbol</code>s. <code>unique symbol</code>s are subtype of <code>symbol</code>, and are produced only from calling <code>Symbol()</code> or <code>Symbol.for()</code>, or from explicit type annotations. They can only occur on <code>const</code> declarations and <code>readonly static</code> properties, and in order to reference an existing unique symbol type, you'll have to use the <code>typeof</code> operator. Each reference to a <code>unique symbol</code> implies a completely unique identity that's tied to a given declaration. <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Works</span>
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">const</span> Foo<span class="pl-k">:</span> <span style="color: #0000ff">unique</span> <span style="color: #0000ff">symbol</span>;

<span style="color: #148A14">// Error! 'Bar' isn't a constant.</span>
<span style="color: #0000ff">let</span> Bar<span class="pl-k">:</span> <span style="color: #0000ff">unique</span> <span style="color: #0000ff">symbol</span> <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>();

<span style="color: #148A14">// Works - refers to a unique symbol, but its identity is tied to 'Foo'.</span>
<span style="color: #0000ff">let</span> Baz<span class="pl-k">:</span> <span style="color: #0000ff">typeof</span> <span style="color: #267F99">Foo</span> <span class="pl-k">=</span> <span class="pl-smi">Foo</span>;

<span style="color: #148A14">// Also works.</span>
<span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    <span style="color: #0000ff">static</span> <span style="color: #0000ff">readonly</span> StaticSymbol<span class="pl-k">:</span> <span style="color: #0000ff">unique</span> <span style="color: #0000ff">symbol</span> <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>();
}</pre>
  </div> Because each 
  
  <code>unique symbol</code> has a completely separate identity, no two <code>unique symbol</code> types are assignable or comparable to each other. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">const</span> Foo <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>();
<span style="color: #0000ff">const</span> Bar <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>();

<span style="color: #148A14">// Error: can't compare two unique symbols.</span>
<span style="color: #0000ff">if</span> (<span class="pl-smi">Foo</span> <span class="pl-k">===</span> <span class="pl-smi">Bar</span>) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> Other potential use-cases include using symbols for tagged unions. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// ./ShapeKind.ts</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> Circle <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>circle<span class="pl-pds">"</span></span>);
<span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> Square <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>square<span class="pl-pds">"</span></span>);

<span style="color: #148A14">// ./ShapeFun.ts</span>
<span style="color: #0000ff">import</span> <span class="pl-c1">*</span> <span style="color: #0000ff">as</span> <span class="pl-smi">ShapeKind</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>./ShapeKind<span class="pl-pds">"</span></span>;

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Circle</span> {
    kind<span class="pl-k">:</span> <span style="color: #0000ff">typeof</span> <span style="color: #267F99">ShapeKind</span>.<span style="color: #267F99">Circle</span>;
    radius<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Square</span> {
    kind<span class="pl-k">:</span> <span style="color: #0000ff">typeof</span> <span style="color: #267F99">ShapeKind</span>.<span style="color: #267F99">Square</span>;
    sideLength<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">function</span> area(<span class="pl-v">shape</span><span class="pl-k">:</span> <span style="color: #267F99">Circle</span> <span class="pl-k">|</span> <span style="color: #267F99">Square</span>) {
    <span style="color: #0000ff">if</span> (<span class="pl-smi">shape</span>.<span class="pl-smi">kind</span> <span class="pl-k">===</span> <span class="pl-smi">ShapeKind</span>.<span class="pl-smi">Circle</span>) {
        <span style="color: #148A14">// 'shape' has type 'Circle'</span>
        <span style="color: #0000ff">return</span> <span class="pl-c1">Math</span>.<span class="pl-c1">PI</span> <span class="pl-k">*</span> <span class="pl-smi">shape</span>.<span class="pl-smi">radius</span> <span class="pl-k">**</span> <span style="color: #09885A">2</span>;
    }
    <span style="color: #148A14">// 'shape' has type 'Square'</span>
    <span style="color: #0000ff">return</span> <span class="pl-smi">shape</span>.<span class="pl-smi">sideLength</span> <span class="pl-k">**</span> <span style="color: #09885A">2</span>;
}</pre>
  </div>
  
  <h2 id="cleaner-output-in-watch-mode">
    Cleaner output in <code style="color: #a31515;font-size: 29px">--watch</code> mode
  </h2> TypeScript's 
  
  <code>--watch</code> mode now clears the screen after a re-compilation is requested. This can make it much easier to read messages from the current compilation. We'd like to thank both <a href="https://github.com/k0pernikus">Philipp Kretzschmar</a> and <a href="https://github.com/JoshuaKGoldberg">Joshua Goldberg</a> for helping out on this feature! <h2 id="prettier-pretty-output">
    Prettier <code style="color: #a31515;font-size: 29px">--pretty</code> output
  </h2> TypeScript's 
  
  <code>--pretty</code> flag can make error messages easier to read and manage. We have two main improvements in this functionality. First, thanks to <a href="https://github.com/Microsoft/TypeScript/pull/20416">a pull request</a> from <a href="https://github.com/JoshuaKGoldberg">Joshua Goldberg</a> <code>--pretty</code> now uses colors for file names, diagnostic codes, and line numbers. Second, thanks to <a href="https://github.com/Microsoft/TypeScript/pull/20736">a separate pull request</a> by <a href="https://github.com/orta">Orta Therox</a>, file names and positions are formatted in such a way that common terminals (including the one embedded in Visual Studio Code) can allow a Ctrl+Click, Cmd+Click, Alt+Click, etc. to jump to the appropriate location in your editor <h2 id="numeric-separators">
    Numeric Separators
  </h2> TypeScript 2.7 introduces support for ECMAScript's 
  
  <a href="https://github.com/tc39/proposal-numeric-separator">numeric separators</a> proposal. This feature allows users to place underscores (<code>_</code>) in between digits to visually distinguish groups of digits (<a href="https://en.wikipedia.org/wiki/Decimal_separator#Digit_grouping">much like how commas and periods are often used to group numbers</a>). <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Constants</span>
<span style="color: #0000ff">const</span> COULOMB <span class="pl-k">=</span> <span style="color: #09885A">8.957_551_787e9</span>; <span style="color: #148A14">// N-m^2 / C^2</span>
<span style="color: #0000ff">const</span> PLANCK <span class="pl-k">=</span> <span style="color: #09885A">6.626_070_040e-34</span>; <span style="color: #148A14">// J-s</span>
<span style="color: #0000ff">const</span> JENNY <span class="pl-k">=</span> <span style="color: #09885A">867_5309</span>; <span style="color: #148A14">// C-A-L^2</span></pre>
  </div> These separators are also useful for binary and hexadecimal numbers. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> bits <span class="pl-k">=</span> <span style="color: #09885A">0b0010_1010</span>;
<span style="color: #0000ff">let</span> routine <span class="pl-k">=</span> <span style="color: #09885A">0xC0FFEE_F00D_BED</span>;
<span style="color: #0000ff">let</span> martin <span class="pl-k">=</span> <span style="color: #09885A">0xF0_1E_</span></pre>
  </div> Note that, perhaps counterintuitively, numbers in JavaScript are not well-suited to represent credit card and telephone numbers. Strings will act as better representations in such cases. 
  
  <h2 id="fixed-length-tuples">
    Fixed Length Tuples
  </h2> In TypeScript 2.6 and earlier, 
  
  <code>[number, string, string]</code> was considered a subtype of <code>[number, string]</code>. This was motivated by TypeScript's structural nature; the first and second elements of a <code>[number, string, string]</code> are respectively subtypes of the first and second elements of <code>[number, string]</code>, and the "trailing" <code>string</code> type is assignable to the union of element types from <code>[number, string]</code>. However, after examining real world usage of tuples, we noticed that most situations in which this was permitted was typically undesirable. Thanks to a pull request from <a href="https://github.com/tycho01">Tycho Grouwstra</a>, tuple types now encode their arity into the type of their respective <code>length</code> property, and tuples of different arities are no longer assignable to each other. This is accomplished by leveraging numeric literal types, which now allow tuples to be distinct from tuples of different arities. Conceptually, you might consider the type <code>[number, string]</code> to be equivalent to the following declaration of <code>NumStrTuple</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">NumStrTuple</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; {
    <span style="color: #09885A">0</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #09885A">1</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    length<span class="pl-k">:</span> <span style="color: #09885A">2</span>; <span style="color: #148A14">// using the numeric literal type '2'</span>
}</pre>
  </div> Note that this is a breaking change. If you need to resort to the original behavior in which tuples only enforce a minimum size, you can use a similar declaration that does not explicitly define a 
  
  <code>length</code> property, falling back to <code>number</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">MinimumNumStrTuple</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; {
    <span style="color: #09885A">0</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #09885A">1</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}</pre>
  </div>
  
  <h2 id="in-operator-narrowing-and-accurate-instanceof">
    <code style="color: #a31515;font-size: 29px">in</code> operator narrowing and accurate <code style="color: #a31515;font-size: 29px">instanceof</code>
  </h2> TypeScript 2.7 brings two new changes to type narrowing - the ability to get a more specific type for a value by running certain types of checks called "type guards". First, the 
  
  <code>instanceof</code> operator is now leverages the inheritance chain instead of relying on structural compatibility, more accurately reflecting whether how <code>instanceof</code> may behave at runtime. This can help avoid certain complex issues when <code>instanceof</code> narrows from structurally similar (but unrelated) types. Second, thanks to GitHub user <a href="https://github.com/ideahunter">IdeaHunter</a>, the <code>in</code> operator <a href="https://github.com/Microsoft/TypeScript/pull/15256">now acts as a type guard</a>, narrowing out types that don't explicitly declare properties of a given name. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">A</span> { a<span class="pl-k">:</span> <span style="color: #0000ff">number</span> };
<span style="color: #0000ff">interface</span> <span style="color: #267F99">B</span> { b<span class="pl-k">:</span> <span style="color: #0000ff">string</span> };

<span style="color: #0000ff">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">A</span> <span class="pl-k">|</span> <span style="color: #267F99">B</span>) {
    <span style="color: #0000ff">if</span> (<span style="color: #a31515"><span class="pl-pds">"</span>a<span class="pl-pds">"</span></span> <span style="color: #0000ff">in</span> <span class="pl-smi">x</span>) {
        <span style="color: #0000ff">return</span> <span class="pl-smi">x</span>.<span class="pl-smi">a</span>;
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">x</span>.<span class="pl-smi">b</span>;
}</pre>
  </div>
  
  <h2 id="smarter-object-literal-inference">
    Smarter object literal inference
  </h2> There are certain patterns in JavaScript where users will omit properties so that all uses of those properties are effectively 
  
  <code>undefined</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> foo <span class="pl-k">=</span> <span class="pl-smi">someTest</span> <span class="pl-k">?</span> { value: <span style="color: #09885A">42</span> } <span class="pl-k">:</span> {};</pre>
  </div> TypeScript used seek the best common supertype between 
  
  <code>{ value: number }</code> and <code>{}</code>, ending up with <code>{}</code>. While being technically correct, this wasn't very useful. Starting with version 2.7, TypeScript now "normalizes" each object literal's type to account for every property, inserting an optional property of type <code>undefined</code> on each object type, and unioning them together. From the above example, the new type of <code>foo</code> would be <code>{ value: number } | { value?: undefined }</code>. Combined with the ways TypeScript can narrow, this lets us write expressive code that TypeScript can still understand. As another example, take the following: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Has type</span>
<span style="color: #148A14">//  | { a: boolean, aData: number, b?: undefined }</span>
<span style="color: #148A14">//  | { b: boolean, bData: string, a?: undefined }</span>
<span style="color: #0000ff">let</span> bar <span class="pl-k">=</span> <span class="pl-c1">Math</span>.<span class="pl-c1">random</span>() <span class="pl-k">&lt;</span> <span style="color: #09885A">0.5</span> <span class="pl-k">?</span>
    { a: <span style="color: #0000ff">true</span>, aData: <span style="color: #09885A">100</span> } <span class="pl-k">:</span>
    { <span class="pl-smi">b</span>: <span style="color: #0000ff">true</span>, <span class="pl-smi">bData</span>: <span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span> };

<span style="color: #0000ff">if</span> (<span class="pl-smi">bar</span>.<span class="pl-smi">b</span>) {
    <span style="color: #148A14">// TypeScript now knows that 'bar' has the type</span>
    <span style="color: #148A14">//</span>
    <span style="color: #148A14">//   '{ b: boolean, bData: string, a?: undefined }'</span>
    <span style="color: #148A14">//</span>
    <span style="color: #148A14">// so it knows that 'bData' is available.</span>
    <span class="pl-smi">bar</span>.<span class="pl-smi">bData</span>.<span class="pl-c1">toLowerCase</span>()
}</pre>
  </div> Here, TypeScript is able to narrow the type of 
  
  <code>bar</code> based on checking its <code>b</code> property, allowing us to access the <code>bData</code> property. <h2 id="breaking-changes">
    Breaking Changes
  </h2> This release brings some minor breaking changes: 
  
  <ul>
    <li>
      Tuples now have fixed numeric <code>length</code> properties.
    </li>
    <li>
      <code>instanceof</code> and <code>in</code> now have slightly different narrowing behavior.
    </li>
    <li>
      Inferences from generic signatures now use base constraint types of type parameters instead of <code>any</code>.
    </li>
    <li>
      <a href="https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/setSelectionRange" rel="nofollow">The <code>setSelectionRange</code> API</a> now only accepts <code>"forward" | "backward" | "none"</code>.
    </li>
    <li>
      <code>allowSyntheticDefaultImports</code> no longer synthesizes default imports from TypeScript implementation files (i.e. <code>.ts</code> and <code>.tsx</code>).
    </li>
  </ul> Additionally, as mentioned above, users with the 
  
  <code>--strict</code> setting on will automatically be opted in to <code>--strictPropertyInitialization</code> which errors on properties which are not directly initialized on their declarations or in constructor bodies. While easy to opt out of by explicitly turning this check off, your code may be impacted. You can get a detailed look from our list of <a href="https://github.com/Microsoft/TypeScript/issues?q=is%3Aissue+milestone%3A%22TypeScript+2.7%22+label%3A%22Breaking+Change%22+is%3Aclosed">breaking changes issues for TypeScript 2.7</a> on GitHub, or keep track of general breaking changes on our <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">Breaking Changes wiki page</a>. <h2 id="whats-next">
    What's next?
  </h2> We always try to bring more joy to the TypeScript experience. We hope this release continues the tradition of making you more productive and expressive to bring that joy to the core experience. Now that 2.7 is out, we've got some fantastic things in mind for TypeScript 2.8, including 
  
  <a href="https://github.com/Microsoft/TypeScript/pull/21316">conditional types</a>! While plans are still currently in flux, you can keep a close watch on <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our roadmap</a> to get an idea of what's on the TypeScript horizon. Let us know what you think of this release over <a href="http://twitter.com/typescriptlang/" rel="nofollow">on Twitter</a> or in the comments below, and feel free to report issues and suggestions filing <a href="https://github.com/Microsoft/TypeScript/issues">a GitHub issue</a>. Happy Hacking! </article>
</div>