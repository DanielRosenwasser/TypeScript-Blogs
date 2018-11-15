---
ID: 1682
post_title: Announcing TypeScript 2.9
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-9-2/
published: true
post_date: 2018-05-31 16:34:58
---
<div style="font-size: 16px">
  Today we're announcing the release of TypeScript 2.9! If you're not familiar with TypeScript, it's a language that adds optional static types to JavaScript. Those static types help make guarantees about your code to avoid typos and other silly errors. They can also provide nice things like code completions and easier project navigation thanks to tooling built around those types. When your code is run through the TypeScript compiler, you're left with clean, readable, and standards-compliant JavaScript code, potentially rewritten to support much older browsers that only support ECMAScript 5 or even ECMAScript 3. If you can't wait any longer, you can download TypeScript via <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">NuGet</a> or by running <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> You can also get editor support for 
  
  <ul>
    <li>
      <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.9.1-TS-release-dev14update3-20180525.2/TypeScript_Dev14Full.exe" rel="nofollow">Visual Studio 2015</a> (requires update 3)
    </li>
    <li>
      <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.9.1-TS-release-dev14update3-20180525.2/TypeScript_SDK.exe" rel="nofollow">Visual Studio 2017</a> (requires 15.2 or later)
    </li>
    <li>
      <a href="https://packagecontrol.io/packages/TypeScript" rel="nofollow">Sublime Text 3 via PackageControl</a>
    </li>
    <li>
      Visual Studio Code with the next release, or <a href="https://code.visualstudio.com/insiders/" rel="nofollow">by trying the Insiders release of VS Code</a>.
    </li>
  </ul>
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a> may have different update schedules, but should all have excellent TypeScript support soon as well. This release brings some great editor features: <ul>
    <li>
      <a href="#rename-move-file">Rename file and move declaration to new file</a>
    </li>
    <li>
      <a href="#unused-span-reporting">Unused span reporting</a>
    </li>
    <li>
      <a href="#convert-getter-setter">Convert property to getter/setter</a>
    </li>
  </ul> And we also have core language/compiler features: 
  
  <ul>
    <li>
      <a href="#import-types"><code style="color: #a31515">import()</code> types</a>
    </li>
    <li>
      <a href="#pretty-by-default"><code style="color: #a31515">--pretty</code> by default</a>
    </li>
    <li>
      <a href="#json-imports">Support for well-typed JSON imports</a>
    </li>
    <li>
      <a href="#type-arguments-tagged-template-strings">Type arguments for tagged template strings</a>
    </li>
    <li>
      <a href="#updated-keyof">Support for symbols and numeric literals in <code style="color: #a31515">keyof</code> and mapped object types</a>
    </li>
  </ul> We also have some minor 
  
  <a href="#breaking-changes">breaking changes</a> that you should keep in mind if upgrading. But otherwise, let's look at what new features come with TypeScript 2.9! <h2>
    Editor features
  </h2> Because TypeScript's language server is built in conjunction with the rest of the compiler, TypeScript can provide consistent cross-platform tooling that can be used on any editor. While we'll dive into language improvements in a bit, it should only take a minute to cover these features which are often the most applicable to users, and, well, fun to see in action! 
  
  <h3>
    <a name="rename-move-file"></a>Rename file and move declaration to new file
  </h3> After much community demand, two extremely useful refactorings are now available! First, this release of TypeScript allows users to move declarations to their own new files. Second, TypeScript 2.9 has functionality to rename files within your project while keeping import paths up-to-date. <video width="100%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2018/05/renameFileAndMoveToNewFile2.mp4">
  
  <br /> Moving two interfaces to their own respective files, and then renaming those files while keeping all reference paths up to date.<br /> </video> While not every editor has implemented these features yet, we expect they'll be more broadly available soon. <h3>
    <a name="unused-span-reporting"></a>Unused span reporting
  </h3> TypeScript provices two lint-like flags: 
  
  <code style="color: #a31515">--noUnusedLocals</code> and <code style="color: #a31515">--noUnusedParameters</code>. These options provide errors when certain declarations are found to be unused; however, while this information is generally useful, errors can be a bit much. TypeScript 2.9 has functionality for editors to surface these as "unused" suggestion spans. Editors are free to display these as they wish. As an example, Visual Studio Code will be displaying these as grayed-out text. <a target="_blank" href="https://camo.githubusercontent.com/7232ea79f660fa515950f4ba1235fa027f1809de/68747470733a2f2f6d73646e7368617265642e626c6f622e636f72652e77696e646f77732e6e65742f6d656469612f323031382f30352f756e7573656453756767657374696f6e732e706e67" rel="noopener"><img src="https://camo.githubusercontent.com/7232ea79f660fa515950f4ba1235fa027f1809de/68747470733a2f2f6d73646e7368617265642e626c6f622e636f72652e77696e646f77732e6e65742f6d656469612f323031382f30352f756e7573656453756767657374696f6e732e706e67" alt="A parameter being grayed out as an unused declaration" /></a> <h3>
    <a name="convert-getter-setter"></a>Convert property to getter/setter
  </h3> Thanks to community contributor 
  
  <a href="https://github.com/Kingwl">Wenlu Wang</a>, TypeScript 2.9 supports converting properties to get- and set- accessors. <video width="65%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2018/05/generateGetterSetter.mp4"><br /> Generating a get- and set-accessor from a class property declaration.<br /> </video> <h2>
    <a name="import-types"></a><code style="color: #a31515;font-size: 29px">import()</code> types
  </h2> One long-running pain-point in TypeScript has been the inability to reference a type in another module, or the type of the module itself, without including an import at the top of the file. In some cases, this is just a matter of convenience - you might not want to add an import at the top of your file just to describe a single type's usage. For example, to reference the type of a module at an arbitrary location, here's what you'd have to write before TypeScript 2.9: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">import</span> <span class="pl-c1">*</span> <span style="color: #0000ff">as</span> <span class="pl-smi">_foo</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>foo<span class="pl-pds">"</span></span>;

<span style="color: #0000ff">export</span> <span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">let</span> foo: <span style="color: #0000ff">typeof</span> <span class="pl-en">_foo</span> = <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>foo<span class="pl-pds">"</span></span>);
}</pre>
  </div> In other cases, there are simply things that you can't achieve today - for example, referencing a type within a module in the global scope is impossible today. This is because a file with any imports or exports is considered a module, so adding an import for a type in a global script file will automatically turn that file into a module, which drastically changes things like scoping rules and strict mode within that file. That's why TypeScript 2.9 is introducing the new 
  
  <code style="color: #a31515">import(...)</code> type syntax. Much like ECMAScript's proposed <code style="color: #a31515">import(...)</code> expressions, import types use the same syntax, and provide a convenient way to reference the type of a module, or the types which a module contains. <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// foo.ts</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">Person</span> {
    name: <span style="color: #0000ff">string</span>;
    age: <span style="color: #0000ff">number</span>;
}

<span style="color: #148A14">// bar.ts</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> greet(<span class="pl-v">p</span>: <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>).<span style="color: #267F99">Person</span>) {
    <span style="color: #0000ff">return</span> <span style="color: #a31515"><span class="pl-pds">`</span></span>
<span style="color: #a31515">        Hello, I'm ${<span class="pl-smi">p</span>.<span class="pl-c1">name</span>}, and I'm ${<span class="pl-smi">p</span>.<span class="pl-smi">age</span>} years old.</span>
<span style="color: #a31515">    <span class="pl-pds">`</span></span>;
}</pre>
  </div> Notice we didn't need to add a top-level import specify the type of 
  
  <code style="color: #a31515">p</code>. We could also rewrite our example from above where we awkwardly needed to reference the type of a module: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> bar() {
    <span style="color: #0000ff">let</span> foo: <span style="color: #0000ff">typeof</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>) = <span style="color: #0000ff">await</span> <span style="color: #0000ff">import</span>(<span style="color: #a31515"><span class="pl-pds">"</span>./foo<span class="pl-pds">"</span></span>);
}</pre>
  </div> Of course, in this specific example 
  
  <code style="color: #a31515">foo</code> could have been inferred, but this might be more useful with something like <a href="https://github.com/Microsoft/TypeScript/wiki/Writing-a-Language-Service-Plugin#setup-and-initialization">the TypeScript language server plugin API</a>. <h2>
    <a name="pretty-by-default"></a><code style="color: #a31515;font-size: 29px">--pretty</code> by default
  </h2> TypeScript's 
  
  <code style="color: #a31515">--pretty</code> mode has been around for a while, and is meant to provide a friendlier console experience. Unfortunately it's been opt-in for fear of breaking changes. However, this meant that users often never knew <code style="color: #a31515">--pretty</code> existed. To minimize breaking changes, we've made <code style="color: #a31515">--pretty</code> the default when TypeScript can reasonably detect that it's printing output to a terminal (or really, whatever Node considers to be a TTY device). Users who want to turn <code style="color: #a31515">--pretty</code> off may do so by specifying <code style="color: #a31515">--pretty false</code> on the command line. Programs that rely on TypeScript's output should adjust the spawned process's TTY options. <h2>
    <a name="json-imports"></a>Support for well-typed JSON imports
  </h2> TypeScript is now able to import JSON files as input files when using the 
  
  <code style="color: #a31515">node</code> strategy for <code style="color: #a31515">moduleResolution</code>. This means you can use <code style="color: #a31515">json</code> files as part of their project, and they'll be well-typed! <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// ./tsconfig.json</span>
{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>module<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>commonjs<span class="pl-pds">"</span></span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>resolveJsonModule<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>esModuleInterop<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>
        <span style="color: #a31515"><span class="pl-pds">"</span>outDir<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>lib<span class="pl-pds">"</span></span>
    },
    <span style="color: #a31515"><span class="pl-pds">"</span>include<span class="pl-pds">"</span></span>: [<span style="color: #a31515"><span class="pl-pds">"</span>src<span class="pl-pds">"</span></span>]
}</pre>
  </div>
  
  <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// ./src/settings.json</span>
{
    <span style="color: #a31515"><span class="pl-pds">"</span>dry<span class="pl-pds">"</span></span>: <span style="color: #0000ff">false</span>,
    <span style="color: #a31515"><span class="pl-pds">"</span>debug<span class="pl-pds">"</span></span>: <span style="color: #0000ff">false</span>
}</pre>
  </div>
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// ./src/foo.ts</span>
<span style="color: #0000ff">import</span> <span class="pl-smi">settings</span> <span style="color: #0000ff">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>./settings.json<span class="pl-pds">"</span></span>;

<span class="pl-smi">settings</span>.<span class="pl-smi">debug</span> === <span style="color: #0000ff">true</span>;  <span style="color: #148A14">// Okay</span>
<span class="pl-smi">settings</span>.<span class="pl-smi">dry</span> === <span style="color: #09885A">2</span>;       <span style="color: #148A14">// Error! Can't compare a `boolean` and `number`</span></pre>
  </div> These JSON files will also carry over to your output directory so that things "just work" at runtime. 
  
  <h2>
    <a name="type-arguments-tagged-template-strings"></a>Type arguments for tagged template strings
  </h2> If you use tagged template strings, you might be interested in some of the improvements in TypeScript 2.9. Most of the time when calling generic functions, TypeScript can infer type arguments. However, there are times where type arguments can't be inferred. For example, one might imagine an API like the following: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">RenderedResult</span> {
    <span style="color: #148A14">// ...</span>
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">TimestampedProps</span> {
    timestamp: <span style="color: #267F99">Date</span>;
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> timestamped&lt;<span style="color: #267F99">OtherProps</span>&gt;(
    <span class="pl-en">component</span>: (<span class="pl-v">props</span>: <span style="color: #267F99">TimestampedProps</span> <span class="pl-k">&</span> <span style="color: #267F99">OtherProps</span>) <span class="pl-k">=&gt;</span> <span style="color: #267F99">RenderedResult</span>):
        (<span class="pl-v">props</span>: <span style="color: #267F99">OtherProps</span>) <span class="pl-k">=&gt;</span> <span style="color: #267F99">RenderedResult</span> {
    <span style="color: #0000ff">return</span> <span class="pl-v">props</span> <span class="pl-k">=&gt;</span> {
        <span style="color: #0000ff">const</span> timestampedProps =
            <span class="pl-c1">Object</span>.<span class="pl-en">assign</span>({}, <span class="pl-smi">props</span>, { timestamp: <span style="color: #0000ff">new</span> <span style="color: #267F99">Date</span>() });
        <span style="color: #0000ff">return</span> <span class="pl-en">component</span>(<span class="pl-smi">timestampedProps</span>);
    }
}</pre>
  </div> Here, let's assume a library where "components" are functions which take objects and return some rendered content. The idea is that 
  
  <code style="color: #a31515">timestamped</code> will take a component that may use a <code style="color: #a31515">timestamp</code> property (from <code style="color: #a31515">TimestampedProps</code>) and some other properties (from <code style="color: #a31515">OtherProps</code>), and return a new component which only takes properties specified in <code style="color: #a31515">OtherProps</code>. Unfortunately there's a problem with inference when using <code style="color: #a31515">timestamped</code> naively: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> createDiv(<span class="pl-v">contents</span>: <span style="color: #0000ff">string</span> | <span style="color: #267F99">RenderedContent</span>): <span style="color: #267F99">RenderedContent</span>;

<span style="color: #0000ff">const</span> TimestampedMessage = <span class="pl-en">timestamped</span>(<span class="pl-v">props</span> <span class="pl-k">=&gt;</span> <span class="pl-en">createDiv</span>(<span style="color: #a31515"><span class="pl-pds">`</span></span>
<span style="color: #a31515">    Message opened at : ${<span class="pl-smi">props</span>.<span class="pl-smi">timestamp</span>}</span>
<span style="color: #a31515">    Message contents<span class="pl-cce">\n</span>${<span class="pl-smi">props</span>.<span class="pl-smi">contents</span>}</span>
<span style="color: #a31515"><span class="pl-pds">`</span></span>));</pre>
  </div> Here, TypeScript infers the wrong type for 
  
  <code style="color: #a31515">props</code> when calling <code style="color: #a31515">timestamped</code> because it can't find any candidates for <code style="color: #a31515">OtherProps</code>. <code style="color: #a31515">OtherProps</code> gets the type <code style="color: #a31515">{}</code>, and <code style="color: #a31515">props</code> is then assigned the type <code style="color: #a31515">TimestampedProps & {}</code> which is undesirable. We can get around this with an explicit annotation on <code style="color: #a31515">props</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">MessageProps</span> {
    contents: <span style="color: #0000ff">string</span>;
}

<span style="color: #148A14">//        Notice this intersection type vvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvvv</span>
<span style="color: #0000ff">const</span> TimestampedMessage = <span class="pl-en">timestamped</span>((<span class="pl-v">props</span>: <span style="color: #267F99">MessageProps</span> <span class="pl-k">&</span> <span style="color: #267F99">TimestampedProps</span>) <span class="pl-k">=&gt;</span> <span style="color: #148A14">/*...*/</span>);</pre>
  </div> But we would prefer not to write as much; the type system already knows 
  
  <code style="color: #a31515">TimestampedProps</code> will be part of the type; it just needs to know what <code style="color: #a31515">OtherProps</code> will be, so we can specify that explicitly. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">MessageProps</span> {
    contents: <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">const</span> TimestampedMessage = <span class="pl-en">timestamped</span>&lt;<span style="color: #267F99">MessageProps</span>&gt;(<span class="pl-v">props</span> <span class="pl-k">=&gt;</span> <span class="pl-en">createDiv</span>(<span style="color: #a31515"><span class="pl-pds">`</span></span>
<span style="color: #a31515">    Message opened at : ${<span class="pl-smi">props</span>.<span class="pl-smi">timestamp</span>.<span class="pl-c1">toLocaleString</span>()}</span>
<span style="color: #a31515">    Message contents<span class="pl-cce">\n</span>${<span class="pl-smi">props</span>.<span class="pl-smi">contents</span>}</span>
<span style="color: #a31515"><span class="pl-pds">`</span></span>));</pre>
  </div> Whew! Great! But what does that have to do with tagged template strings? Well, the point here is that we the users were able to give type arguments when the type system had a hard time figuring things out on our invocations. It's not ideal, but it at least it was possible. But tagged template strings are 
  
  <em>also</em> a type of invocation. Tagged template strings actually invoke functions, but up until TypeScript 2.9, they support type arguments at all. For tagged template strings, this can be useful for libraries that work like <a href="https://www.styled-components.com/" rel="nofollow">styled-components</a>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">StyleProps</span> {
    themeName: <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> styledInput&lt;<span style="color: #267F99">OtherProps</span>&gt;(
    <span class="pl-v">strs</span>: <span style="color: #267F99">TemplateStringsArray</span>, 
    ...<span class="pl-v">fns</span>: ((<span class="pl-v">props</span>: <span style="color: #267F99">OtherProps</span> <span class="pl-k">&</span> <span style="color: #267F99">StyleProps</span>) <span class="pl-k">=&gt;</span> <span style="color: #0000ff">string</span>)[]):
        <span style="color: #267F99">React</span>.<span style="color: #267F99">Component</span>&lt;<span style="color: #267F99">OtherProps</span>&gt;;</pre>
  </div> Similar to the above example, TypeScript would have no way to infer the type of 
  
  <code style="color: #a31515">OtherProps</code> if the functions passed to <code style="color: #a31515">fns</code> were not annotated: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">InputFormProps</span> {
    invalidInput: <span style="color: #0000ff">string</span>;
}

<span style="color: #148A14">// Error! Type 'StyleProps' has no property 'invalidInput'.</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> InputForm = <span class="pl-smi">styledInput</span> <span style="color: #a31515"><span class="pl-pds">`</span></span>
<span style="color: #a31515">    color:</span>
<span style="color: #a31515">        ${({<span class="pl-v">themeName</span>}) <span class="pl-k">=&gt;</span> <span class="pl-smi">themeName</span> === <span class="pl-pds">'</span>dark<span class="pl-pds">'</span> ? <span class="pl-pds">'</span>black<span class="pl-pds">'</span> : <span class="pl-pds">'</span>white<span class="pl-pds">'</span>};</span>
<span style="color: #a31515">    border-color: ${({<span class="pl-v">invalidInput</span>}) <span class="pl-k">=&gt;</span> <span class="pl-smi">invalidInput</span> ? <span class="pl-pds">'</span>red<span class="pl-pds">'</span> : <span class="pl-pds">'</span>black<span class="pl-pds">'</span>};</span>
<span style="color: #a31515"><span class="pl-pds">`</span></span>;</pre>
  </div> TypeScript now 2.9 allows type arguments to be placed on tagged template strings, and makes this just as easy as a regular function call! 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">export</span> <span style="color: #0000ff">interface</span> <span style="color: #267F99">InputFormProps</span> {
    invalidInput: <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">const</span> InputForm = <span style="color: #a31515"><span class="pl-en">styledInput</span>&lt;<span style="color: #267F99">InputFormProps</span>&gt; <span class="pl-pds">`</span></span>
<span style="color: #a31515">    color:</span>
<span style="color: #a31515">        ${({<span class="pl-v">themeName</span>}) <span class="pl-k">=&gt;</span> <span class="pl-smi">themeName</span> === <span class="pl-pds">'</span>dark<span class="pl-pds">'</span> ? <span class="pl-pds">'</span>black<span class="pl-pds">'</span> : <span class="pl-pds">'</span>white<span class="pl-pds">'</span>};</span>
<span style="color: #a31515">    border-color: ${({<span class="pl-v">invalidInput</span>}) <span class="pl-k">=&gt;</span> <span class="pl-smi">invalidInput</span> ? <span class="pl-pds">'</span>red<span class="pl-pds">'</span> : <span class="pl-pds">'</span>black<span class="pl-pds">'</span>};</span>
<span style="color: #a31515"><span class="pl-pds">`</span></span>;</pre>
  </div> In the above example, 
  
  <code style="color: #a31515">themeName</code> and <code style="color: #a31515">invalidInput</code> are both well-typed. TypeScript knows they are both <code style="color: #a31515">string</code>s, and would have told us if we'd misspelled either. <h2>
    <a name="updated-keyof"></a>Support for symbols and numeric literals in <code style="color: #a31515;font-size: 29px">keyof</code> and mapped object types
  </h2> TypeScript's 
  
  <code style="color: #a31515">keyof</code> operator is a useful way to query the property names of an existing type. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Person</span> {
    name: <span style="color: #0000ff">string</span>;
    age: <span style="color: #0000ff">number</span>;
}

<span style="color: #148A14">// Equivalent to the type</span>
<span style="color: #148A14">//  "name" | "age"</span>
<span style="color: #0000ff">type</span> <span style="color: #267F99">PersonPropertiesNames</span> = <span style="color: #0000ff">keyof</span> <span style="color: #267F99">Person</span>;</pre>
  </div> Unfortunately, because 
  
  <code style="color: #a31515">keyof</code> predates TypeScript's ability to reason about <code style="color: #a31515">unique symbol</code> types, <code style="color: #a31515">keyof</code> never recognized symbolic keys. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">const</span> baz = <span class="pl-c1">Symbol</span>(<span style="color: #a31515"><span class="pl-pds">"</span>baz<span class="pl-pds">"</span></span>);

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Thing</span> {
    foo: <span style="color: #0000ff">string</span>;
    bar: <span style="color: #0000ff">number</span>;
    [<span class="pl-smi">baz</span>]: <span style="color: #0000ff">boolean</span>; <span style="color: #148A14">// this is a computed property type</span>
}

<span style="color: #148A14">// Error in TypeScript 2.8 and earlier!</span>
<span style="color: #148A14">// `typeof baz` isn't assignable to `"foo" | "bar"`</span>
<span style="color: #0000ff">let</span> x: <span style="color: #0000ff">keyof</span> <span style="color: #267F99">Thing</span> = <span class="pl-smi">baz</span>;</pre>
  </div> TypeScript 2.9 changes the behavior of 
  
  <code style="color: #a31515">keyof</code> to factor in both unique symbols as well as <code style="color: #a31515">number</code> and numeric literal types. As such, the above example now compiles as expected. <code style="color: #a31515">keyof Thing</code> now boils down to the type <code style="color: #a31515">"foo" | "bar" | typeof baz</code>. With this functionality, mapped object types like <code style="color: #a31515">Partial</code>, <code style="color: #a31515">Required</code>, or <code style="color: #a31515">Readonly</code> also recognize symbolic and numeric property keys, and no longer drop properties named by symbols: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Partial</span>&lt;<span style="color: #267F99">T</span>&gt; = {
    [<span style="color: #267F99">K</span> <span style="color: #0000ff">in</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>]: <span style="color: #267F99">T</span>[<span style="color: #267F99">K</span>]
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Thing</span> {
    foo: <span style="color: #0000ff">string</span>;
    bar: <span style="color: #0000ff">number</span>;
    [<span class="pl-smi">baz</span>]: <span style="color: #0000ff">boolean</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">PartialThing</span> = <span style="color: #267F99">Partial</span>&lt;<span style="color: #267F99">Thing</span>&gt;;

<span style="color: #148A14">// This now works correctly and is equivalent to</span>
<span style="color: #148A14">//</span>
<span style="color: #148A14">//   interface PartialThing {</span>
<span style="color: #148A14">//       foo?: string;</span>
<span style="color: #148A14">//       bar?: number;</span>
<span style="color: #148A14">//       [baz]?: boolean;</span>
<span style="color: #148A14">//   }</span></pre>
  </div> Unfortunately this is a breaking change for any usage where users believed that for any type 
  
  <code style="color: #a31515">T</code>, <code style="color: #a31515">keyof T</code> would always be assignable to a <code style="color: #a31515">string</code>. Because symbol- and numeric-named properties invalidate this assumption, we expect some minor breaks which we believe to be easy to catch. In such cases, there are several possible workarounds. If you have code that's really meant to only operate on string properties, you can use <code style="color: #a31515">Extract&lt;keyof T, string&gt;</code> to remove <code style="color: #a31515">symbol</code> and <code style="color: #a31515">number</code> inputs: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> useKey&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">K</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Extract</span>&lt;<span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>, <span style="color: #0000ff">string</span>&gt;&gt;(<span class="pl-v">obj</span>: <span style="color: #267F99">T</span>, <span class="pl-v">k</span>: <span style="color: #267F99">K</span>) {
    <span style="color: #0000ff">let</span> propName: <span style="color: #0000ff">string</span> = <span class="pl-smi">k</span>;
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> If you have code that's more broadly applicable and can handle more than just 
  
  <code style="color: #a31515">string</code>s, you should be able to substitute <code style="color: #a31515">string</code> with <code style="color: #a31515">string | number | symbol</code>, or use the built-in type alias <code style="color: #a31515">PropertyKey</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> useKey&lt;<span style="color: #267F99">T</span>, <span style="color: #267F99">K</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">keyof</span> <span style="color: #267F99">T</span>&gt;(<span class="pl-v">obj</span>: <span style="color: #267F99">T</span>, <span class="pl-v">k</span>: <span style="color: #267F99">K</span>) {
    <span style="color: #0000ff">let</span> propName: <span style="color: #0000ff">string</span> | <span style="color: #0000ff">number</span> | <span style="color: #0000ff">symbol</span> = <span class="pl-smi">k</span>; 
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> Alternatively, you can revert to the old behavior under the 
  
  <code style="color: #a31515">--keyofStringsOnly</code> compiler flag, but this is meant to be used as a transitionary flag. If you intend on using <code style="color: #a31515">--keyofStringsOnly</code> and migrating off, instead of <code style="color: #a31515">PropertyKey</code>, you can create a type alias on <code style="color: #a31515">keyof any</code>, which is equivalent to <code style="color: #a31515">string | number | symbol</code> under normal circumstances, but becomes <code style="color: #a31515">string</code> when <code style="color: #a31515">--keyofStringsOnly</code> is set. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">KeyofBase</span> = <span style="color: #0000ff">keyof</span> <span style="color: #0000ff">any</span>;</pre>
  </div>
  
  <h2>
    <a name="breaking-changes"></a>Breaking changes
  </h2>
  
  <h3>
    <code style="color: #a31515;font-size: 29px">keyof</code> types include symbolic/numeric properties
  </h3> As mentioned above, 
  
  <code style="color: #a31515">keyof</code> types (also called "key query types") now include names that are <code style="color: #a31515">symbol</code>s and <code style="color: #a31515">number</code>s, which can break some code that assumes <code style="color: #a31515">keyof T</code> is assignable to <code style="color: #a31515">string</code>. You can correct your code's assumptions, or revert to the old behavior by using the <code style="color: #a31515">--keyofStringsOnly</code> compiler option: <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// tsconfig.json</span>
{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>keyofStringsOnly<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>
    }
}</pre>
  </div>
  
  <h3>
    <code style="color: #a31515;font-size: 29px">--pretty</code> on by default
  </h3> Also mentioned above, 
  
  <code style="color: #a31515">--pretty</code> is now turned on by default, though this may be a breaking change for some workflows. <h3>
    Trailing commas not allowed on rest parameters
  </h3> Trailing commas can no longer occur after 
  
  <code style="color: #a31515">...rest</code>-parameters, as in the following. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> pushElement(
        <span class="pl-v">foo</span>: <span style="color: #0000ff">number</span>,
        <span class="pl-v">bar</span>: <span style="color: #0000ff">string</span>,
        ...<span class="pl-v">rest</span>: <span style="color: #0000ff">any</span>[], <span style="color: #148A14">// error!</span>
    ) {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> This break was added for conformance with ECMAScript, as trailing commas are not allowed to follow rest parameters in the specification. Trailing commas should simply be removed following this syntax. 
  
  <h3>
    Unconstrained type parameters are no longer assignable to <code style="color: #a31515">object</code> in <code style="color: #a31515">strictNullChecks</code>
  </h3> The following code now errors: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> f&lt;<span style="color: #267F99">T</span>&gt;(<span class="pl-v">x</span>: <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> y: <span style="color: #0000ff">object</span> | <span style="color: #0000ff">null</span> | <span style="color: #0000ff">undefined</span> = <span class="pl-smi">x</span>;
}</pre>
  </div> Since generic type parameters can be substituted with any primitive type, this is a precaution TypeScript has added under 
  
  <code style="color: #a31515">strictNullChecks</code>. To fix this, you can add a constraint of <code style="color: #a31515">object</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// We can add an upper-bound constraint here.</span>
<span style="color: #148A14">//           vvvvvvvvvvvvvv</span>
<span style="color: #0000ff">function</span> f&lt;<span style="color: #267F99">T</span> <span style="color: #0000ff">extends</span> <span style="color: #0000ff">object</span>&gt;(<span class="pl-v">x</span>: <span style="color: #267F99">T</span>) {
    <span style="color: #0000ff">const</span> y: <span style="color: #0000ff">object</span> | <span style="color: #0000ff">null</span> | <span style="color: #0000ff">undefined</span> = <span class="pl-smi">x</span>;
}</pre>
  </div>
  
  <h3>
    <code style="color: #a31515;font-size: 29px">never</code> can no longer be iterated over
  </h3> Values of type 
  
  <code style="color: #a31515">never</code> can no longer be iterated over, which may catch a good class of bugs. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> foo: <span style="color: #0000ff">never</span>;
<span style="color: #0000ff">for</span> (<span style="color: #0000ff">let</span> prop <span style="color: #0000ff">in</span> <span class="pl-smi">foo</span>) {
    <span style="color: #148A14">// Error! `foo` has type `never`.</span>
}</pre>
  </div> Users can avoid this behavior by using a type assertion to cast to the type 
  
  <code style="color: #a31515">any</code> (i.e. <code style="color: #a31515">foo as any</code>). <h2>
    What's next?
  </h2> We hope you're as excited about the improvements to TypeScript 2.9 as we are - but save some excitement for TypeScript 3.0, where we're aiming to deliver an experience around project-to-project references, a new 
  
  <code style="color: #a31515">unknown</code> type, a stricter <code style="color: #a31515">any</code> type, and more! As always, you can keep an eye on <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">the TypeScript roadmap</a> to see what we're working on for our next release (as well as anything we didn't get the chance to mention in this blog post for this release). We also have nightly releases so you can try things out on your machine and give us your feedback early on. You can even try installing it now (<code style="color: #a31515">npm install -g typescript@next</code>) and play around with the new <code style="color: #a31515">unknown</code> type. Let us know what you think of this release over <a href="http://twitter.com/typescriptlang/" rel="nofollow">on Twitter</a> or in the comments below, and feel free to report issues and suggestions filing <a href="https://github.com/Microsoft/TypeScript/issues">a GitHub issue</a>. Happy Hacking!
</div>