---
ID: 1595
post_title: Announcing TypeScript 3.0
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-3-0/
published: true
post_date: 2018-07-30 16:40:37
---
<div style="font-size: 16px;">
  TypeScript 3.0 is here! Today marks a new milestone in the TypeScript journey, serving JavaScript users everywhere.If you're unfamiliar with TypeScript, it's not too late to learn about it now! TypeScript is an extension of JavaScript that aims to bring static types to modern JavaScript. The TypeScript compiler reads in TypeScript code, which has things like type declarations and type annotations, and emits clean readable JavaScript with those constructs transformed and removed. That code runs in any ECMAScript runtime like your favorite browsers and Node.js. At its core, this experience means analyzing your code to catch things like bugs and typos before your users run into them; but it brings more than that. Thanks to all that information and analysis TypeScript can provide a better authoring experience, providing code completion and navigation features like <em>Find all References</em>, <em>Go to Definition</em>, and <em>Rename</em> in your favorite editor. To get started with the language itself, check out <a href="https://www.typescriptlang.org/" rel="nofollow">typescriptlang.org</a> to learn more. And if you want to try TypeScript 3.0 out now, you can get it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a> or via npm by running <div class="highlight highlight-source-shell">
    <pre class="">npm install -g typescript</pre>
  </div> You can also get editor support for 
  
  <ul>
    <li>
      <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/3.0.1-TS-release-dev14update3-20180725.1/TypeScript_SDK.exe" rel="nofollow">Visual Studio 2017</a> (for version 15.2 or later).
    </li>
    <li>
      <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/3.0.1-TS-release-dev14update3-20180725.1/TypeScript_Dev14Full.exe" rel="nofollow">Visual Studio 2015</a> (which requires update 3).
    </li>
    <li>
      For Visual Studio Code <a href="https://code.visualstudio.com/insiders/" rel="nofollow">by installing the Insiders release</a> until the full release provides it.
    </li>
    <li>
      <a href="https://packagecontrol.io/packages/TypeScript" rel="nofollow">Sublime Text 3 via PackageControl</a>.
    </li>
  </ul>
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a> may have different update schedules, but should all have excellent TypeScript support soon as well. <h1 id="the-30-journey">
    The 3.0 Journey
  </h1> When we released TypeScript 2.0, we took a brief look back at how each release leading up to TypeScript 2.0 brought the language to where it is today. Between TypeScript 1.0 and up until 2.0, the language added union types, type guards, modern ECMAScript support, type aliases, JSX support, literal types, and polymorphic 
  
  <code style="color: #a31515;">this</code> types. If we include TypeScript 2.0 with its introduction of non-nullable types, control flow analysis, tagged union support, <code style="color: #a31515;">this</code>-types, and a simplified model around <code style="color: #a31515;">.d.ts</code> file acquisition, that era truly defined the fundamentals of using TypeScript. So what have we done since? What, apart from new ECMAScript features like the long-<em>await</em>-ed <code style="color: #a31515;">async</code>/<code style="color: #a31515;">await</code>, generators, and object rest/spread brought us to TypeScript 3.0? TypeScript 2.1 was a foundational release that introduced a static model for metaprogramming in JavaScript. The key query (<code style="color: #a31515;">keyof</code>), indexed access (<code style="color: #a31515;">T[K]</code>), and mapped object (<code style="color: #a31515;">{ [K in keyof T]: T[K] }</code>) types have been instrumental in better modeling libraries like React, Ember, Lodash, and more. TypeScript 2.2 and 2.3 brought support for mixin patterns, the non-primitive <code style="color: #a31515;">object</code> type, and generic defaults, used by a number of projects like Angular Material and Polymer. TypeScript 2.3 also shipped a feature for fine-grained control of <code style="color: #a31515;">this</code> types that allowed TypeScript to work well with libraries like Vue, and added the <code style="color: #a31515;">checkJs</code> flag to enable type-checking on JavaScript files. TypeScript 2.4 and 2.6 tightened up the story for strict checking on function types, addressing some of the longest-standing feedback about our type system through <code style="color: #a31515;">--strictFunctionTypes</code> which enforced contravariance on parameters. 2.7 continued the trend of strictness with <code style="color: #a31515;">--strictPropertyInitialization</code> checks in classes. TypeScript 2.8 introduced conditional types, a powerful tool for statically expressing decisions based on types, and 2.9 generalized <code style="color: #a31515;">keyof</code> and provided easier imports for types. Which brings us to TypeScript 3.0! Despite the new big number, 3.0 has few breaking changes (meaning it should be <em>very</em> easy to upgrade) and introduces a new flexible and scalable way to structure your projects, powerful new support for operating on parameter lists, new types to enforce explicit checks, better JSX support, an overall better error UX, and much more! <h1 id="whats-new">
    What's New?
  </h1>
  
  <ul>
    <li>
      <a href="#project-references">Project references</a> <ul>
        <li>
          <a href="#build-mode"><code style="color: #a31515;">--build</code> mode</a>
        </li>
        <li>
          <a href="#controlling-output-structure">Controlling output structure</a>
        </li>
        <li>
          <a href="#further-work">Further work</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#tuples-and-parameters">Extracting and spreading parameter lists with tuples</a>
    </li>
    <li>
      <a href="#richer-tuple-types">Richer tuple types</a>
    </li>
    <li>
      <a href="#the-unknown-type">The <code style="color: #a31515;">unknown</code> type</a>
    </li>
    <li>
      <a href="#improved-errors-and-ux">Improved errors and UX</a> <ul>
        <li>
          <a href="#related-error-spans">Related error spans</a>
        </li>
        <li>
          <a href="#Improved-messages-and-elaboration">Improved messages and elaboration</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#default-props-support">Support for <code style="color: #a31515;">defaultProps</code> in JSX</a>
    </li>
    <li>
      <a href="#lib-directives"><code style="color: #a31515;">/// &lt;reference lib="..." /&gt;</code> directives</a>
    </li>
    <li>
      <a href="#editor-productivity">Editor Productivity</a> <ul>
        <li>
          <a href="#named-import-refactorings">Named import refactorings</a>
        </li>
        <li>
          <a href="#jsx-productivity">Closing JSX tag completions and outlining spans</a>
        </li>
        <li>
          <a href="#remove-bogus-code">Quick fixes for unreachable code and unused labels</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#breaking-changes">Breaking changes</a> <ul>
        <li>
          <a href="#unknown-is-a-reserved-type-name"><code style="color: #a31515;">unknown</code> is a reserved type name</a>
        </li>
        <li>
          <a href="#intersecting-with-nullundefined">Intersecting with <code style="color: #a31515;">null</code>/<code style="color: #a31515;">undefined</code> reduces to <code style="color: #a31515;">null</code>/<code style="color: #a31515;">undefined</code> outside of <code style="color: #a31515;">strictNullChecks</code></a>
        </li>
        <li>
          <a href="#api-breaking-changes">API breaking changes</a>
        </li>
      </ul>
    </li>
  </ul>
  
  <h2 id="project-references">
    Project references
  </h2> It's fairly common to have several different build steps for a library or application. Maybe your codebase has a 
  
  <code style="color: #a31515;">src</code> and a <code style="color: #a31515;">test</code> directory. Maybe you have your front-end code in a folder called <code style="color: #a31515;">client</code> with your Node.js back-end code in a folder called <code style="color: #a31515;">server</code>, and each imports code from a <code style="color: #a31515;">shared</code> folder. And maybe you use what's called a "monorepo" and have many many projects which depend on each other in non-trivial ways. One of the biggest features that we've worked on for TypeScript 3.0 is called "project references", and it aims to make working with these scenarios easier. Project references allow TypeScript projects to depend on other TypeScript projects - specifically, allowing <code style="color: #a31515;">tsconfig.json</code> files to reference other <code style="color: #a31515;">tsconfig.json</code> files. Specifying these dependencies makes it easier to split your code into smaller projects, since it gives TypeScript (and tools around it) a way to understand build ordering and output structure. That means things like faster builds that work incrementally, and support for transparently navigating, editing, and refactoring across projects. Since 3.0 lays the foundation and exposes the APIs, any build tool should be able to provide this. <h3 id="whats-it-look-like">
    What's it look like?
  </h3> As a quick example, here's what a 
  
  <code style="color: #a31515;">tsconfig.json</code> with project references looks like: <pre class="lang:default decode:true">// ./src/bar/tsconfig.json
{
    "compilerOptions": {
        // Needed for project references.
        "composite": true,
        "declaration": true,

        // Other options...
        "outDir": "../../lib/bar",
        "strict": true, "module": "esnext", "moduleResolution": "node",
    },
    "references": [
        { "path": "../foo" }
    ]
}
</pre> There are two new fields to notice here: 
  
  <code style="color: #a31515;">composite</code> and <code style="color: #a31515;">references</code>. <code style="color: #a31515;">references</code> simply specifies other <code style="color: #a31515;">tsconfig.json</code> files (or folders immediately containing them). Each reference is currently just an object with a <code style="color: #a31515;">path</code> field, and lets TypeScript know that building the current project requires building that referenced project first. Perhaps equally important is the <code style="color: #a31515;">composite</code> field. The <code style="color: #a31515;">composite</code> field ensures certain options are enabled so that this project can be referenced and built incrementally for any project that depends on it. Being able to intelligently and incrementally rebuild is important, since build speed is one of the reasons you might break up a project in the first place. For example, if project <code style="color: #a31515;">front-end</code> depends on <code style="color: #a31515;">shared</code>, and <code style="color: #a31515;">shared</code> depends on <code style="color: #a31515;">core</code>, our APIs around project references can be used to detect a change in <code style="color: #a31515;">core</code>, but to only rebuild <code style="color: #a31515;">shared</code> if the types (i.e. the <code style="color: #a31515;">.d.ts</code> files) produced by <code style="color: #a31515;">core</code> have changed. That means a change to <code style="color: #a31515;">core</code> doesn't completely force us to rebuild the world. For that reason, setting <code style="color: #a31515;">composite</code> forces the <code style="color: #a31515;">declaration</code> flag to be set as well. <h3 id="build-mode">
    <code style="color: #a31515; font-size: 26px;">--build</code> mode
  </h3> TypeScript 3.0 will provide a set of APIs for project references so that other tools can provide this fast incremental behavior. As an example, gulp-typescript already leverages it! So project references should be able to integrate with your choice of build orchestrators in the future. However, for many simple apps and libraries, it's nice not to need external tools. That's why 
  
  <code style="color: #a31515;">tsc</code> now ships with a new <code style="color: #a31515;">--build</code> flag. <code style="color: #a31515;">tsc --build</code> (or its nickname, <code style="color: #a31515;">tsc -b</code>) takes a set of projects and builds them and their dependencies. When using this new build mode, the <code style="color: #a31515;">--build</code> flag has to be set first, and can be paired with certain other flags: <ul>
    <li>
      <code style="color: #a31515;">--verbose</code>: displays every step of what a build requires
    </li>
    <li>
      <code style="color: #a31515;">--dry</code>: performs a build without emitting files (this is useful with <code style="color: #a31515;">--verbose</code>)
    </li>
    <li>
      <code style="color: #a31515;">--clean</code>: attempts to remove output files given the inputs
    </li>
    <li>
      <code style="color: #a31515;">--force</code>: forces a full non-incremental rebuild for a project
    </li>
  </ul>
  
  <h3 id="controlling-output-structure">
    Controlling output structure
  </h3> One subtle but incredibly useful benefit of project references is logically being able to map your input source to its outputs. If you've ever tried to share TypeScript code between the client and server of your application, you might have run into problems controlling the output structure. For example, if 
  
  <code style="color: #a31515;">client/index.ts</code> and <code style="color: #a31515;">server/index.ts</code> both reference <code style="color: #a31515;">shared/index.ts</code> for the following projects: <pre><code>src
├── client
│   ├── index.ts
│   └── tsconfig.json
├── server
│   ├── index.ts
│   └── tsconfig.json
└── shared
    └── index.ts
</code></pre> ...then trying to build 
  
  <code style="color: #a31515;">client</code> and <code style="color: #a31515;">server</code>, we'll end up with... <pre><code>lib
├── client
│   ├── client
│   │   └── index.js
│   └── shared
│       └── index.js
└── server
    ├── server
    │   └── index.js
    └── shared
        └── index.js
</code></pre> rather than 
  
  <pre><code>lib
├── client
│   └── index.js
├── shared
│   └── index.js
└── server
    └── index.js
</code></pre> Notice that we ended up with a copy of 
  
  <code style="color: #a31515;">shared</code> in both <code style="color: #a31515;">client</code> and <code style="color: #a31515;">server</code>. We unnecessarily spent time building <code style="color: #a31515;">shared</code> twice and introduced an undesirable level of nesting in <code style="color: #a31515;">lib/client/client</code> and <code style="color: #a31515;">lib/server/server</code>. The problem is that TypeScript greedily looks for <code style="color: #a31515;">.ts</code> files and tries to include them in a given compilation. Ideally, TypeScript would understand that these files don't need to be built in the same compilation, and would instead jump to the <code style="color: #a31515;">.d.ts</code> files for type information. Creating a <code style="color: #a31515;">tsconfig.json</code> for <code style="color: #a31515;">shared</code> and using project references does exactly that. It signals to TypeScript that <ol>
    <li>
      <code style="color: #a31515;">shared</code> should be built independently, and that
    </li>
    <li>
      when importing from <code style="color: #a31515;">../shared</code>, we should look for the <code style="color: #a31515;">.d.ts</code> files in its output directory.
    </li>
  </ol> This avoids triggering a double-build, and also avoids accidentally absorbing all the contents of 
  
  <code style="color: #a31515;">shared</code>. <h3 id="further-work">
    Further work
  </h3> To get a deeper understanding of project references and how you can use them, 
  
  <a href="https://github.com/Microsoft/TypeScript/issues/3469#issuecomment-400439520">read up more our issue tracker</a>. In the near future, we'll have documentation on project references and build mode. We're committed to ensuring that other tool authors can support project references, and will continue to improve the editing experience around project references. Our intent is for project references to feel as seamless as authoring code with a single <code style="color: #a31515;">tsconfig.json</code>. If you do end up using project references, we'd appreciate any and all feedback to do just that. <h2 id="tuples-and-parameters">
    Extracting and spreading parameter lists with tuples
  </h2> We often take it for granted, but JavaScript lets us think about parameter lists as first-class values - either by using 
  
  <code style="color: #a31515;">arguments</code> or rest-parameters (e.g. <code style="color: #a31515;">...rest</code>). <div class="highlight highlight-source-js">
    <pre class=""><span style="color: #0000ff;">function</span> <span class="pl-en">call</span>(<span class="pl-smi">fn</span>, <span class="pl-k">...</span><span class="pl-v">args</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(<span class="pl-k">...</span>args);
}</pre>
  </div> Notice here that 
  
  <code style="color: #a31515;">call</code> works on functions of any parameter length. Unlike other languages, JavaScript doesn't force us to define a <code style="color: #a31515;">call0</code>, <code style="color: #a31515;">call1</code>, <code style="color: #a31515;">call2</code>, etc. as follows: <div class="highlight highlight-source-js">
    <pre class=""><span style="color: #0000ff;">function</span> <span class="pl-en">call0</span>(<span class="pl-smi">fn</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>();
}

<span style="color: #0000ff;">function</span> <span class="pl-en">call1</span>(<span class="pl-smi">fn</span>, <span class="pl-smi">param1</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(param1);
}

<span style="color: #0000ff;">function</span> <span class="pl-en">call2</span>(<span class="pl-smi">fn</span>, <span class="pl-smi">param1</span>, <span class="pl-smi">param2</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(param1, param2);
}

<span style="color: #0000ff;">function</span> <span class="pl-en">call3</span>(<span class="pl-smi">fn</span>, <span class="pl-smi">param1</span>, <span class="pl-smi">param2</span>, <span class="pl-smi">param3</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(param1, param2, param3);
}</pre>
  </div> Unfortunately, for a while there wasn't a great well-typed way to express this statically in TypeScript without declaring a finite number of overloads: 
  
  <div class="highlight highlight-source-ts">
    <pre class=""><span style="color: #148a14;">// TODO (billg): 5 overloads should *probably* be enough for anybody?</span>
<span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">T1</span>, <span style="color: #267f99;">T2</span>, <span style="color: #267f99;">T3</span>, <span style="color: #267f99;">T4</span>, <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>, <span class="pl-v">param3</span><span class="pl-k">:</span> <span style="color: #267f99;">T3</span>, <span class="pl-v">param4</span><span class="pl-k">:</span> <span style="color: #267f99;">T4</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>, <span class="pl-v">param3</span><span class="pl-k">:</span> <span style="color: #267f99;">T3</span>, <span class="pl-v">param4</span><span class="pl-k">:</span> <span style="color: #267f99;">T4</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span>
<span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">T1</span>, <span style="color: #267f99;">T2</span>, <span style="color: #267f99;">T3</span>, <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>, <span class="pl-v">param3</span><span class="pl-k">:</span> <span style="color: #267f99;">T3</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>, <span class="pl-v">param3</span><span class="pl-k">:</span> <span style="color: #267f99;">T3</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span>
<span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">T1</span>, <span style="color: #267f99;">T2</span>, <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>, <span class="pl-v">param2</span><span class="pl-k">:</span> <span style="color: #267f99;">T2</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span>
<span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">T1</span>, <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span>;
<span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> () <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-v">param1</span><span class="pl-k">:</span> <span style="color: #267f99;">T1</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span>;
<span style="color: #0000ff;">function</span> call(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff;">any</span>[]) <span class="pl-k">=&gt;</span> <span style="color: #0000ff;">any</span>, <span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #0000ff;">any</span>[]) {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(<span class="pl-k">...</span><span class="pl-smi">args</span>);
}</pre>
  </div> Oof! Another case of death by a thousand overloads! Or at least, as many overloads as our users asked us for. TypeScript 3.0 allows us to better model scenarios like these by now allowing rest parameters to be generic, and inferring those generics as tuple types! Instead of declaring each of these overloads, we can say that the 
  
  <code style="color: #a31515;">...args</code> rest parameter from <code style="color: #a31515;">fn</code> must be a type parameter that extends an array, and then we can re-use that for the <code style="color: #a31515;">...args</code> that <code style="color: #a31515;">call</code> passes: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">TS</span> <span style="color: #0000ff;">extends</span> <span style="color: #0000ff;">any</span>[], <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #267f99;">TS</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #267f99;">TS</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span> {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(<span class="pl-k">...</span><span class="pl-smi">args</span>);
}</pre>
  </div> When we call the 
  
  <code style="color: #a31515;">call</code> function, TypeScript will try to extract the parameter list from whatever we pass to <code style="color: #a31515;">fn</code>, and turn that into a tuple: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">number</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>)<span class="pl-k">:</span> <span style="color: #0000ff;">string</span> {
    <span style="color: #0000ff;">return</span> (<span class="pl-smi">x</span> <span class="pl-k">+</span> <span class="pl-smi">y</span>).<span class="pl-c1">toLowerCase</span>();
}

<span style="color: #148a14;">// The `TS` type parameter is inferred as `[number, string]`</span>
<span class="pl-en">call</span>(<span class="pl-smi">foo</span>, <span style="color: #09885a;">100</span>, <span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>);</pre>
  </div> When TypeScript infers 
  
  <code style="color: #a31515;">TS</code> as <code style="color: #a31515;">[number, string]</code> and we end up re-using <code style="color: #a31515;">TS</code> on the rest parameter of <code style="color: #a31515;">call</code>, the instantiation looks like the following <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> call(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> [<span style="color: #0000ff;">number</span>, <span style="color: #0000ff;">string</span>]) <span class="pl-k">=&gt;</span> <span style="color: #0000ff;">string</span>, <span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> [<span style="color: #0000ff;">number</span>, <span style="color: #0000ff;">string</span>])<span class="pl-k">:</span> <span style="color: #0000ff;">string</span></pre>
  </div> And with TypeScript 3.0, using a tuple in a rest parameter gets 
  
  <em>flattened</em> into the rest of the parameter list! The above boils down to simple parameters with no tuples: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> call(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-v">arg1</span><span class="pl-k">:</span> <span style="color: #0000ff;">number</span>, <span class="pl-v">arg2</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) <span class="pl-k">=&gt;</span> <span style="color: #0000ff;">string</span>, <span class="pl-v">arg1</span><span class="pl-k">:</span> <span style="color: #0000ff;">number</span>, <span class="pl-v">arg2</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>)<span class="pl-k">:</span> <span style="color: #0000ff;">string</span></pre>
  </div> So in addition to catching type errors when we pass in the wrong arguments: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> call&lt;<span style="color: #267f99;">TS</span> <span style="color: #0000ff;">extends</span> <span style="color: #0000ff;">any</span>[], <span style="color: #267f99;">R</span>&gt;(<span class="pl-en">fn</span><span class="pl-k">:</span> (<span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #267f99;">TS</span>) <span class="pl-k">=&gt;</span> <span style="color: #267f99;">R</span>, <span class="pl-k">...</span><span class="pl-v">args</span><span class="pl-k">:</span> <span style="color: #267f99;">TS</span>)<span class="pl-k">:</span> <span style="color: #267f99;">R</span> {
    <span style="color: #0000ff;">return</span> <span class="pl-en">fn</span>(<span class="pl-k">...</span><span class="pl-smi">args</span>);
}

<span class="pl-en">call</span>((<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">number</span>, <span class="pl-v">y</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) <span class="pl-k">=&gt;</span> <span class="pl-smi">y</span>, <span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>, <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>);
<span style="color: #148a14;">//                                ~~~~~~~</span>
<span style="color: #148a14;">// Error! `string` isn't assignable to `number`!</span></pre>
  </div> and inference from other arguments: 
  
  <div class="highlight highlight-source-ts">
    <pre><span class="pl-en">call</span>((<span class="pl-v">x</span>, <span class="pl-v">y</span>) <span class="pl-k">=&gt;</span> { <span style="color: #148a14;">/* .... */</span> }, <span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>, <span style="color: #09885a;">100</span>);
<span style="color: #148a14;">//    ^  ^</span>
<span style="color: #148a14;">// `x` and `y` have their types inferred as `string` and `number` respectively.</span></pre>
  </div> we can also observe the tuple types that these functions infer from the outside: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> tuple&lt;<span style="color: #267f99;">TS</span> <span style="color: #0000ff;">extends</span> <span style="color: #0000ff;">any</span>[]&gt;(<span class="pl-k">...</span><span class="pl-v">xs</span><span class="pl-k">:</span> <span style="color: #267f99;">TS</span>)<span class="pl-k">:</span> <span style="color: #267f99;">TS</span> {
    <span style="color: #0000ff;">return</span> <span class="pl-smi">xs</span>;
}

<span style="color: #0000ff;">let</span> x <span class="pl-k">=</span> <span class="pl-en">tuple</span>(<span style="color: #09885a;">1</span>, <span style="color: #09885a;">2</span>, <span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>); <span style="color: #148a14;">// has type `[number, number, string]`</span></pre>
  </div> There is a subtler point to note though. In order to make all of this work, we needed to expand what tuples could do... 
  
  <h2 id="richer-tuple-types">
    Richer tuple types
  </h2> To make tuples model parameter lists (as we just discussed), we had to rethink tuple types a bit. Before TypeScript 3.0, the best that tuples could model was the order and count of a set of parameters. However, parameter lists aren't just ordered lists of types. For example, parameters at the end can be optional: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148a14;">// Both `y` and `z` are optional here.</span>
<span style="color: #0000ff;">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">boolean</span>, <span class="pl-v">y</span> <span class="pl-k">=</span> <span style="color: #09885a;">100</span>, <span class="pl-v">z</span><span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) {
    <span style="color: #148a14;">// ...</span>
}

<span class="pl-en">foo</span>(<span style="color: #0000ff;">true</span>);
<span class="pl-en">foo</span>(<span style="color: #0000ff;">true</span>, <span style="color: #0000ff;">undefined</span>, <span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>);
<span class="pl-en">foo</span>(<span style="color: #0000ff;">true</span>, <span style="color: #09885a;">200</span>);</pre>
  </div> The last parameter can be a 
  
  <em>rest</em> parameter. <div class="highlight highlight-source-ts">
    <pre><span style="color: #148a14;">// `rest` accepts any number of strings - even none!</span>
<span style="color: #0000ff;">function</span> foo(<span class="pl-k">...</span><span class="pl-v">rest</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>[]) {
    <span style="color: #148a14;">// ...</span>
}

<span class="pl-en">foo</span>();
<span class="pl-en">foo</span>(<span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>);
<span class="pl-en">foo</span>(<span style="color: #a31515;"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>, <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>);</pre>
  </div> And finally, there is one mildly interesting property about parameter lists which is that they can be empty: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148a14;">// Accepts no parameters.</span>
<span style="color: #0000ff;">function</span> foo() {
    <span style="color: #148a14;">// ...</span>
}

<span class="pl-en">foo</span>();</pre>
  </div> So to make it possible for tuples to correspond to parameter lists, we needed to model each of these scenarios. First, tuples now allow trailing optional elements: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148a14;">/**</span>
<span style="color: #148a14;"> * 2D, or potentially 3D, coordinate.</span>
<span style="color: #148a14;"> */</span>
<span style="color: #0000ff;">type</span> <span style="color: #267f99;">Coordinate</span> <span class="pl-k">=</span> [<span style="color: #0000ff;">number</span>, <span style="color: #0000ff;">number</span>, <span style="color: #0000ff;">number</span><span class="pl-k">?</span>];</pre>
  </div> The 
  
  <code style="color: #a31515;">Coordinate</code> type creates a tuple with an optional property named <code style="color: #a31515;">2</code> - the element at index <code style="color: #a31515;">2</code> might not be defined! Interestingly, since tuples use numeric literal types for their <code style="color: #a31515;">length</code> properties, <code style="color: #a31515;">Coordinate</code>'s <code style="color: #a31515;">length</code> property has the type <code style="color: #a31515;">2 | 3</code>. Second, tuples now allow rest elements at the end. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">type</span> <span style="color: #267f99;">OneNumberAndSomeStrings</span> <span class="pl-k">=</span> [<span style="color: #0000ff;">number</span>, ...<span style="color: #0000ff;">string</span>[]];</pre>
  </div> Rest elements introduce some interesting open-ended behavior to tuples. The above 
  
  <code style="color: #a31515;">OneNumberAndSomeStrings</code> type requires its first property to be a <code style="color: #a31515;">number</code>, and permits 0 or more <code style="color: #a31515;">string</code>s. Indexing with an arbitrary <code style="color: #a31515;">number</code> will return a <code style="color: #a31515;">string | number</code> since the index won't be known. Likewise, since the tuple length won't be known, the <code style="color: #a31515;">length</code> property is just <code style="color: #a31515;">number</code>. Of note, when no other elements are present, a rest element in a tuple is identical to itself: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">type</span> <span style="color: #267f99;">Foo</span> <span class="pl-k">=</span> [...<span style="color: #0000ff;">number</span>[]]; <span style="color: #148a14;">// Equivalent to `number[]`.</span></pre>
  </div> Finally, tuples can now be empty! While it's not that useful outside of parameter lists, the empty tuple type can be referenced as 
  
  <code style="color: #a31515;">[]</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">type</span> <span style="color: #267f99;">EmptyTuple</span> <span class="pl-k">=</span> [];</pre>
  </div> As you might expect, the empty tuple has a 
  
  <code style="color: #a31515;">length</code> of <code style="color: #a31515;">0</code> and indexing with a <code style="color: #a31515;">number</code> returns the <code style="color: #a31515;">never</code> type. <h2 id="improved-errors-and-ux">
    Improved errors and UX
  </h2> Over time we've heard more and more demand from our community regarding better error messages. While we're by no means done, we heard you in TypeScript 3.0 and have invested a bit here. 
  
  <h3 id="related-error-spans">
    Related error spans
  </h3> Part of the goal of providing a good error message is also guiding a user towards a way to fix the error, or providing a way to intuit why the error message was given in the first place. Much of the time, there can be a lot of information or multiple reasons an error message might surface. Of those reasons, we might find they come from different parts of the code. Related error spans are a new way to surface that information to users. In TypeScript 3.0, error messages can provide messages on other locations so that users can reason about cause-and-effect of an error. 
  
  <img src="https://msdnshared.blob.core.windows.net/media/2018/07/3.0-related-spans-export-default.png" alt="Using import * as express syntax can cause an error when calling express(). Here, the provided error tells the user not just that the call is invalid, but that it has occurred because of the way the user imported express." /> In some sense, related error messages can give a user not just an explanation, but also breadcrumbs to see where things went wrong. <img src="https://msdnshared.blob.core.windows.net/media/2018/07/3.0-misspelled-related-span1.gif" alt="An error on a potentially misspelled property now also informs the user of where the most likely candidate originated." /> These spans will also appear in the terminal when running <code style="color: #a31515;">tsc</code> with <code style="color: #a31515;">--pretty</code> mode enabled, though our team is still iterating on the UI and would appreciate feedback! <h3 id="improved-messages-and-elaboration">
    Improved messages and elaboration
  </h3> Around TypeScript 2.9, we started investing more in our error messages, and 
  
  <a href="https://github.com/Microsoft/TypeScript/issues/25310">with 3.0 we really tried to tackle a core set of cases</a> that could give a smarter, cleaner, and more accurate error experience. This includes things like picking better types with mismatches in union types, and cutting right to the chase for certain error messages. We believe this effort had paid off and will provide significantly shorter and cleaner error messages. <img src="https://msdnshared.blob.core.windows.net/media/2018/07/3.0-error-messages-vertical.png" /> <img src="https://msdnshared.blob.core.windows.net/media/2018/07/3.0-error-messages-object-literals-vertical-2.png" alt="Error messages for the equivalent code/issue in JSX compared between TypeScript 2.8 and TypeScript 3.0. In TypeScript 3.0, the message is dramatically shorter and has a related span, while still providing context." /> <h2 id="the-unknown-type">
    The <code style="color: #a31515; font-size: 29px;">unknown</code> type
  </h2> The 
  
  <code style="color: #a31515;">any</code> type is the most-capable type in TypeScript - while it encompasses the type of every possible value, it doesn't force us to do any checking before we try to call, construct, or access properties on these values. It also lets us assign values of type <code style="color: #a31515;">any</code> to values that expect any other type. This is mostly useful, but it can be a bit lax. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">let</span> foo<span class="pl-k">:</span> <span style="color: #0000ff;">any</span> <span class="pl-k">=</span> <span style="color: #09885a;">10</span>;

<span style="color: #148a14;">// All of these will throw errors, but TypeScript</span>
<span style="color: #148a14;">// won't complain since `foo` has the type `any`.</span>
<span class="pl-smi">foo</span>.<span class="pl-c1">x</span>.<span class="pl-smi">prop</span>;
<span class="pl-smi">foo</span>.<span class="pl-c1">y</span>.<span class="pl-smi">prop</span>;
<span class="pl-smi">foo</span>.<span class="pl-c1">z</span>.<span class="pl-smi">prop</span>;
<span class="pl-en">foo</span>();
<span style="color: #0000ff;">new</span> <span class="pl-en">foo</span>();
<span class="pl-en">upperCase</span>(<span class="pl-smi">foo</span>);
<span class="pl-smi">foo</span> <span style="color: #a31515;"><span class="pl-pds">`</span>hello world!<span class="pl-pds">`</span></span>;

<span style="color: #0000ff;">function</span> upperCase(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-smi">x</span>.<span class="pl-c1">toUpperCase</span>();
}</pre>
  </div> There are often times where we want to describe the 
  
  <em>least</em>-capable type in TypeScript. This is useful for APIs that want to signal "this can be any value, so you <em>must</em> perform some type of checking before you use it". This forces users to safely introspect returned values. TypeScript 3.0 introduces a new type called <code style="color: #a31515;">unknown</code> that does exactly that. Much like <code style="color: #a31515;">any</code>, any value is assignable <em>to</em> <code style="color: #a31515;">unknown</code>; however, unlike <code style="color: #a31515;">any</code>, <code style="color: #a31515;">unknown</code> is assignable to almost nothing else without a type assertion. You also can't access any properties off of an <code style="color: #a31515;">unknown</code>, nor can you call/construct them. As an example, swapping the above example to use <code style="color: #a31515;">unknown</code> instead of <code style="color: #a31515;">any</code> turns all usages of <code style="color: #a31515;">foo</code> into an error: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">let</span> foo<span class="pl-k">:</span> <span style="color: #0000ff;">unknown</span> <span class="pl-k">=</span> <span style="color: #09885a;">10</span>;

<span style="color: #148a14;">// Since `foo` has type `unknown`, TypeScript</span>
<span style="color: #148a14;">// errors on each of these locations.</span>
<span class="pl-smi">foo</span>.<span class="pl-c1">x</span>.<span class="pl-smi">prop</span>;
<span class="pl-smi">foo</span>.<span class="pl-c1">y</span>.<span class="pl-smi">prop</span>;
<span class="pl-smi">foo</span>.<span class="pl-c1">z</span>.<span class="pl-smi">prop</span>;
<span class="pl-en">foo</span>();
<span style="color: #0000ff;">new</span> <span class="pl-en">foo</span>();
<span class="pl-en">upperCase</span>(<span class="pl-smi">foo</span>);
<span class="pl-smi">foo</span> <span style="color: #a31515;"><span class="pl-pds">`</span>hello world!<span class="pl-pds">`</span></span>;

<span style="color: #0000ff;">function</span> upperCase(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-smi">x</span>.<span class="pl-c1">toUpperCase</span>();
}</pre>
  </div> Instead, we're now forced to either perform checking, or use a type assertion to convince the type-system that we know better. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">let</span> foo<span class="pl-k">:</span> <span style="color: #0000ff;">unknown</span> <span class="pl-k">=</span> <span style="color: #09885a;">10</span>;

<span style="color: #0000ff;">function</span> hasXYZ(<span class="pl-v">obj</span><span class="pl-k">:</span> <span style="color: #0000ff;">any</span>)<span class="pl-k">:</span> <span class="pl-en">obj</span> <span style="color: #0000ff;">is</span> { <span class="pl-en">x</span>: <span class="pl-smi">any</span>, <span class="pl-en">y</span>: <span class="pl-smi">any</span>, <span class="pl-en">z</span>: <span class="pl-smi">any</span> } {
    <span style="color: #0000ff;">return</span> <span class="pl-k">!!</span><span class="pl-smi">obj</span> <span class="pl-k">&&</span>
        <span style="color: #0000ff;">typeof</span> <span class="pl-smi">obj</span> <span class="pl-k">===</span> <span style="color: #a31515;"><span class="pl-pds">"</span>object<span class="pl-pds">"</span></span> <span class="pl-k">&&</span>
        <span style="color: #a31515;"><span class="pl-pds">"</span>x<span class="pl-pds">"</span></span> <span style="color: #0000ff;">in</span> <span class="pl-smi">obj</span> <span class="pl-k">&&</span> <span style="color: #a31515;"><span class="pl-pds">"</span>y<span class="pl-pds">"</span></span> <span style="color: #0000ff;">in</span> <span class="pl-smi">obj</span> <span class="pl-k">&&</span> <span style="color: #a31515;"><span class="pl-pds">"</span>z<span class="pl-pds">"</span></span> <span style="color: #0000ff;">in</span> <span class="pl-smi">obj</span>
}

<span style="color: #148a14;">// Using a user-defined type guard...</span>
<span style="color: #0000ff;">if</span> (<span class="pl-en">hasXYZ</span>(<span class="pl-smi">foo</span>)) {
    <span style="color: #148a14;">// ...we're allowed to access certain properties again.</span>
    <span class="pl-smi">foo</span>.<span class="pl-c1">x</span>.<span class="pl-smi">prop</span>;
    <span class="pl-smi">foo</span>.<span class="pl-c1">y</span>.<span class="pl-smi">prop</span>;
    <span class="pl-smi">foo</span>.<span class="pl-c1">z</span>.<span class="pl-smi">prop</span>;
}

<span style="color: #148a14;">// We can also just convince TypeScript we know what we're doing</span>
<span style="color: #148a14;">// by using a type assertion.</span>
<span class="pl-en">upperCase</span>(<span class="pl-smi">foo</span> <span style="color: #0000ff;">as</span> <span style="color: #0000ff;">string</span>);

<span style="color: #0000ff;">function</span> upperCase(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span>) {
    <span style="color: #0000ff;">return</span> <span class="pl-smi">x</span>.<span class="pl-c1">toUpperCase</span>();
}</pre>
  </div> Note that if you've been using a type like 
  
  <code style="color: #a31515;">{} | null | undefined</code> to achieve similar behavior, <code style="color: #a31515;">unknown</code> usually has more desirable behavior in constructs like conditional types, since conditional types distribute across unions: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">type</span> <span style="color: #267f99;">Arrayify</span>&lt;<span style="color: #267f99;">T</span>&gt; <span class="pl-k">=</span> <span style="color: #267f99;">T</span> <span style="color: #0000ff;">extends</span> <span style="color: #0000ff;">any</span> <span class="pl-k">?</span> <span style="color: #267f99;">Array</span>&lt;<span style="color: #267f99;">T</span>&gt; <span class="pl-k">:</span> <span style="color: #0000ff;">never</span>;

<span style="color: #0000ff;">type</span> <span style="color: #267f99;">A</span> <span class="pl-k">=</span> <span style="color: #267f99;">Arrayify</span>&lt;{} <span class="pl-k">|</span> <span style="color: #0000ff;">null</span> <span class="pl-k">|</span> <span style="color: #0000ff;">undefined</span>&gt;; <span style="color: #148a14;">// null[] | undefined[] | {}[]</span>
<span style="color: #0000ff;">type</span> <span style="color: #267f99;">B</span> <span class="pl-k">=</span> <span style="color: #267f99;">Arrayify</span>&lt;<span style="color: #0000ff;">unknown</span>&gt;;               <span style="color: #148a14;">// unknown[]</span></pre>
  </div>
  
  <h2 id="default-props-support">
    Support for <code style="color: #a31515; font-size: 29px;">defaultProps</code> in JSX
  </h2>
  
  <em>Note: at the time of writing, React's <code style="color: #a31515;">.d.ts</code> files may not yet support this functionality.</em> If you've ever used default initializers in modern TypeScript/JavaScript, you might know how handy they can be for function callers. They give us a useful syntax to let callers use functions more easily by not requiring certain arguments, while letting function authors ensure that their values are always defined in a clean way. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> loudlyGreet(<span class="pl-v">name</span> <span class="pl-k">=</span> <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>) {
    <span style="color: #148a14;">// Thanks to the default initializer, `name` will always have type `string` internally.</span>
    <span style="color: #148a14;">// We don't have to check for `undefined` here.</span>
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515;"><span class="pl-pds">"</span>HELLO<span class="pl-pds">"</span></span>, <span class="pl-smi">name</span>.<span class="pl-c1">toUpperCase</span>());
}

<span style="color: #148a14;">// Externally, `name` is optional, and we can potentially pass `undefined` or omit it entirely.</span>
<span class="pl-en">loudlyGreet</span>();
<span class="pl-en">loudlyGreet</span>(<span style="color: #0000ff;">undefined</span>);</pre>
  </div> In 
  
  <a href="https://reactjs.org/" rel="nofollow">React</a>, a similar concept exists for components and their <code style="color: #a31515;">props</code>. When creating a new element using a component, React looks up a property called <code style="color: #a31515;">defaultProps</code>, to fill in values for <code style="color: #a31515;">props</code> that are omitted. <div class="highlight highlight-source-js-jsx">
    <pre><span style="color: #148a14;">// Some non-TypeScript JSX file</span>

<span style="color: #0000ff;">import</span><span class="pl-k"> *</span><span class="pl-k"> as</span><span class="pl-smi"> React</span><span class="pl-k"> from</span> <span style="color: #a31515;"><span class="pl-pds">"</span>react<span class="pl-pds">"</span></span>;
<span style="color: #0000ff;">import</span><span class="pl-k"> *</span><span class="pl-k"> as</span><span class="pl-smi"> ReactDOM</span><span class="pl-k"> from</span> <span style="color: #a31515;"><span class="pl-pds">"</span>react-dom<span class="pl-pds">"</span></span>;

<span style="color: #0000ff;">export</span> <span style="color: #0000ff;">class</span> <span style="color: #267f99;">Greet</span> <span style="color: #0000ff;">extends</span> <span style="color: #267f99;">React</span><span class="pl-k">.</span><span class="pl-smi">Component</span> {
    <span class="pl-en">render</span>() {
        <span style="color: #0000ff;">const</span> {<span class="pl-smi"> name</span> } <span class="pl-k">=</span><span class="pl-c1"> this</span><span class="pl-k">.</span><span class="pl-smi">props</span>;
<span class="pl-k">        return</span> &lt;<span class="pl-ent">div</span>&gt;Hello $<span class="pl-pse">{</span><span class="pl-s1"><span class="pl-smi">name</span><span class="pl-k">.</span><span class="pl-en">toUpperCase</span>()</span><span class="pl-pse">}</span>!&lt;/<span class="pl-ent">div</span>&gt;;
    }

<span class="pl-k">    static</span> <span class="pl-smi">defaultProps</span> <span class="pl-k">=</span> {
<span class="pl-c1">        <span style="color: #a31515;">name</span>:</span> <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>,
    };
}

<span style="color: #148a14;">//      Notice no `name` attribute was specified!</span>
<span style="color: #148a14;">//                                     vvvvvvvvv</span>
<span style="color: #0000ff;">const</span> <span class="pl-smi">result</span><span class="pl-k"> =</span> <span class="pl-smi">ReactDOM</span><span class="pl-k">.</span><span class="pl-en">renderToString</span>(&lt;<span class="pl-ent"><span class="pl-c1">Greet</span></span> /&gt;);
<span class="pl-c1">console</span><span class="pl-k">.</span><span class="pl-c1">log</span>(<span class="pl-smi">result</span>);</pre>
  </div> Notice that in 
  
  <code style="color: #a31515;">&lt;Greet /&gt;</code>, <code style="color: #a31515;">name</code> didn't have to be specified. When a <code style="color: #a31515;">Greet</code> element is created, <code style="color: #a31515;">name</code> will be initialized with <code style="color: #a31515;">"world"</code> and this code will print <code style="color: #a31515;">&lt;div&gt;Hello world!&lt;/div&gt;</code>. Unfortunately, TypeScript didn't understand that <code style="color: #a31515;">defaultProps</code> had any bearing on JSX invocations. Instead, users would often have to declare properties optional and use non-null assertions inside of <code style="color: #a31515;">render</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">export</span> <span style="color: #0000ff;">interface</span> <span style="color: #267f99;">Props</span> { name<span class="pl-k">?</span><span class="pl-k">:</span> <span style="color: #0000ff;">string</span> }
<span style="color: #0000ff;">export</span> <span style="color: #0000ff;">class</span> <span style="color: #267f99;">Greet</span> <span style="color: #0000ff;">extends</span> <span style="color: #267f99;">React</span>.<span style="color: #267f99;">Component</span>&lt;<span style="color: #267f99;">Props</span>&gt; {
    render() {
        <span style="color: #0000ff;">const</span> { name } <span class="pl-k">=</span> <span style="color: #0000ff;">this</span>.<span class="pl-smi">props</span>;

        <span style="color: #148a14;">// Notice the `!` ------v</span>
        <span style="color: #0000ff;">return</span> &lt;<span class="pl-en">div</span>&gt;<span class="pl-smi">Hello</span> <span class="pl-smi">$</span>{<span class="pl-smi">name</span><span class="pl-k">!</span>.<span class="pl-c1">toUpperCase</span>()}<span class="pl-k">!</span><span class="pl-k">&lt;</span><span class="pl-k">/</span><span class="pl-smi">div</span><span class="pl-k">&gt;</span>;
    }
    <span style="color: #0000ff;">static</span> defaultProps <span class="pl-k">=</span> { name: <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>}
}</pre>
  </div> Or they'd use some hacky type-assertions to fix up the type of the component before exporting it. That's why TypeScript 3.0, the language supports a new type alias in the 
  
  <code style="color: #a31515;">JSX</code> namespace called <code style="color: #a31515;">LibraryManagedAttributes</code>. Despite the long name, this is just a helper type that tells TypeScript what attributes a JSX tag accepts. The short story is that using this general type, we can model React's specific behavior for things like <code style="color: #a31515;">defaultProps</code> and, to some extent, <code style="color: #a31515;">propTypes</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">export</span> <span style="color: #0000ff;">interface</span> <span style="color: #267f99;">Props</span> {
    name<span class="pl-k">:</span> <span style="color: #0000ff;">string</span>
}

<span style="color: #0000ff;">export</span> <span style="color: #0000ff;">class</span> <span style="color: #267f99;">Greet</span> <span style="color: #0000ff;">extends</span> <span style="color: #267f99;">React</span>.<span style="color: #267f99;">Component</span>&lt;<span style="color: #267f99;">Props</span>&gt; {
    render() {
        <span style="color: #0000ff;">const</span> { name } <span class="pl-k">=</span> <span style="color: #0000ff;">this</span>.<span class="pl-smi">props</span>;
        <span style="color: #0000ff;">return</span> &lt;<span class="pl-en">div</span>&gt;<span class="pl-smi">Hello</span> <span class="pl-smi">$</span>{<span class="pl-smi">name</span>.<span class="pl-c1">toUpperCase</span>()}<span class="pl-k">!</span><span class="pl-k">&lt;</span><span class="pl-k">/</span><span class="pl-smi">div</span><span class="pl-k">&gt;</span>;
    }
    <span style="color: #0000ff;">static</span> defaultProps <span class="pl-k">=</span> { name: <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span>}
}

<span style="color: #148a14;">// Type-checks! No type assertions needed!</span>
<span style="color: #0000ff;">let</span> el <span class="pl-k">=</span> &lt;<span style="color: #267f99;">Greet</span> /&gt;</pre>
  </div> Keep in mind that there are some limitations. For 
  
  <code style="color: #a31515;">defaultProps</code> that explicitly specify their type as something like <code style="color: #a31515;">Partial&lt;Props&gt;</code>, or stateless function components (SFCs) whose <code style="color: #a31515;">defaultProps</code> are declared with <code style="color: #a31515;">Partial&lt;Props&gt;</code>, will make <em>all</em> props optional. As a workaround, you can omit the type annotation entirely for <code style="color: #a31515;">defaultProps</code> on a class component (like we did above), or use ES2015 default initializers for SFCs: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">function</span> Greet({ <span class="pl-v">name</span> <span class="pl-k">=</span> <span style="color: #a31515;"><span class="pl-pds">"</span>world<span class="pl-pds">"</span></span> }<span class="pl-k">:</span> <span style="color: #267f99;">Props</span>) {
    <span style="color: #0000ff;">return</span> &lt;<span class="pl-en">div</span>&gt;<span class="pl-smi">Hello</span> <span class="pl-smi">$</span>{<span class="pl-smi">name</span>.<span class="pl-c1">toUpperCase</span>()}<span class="pl-k">!</span><span class="pl-k">&lt;</span><span class="pl-k">/</span><span class="pl-smi">div</span><span class="pl-k">&gt;</span>;
}</pre>
  </div> One last thing to note is that while the support is built into TypeScript, the current 
  
  <code style="color: #a31515;">.d.ts</code> files on DefinitelyTyped are not <em>currently</em> leveraging it - therefore <code style="color: #a31515;">@types/react</code> may not have the change available yet. We are currently waiting on stabilization throughout DefinitelyTyped to ensure that the change is minimally disruptive. <h2 id="lib-directives">
    <code style="color: #a31515; font-size: 29px;">/// &lt;reference lib="..." /&gt;</code> directives
  </h2> One of the issues we've seen in the community is that polyfills - libraries that provide newer APIs in older runtimes - often have their own declaration files (
  
  <code style="color: #a31515;">.d.ts</code> files) that attempt to define those APIs themselves. While this is sometimes fine, these declarations are global, and may provide issues with TypeScript's built-in <code style="color: #a31515;">lib.d.ts</code> depending on users' compiler options like <code style="color: #a31515;">--lib</code> and <code style="color: #a31515;">--target</code>. For example, declarations for <code style="color: #a31515;">core-js</code> might conflict with the built-in <code style="color: #a31515;">lib.es2015.d.ts</code>. To solve this, TypeScript 3.0 provides a new way for files to declare the built-in APIs which they expect to be present using a new reference directive: <code style="color: #a31515;">/// &lt;reference lib="..." /&gt;</code>. For example, a polyfill for ES2015's <code style="color: #a31515;">Promise</code> might now simply contain the lines <div class="highlight highlight-source-ts">
    <pre><span style="color: #148a14;">/// &lt;<span class="pl-ent">reference</span> <span class="pl-e">lib</span><span class="pl-k">=</span><span style="color: #a31515;"><span class="pl-pds">"</span>es2015.promise<span class="pl-pds">"</span></span> /&gt;</span>
<span style="color: #0000ff;">export</span> {};</pre>
  </div> With this comment, even if a TypeScript 3.0 consumer has explicitly used a target that doesn't bring in 
  
  <code style="color: #a31515;">lib.es2015.promise.d.ts</code>, importing the above library will ensure that <code style="color: #a31515;">Promise</code> is present. <h2 id="editor-productivity">
    Editor Productivity
  </h2> For those who are unfamiliar, TypeScript leverages its syntactic and semantic knowledge to provide services for writing code more easily. It acts as the engine for TypeScript and JavaScript underneath editors like Visual Studio, Visual Studio Code, and any other editor with a TypeScript plugin to provide the things users love like code completion, 
  
  <em>Go to Definition</em>, and even quick fixes and refactorings. TypeScript 3.0 continues to deliver here. <h3 id="named-import-refactorings">
    Named import refactorings
  </h3> Occasionally, qualifying every import with the module it came from can be cumbersome. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">import</span> <span class="pl-c1">*</span> <span style="color: #0000ff;">as</span> <span class="pl-smi">dependency</span> <span style="color: #0000ff;">from</span> <span style="color: #a31515;"><span class="pl-pds">"</span>./dependency<span class="pl-pds">"</span></span>;

<span style="color: #148a14;">// look at all this repetition!</span>

<span class="pl-smi">dependency</span>.<span class="pl-en">foo</span>();

<span class="pl-smi">dependency</span>.<span class="pl-en">bar</span>();

<span class="pl-smi">dependency</span>.<span class="pl-en">baz</span>();</pre>
  </div> On the other hand, if we individually import the things we use, we might find that after many uses it's become unclear for new readers where these imports originated from. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">import</span> { <span class="pl-smi">foo</span>, <span class="pl-smi">bar</span>, <span class="pl-smi">baz</span> } <span style="color: #0000ff;">from</span> <span style="color: #a31515;"><span class="pl-pds">"</span>./dependency<span class="pl-pds">"</span></span>;

<span style="color: #148a14;">// way lower in the file...</span>

<span class="pl-en">foo</span>();

<span class="pl-en">bar</span>();

<span class="pl-en">baz</span>();</pre>
  </div> Regardless of which you decide to choose now, you might change your mind later. TypeScript 3.0 provides refactorings so that switch never feels daunting. 
  
  <img src="https://msdnshared.blob.core.windows.net/media/2018/07/namespaceToNamedImports.gif" /> <h3 id="jsx-productivity">
    Closing JSX tag completions and outlining spans
  </h3> TypeScript now provides two new productivity features around JSX: 
  
  <ul>
    <li>
      providing completions for JSX closing tags
    </li>
    <li>
      providing collapsible outlining spans for JSX
    </li>
  </ul>
  
  <img src="https://msdnshared.blob.core.windows.net/media/2018/07/jsxCompletionsAndOutlining.gif" /> <h3 id="remove-bogus-code">
    Quick fixes for unreachable code and unused labels
  </h3> TypeScript will now provide quick fixes to remove any unreachable code, as well as remove unused labels. 
  
  <h2 id="breaking-changes">
    Breaking changes
  </h2> You can always keep an eye on upcoming breaking changes 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">in the language</a> as well as <a href="https://github.com/Microsoft/TypeScript/wiki/API-Breaking-Changes/">in our API</a>. We expect TypeScript 3.0 to have very few impactful breaking changes. Language changes should be minimally disruptive, and most breaks in our APIs are oriented around removing already-deprecated functions. <h3 id="unknown-is-a-reserved-type-name">
    <code style="color: #a31515; font-size: 26px;">unknown</code> is a reserved type name
  </h3> Since 
  
  <code style="color: #a31515;">unknown</code> is a new built-in type, it can no longer be used in type declarations like interfaces, type aliases, or classes. <h3 id="intersecting-with-nullundefined">
    Intersecting with <code style="color: #a31515; font-size: 26px;">null</code>/<code style="color: #a31515; font-size: 26px;">undefined</code> reduces to <code style="color: #a31515; font-size: 26px;">null</code>/<code style="color: #a31515; font-size: 26px;">undefined</code> outside of <code style="color: #a31515; font-size: 26px;">strictNullChecks</code>
  </h3> In the following example, 
  
  <code style="color: #a31515;">A</code> has the type <code style="color: #a31515;">null</code> and <code style="color: #a31515;">B</code> has the type <code style="color: #a31515;">undefined</code> when <code style="color: #a31515;">strictNullChecks</code> is turned off: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff;">type</span> <span class="pl-en">A</span> <span class="pl-k">=</span> { a<span class="pl-k">:</span> <span style="color: #0000ff;">number</span> } <span class="pl-k">&</span> <span style="color: #0000ff;">null</span>;      <span style="color: #148a14;">// null</span>
<span style="color: #0000ff;">type</span> <span class="pl-en">B</span> <span class="pl-k">=</span> { a<span class="pl-k">:</span> <span style="color: #0000ff;">number</span> } <span class="pl-k">&</span> <span style="color: #0000ff;">undefined</span>; <span style="color: #148a14;">// undefined</span></pre>
  </div> This is because TypeScript 3.0 is better at reducing subtypes and supertypes in intersection and union types respectively; however, because 
  
  <code style="color: #a31515;">null</code> and <code style="color: #a31515;">undefined</code> are both considered subtypes of every other type when <code style="color: #a31515;">strictNullChecks</code> is off, an intersection with some object type and either will always reduce to <code style="color: #a31515;">null</code> or <code style="color: #a31515;">undefined</code>. If you were relying on <code style="color: #a31515;">null</code> and <code style="color: #a31515;">undefined</code> to be <a href="https://en.wikipedia.org/wiki/Identity_element" rel="nofollow">"identity" elements</a> under intersections, you should look for a way to use <code style="color: #a31515;">unknown</code> instead of <code style="color: #a31515;">null</code> or <code style="color: #a31515;">undefined</code> wherever they appeared. <h3 id="api-breaking-changes">
    API breaking changes
  </h3>
  
  <ul>
    <li>
      The deprecated internal method <code style="color: #a31515;">LanguageService#getSourceFile</code> has been removed, as it has been deprecated for two years. See <a href="https://github.com/Microsoft/TypeScript/pull/24540">#24540</a>.
    </li>
    <li>
      The deprecated function <code style="color: #a31515;">TypeChecker#getSymbolDisplayBuilder</code> and associated interfaces have been removed. See <a href="https://github.com/Microsoft/TypeScript/pull/25331">#25331</a>. The emitter and node builder should be used instead.
    </li>
    <li>
      The deprecated functions <code style="color: #a31515;">escapeIdentifier</code> and <code style="color: #a31515;">unescapeIdentifier</code> have been removed. Due to changing how the identifier name API worked in general, they have been identity functions for a few releases, so if you need your code to behave the same way, simply removing the calls should be sufficient. Alternatively, the typesafe <code style="color: #a31515;">escapeLeadingUnderscores</code> and <code style="color: #a31515;">unescapeLeadingUnderscores</code> should be used if the types indicate they are required (as they are used to convert to or from branded <code style="color: #a31515;">__String</code> and <code style="color: #a31515;">string</code> types).
    </li>
    <li>
      The <code style="color: #a31515;">TypeChecker#getSuggestionForNonexistentProperty</code>, <code style="color: #a31515;">TypeChecker#getSuggestionForNonexistentSymbol</code>, and <code style="color: #a31515;">TypeChecker#getSuggestionForNonexistentModule</code> methods have been made internal, and are no longer part of our public API. See <a href="https://github.com/Microsoft/TypeScript/pull/25520">#25520</a>.
    </li>
  </ul>
  
  <h1 id="going-forward">
    Going forward
  </h1> TypeScript owes so much of its success to its community. We're indebted to our contributors who've worked on the compiler, the language service, DefinitelyTyped, and tooling integration that leveraged any combination of them. But we're also grateful for our users who've consistently given us the feedback we needed and pushed us to improve. Going forward, we foresee bringing more value to the type system and tooling experience, polishing the existing work on project references, and making TypeScript (both the language and the project) more approachable by whatever means we can. But in addition to that, we want to explore what we can do to empower more tool authors and users in the JavaScript community - to bring value to users who could still get value from using TypeScript even without directly using TypeScript. Keep an eye on 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">our roadmap</a> as these ideas become specifics, and feel free to drop us a line to give us feedback, whether via the comments below, <a href="https://twitter.com/typescriptlang/" rel="nofollow">over Twitter</a>, or <a href="https://github.com/Microsoft/TypeScript/issues/new/choose">by filing an issue</a>. We're always trying to do better. For everyone who's been a part of the TypeScript journey so far - thank you. We look forward to bringing you the best experience we can. And for everyone else, we hope you'll start exploring and loving TypeScript as much as we do. Happy hacking! The TypeScript Team
</div>