---
ID: 1045
post_title: Announcing TypeScript 2.3
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-3/
published: true
post_date: 2017-04-27 16:05:52
---
<div style="font-size: 16px">
  Today we're excited to bring you our latest release with TypeScript 2.3! For those who aren't familiar, TypeScript is a superset of JavaScript that brings users optional static types and solid tooling. Using TypeScript can help avoid painful bugs people commonly run into when writing JavaScript by type-checking your code. TypeScript can actually report issues without you even saving your file, and leverage the type system to help you write code even faster. This leads to a truly awesome editing experience, giving you time to think about and test the things that really matter. To get started with the latest stable version of TypeScript, you can grab it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/2.3.1">through NuGet</a>, or use the following command with npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> Visual Studio 2015 users (who have 
  
  <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>) <em>as well as</em> Visual Studio 2017 users using <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2017-preview-relnotes">Update 2 Preview</a> will be able to get TypeScript <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.3.1-TS-release-dev14update3-20170425.3/TypeScript_Dev14Full.exe">by simply installing it from here</a>. To elaborate a bit, this also means that Visual Studio 2017 Update 2 will be able to use any newer version of TypeScript <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2017-preview-relnotes#typescript">side-by-side with older ones</a>. For other editors, default support for 2.3 should be coming soon, but you can configure <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">our Sublime Text plugin</a> to pick up whatever version you need. While our <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">What's New in TypeScript</a> Page will give the full scoop, let's dive in to see some of the great new features TypeScript 2.3 brings! <h2 id="type-checking-in-javascript-files-with--ts-check-and---checkjs">
    Type checking in JavaScript files with <code style="color: #a31515;font-size: 29px">// @ts-check</code> and <code style="color: #a31515;font-size: 29px">--checkJs</code>
  </h2> TypeScript has long had an option for gradually migrating your files from JavaScript to TypeScript using the 
  
  <code style="color: #a31515">--allowJs</code> flag; however, one of the common pain-points we heard from JavaScript users was that migrating JavaScript codebases and getting early benefits from TypeScript was difficult. That's why in TypeScript 2.3, we're experimenting with a new "soft" form of checking in <code style="color: #a31515">.js</code> files, which brings many of the advantages of writing TypeScript without actually writing <code style="color: #a31515">.ts</code> files. This new checking mode <a href="https://github.com/Microsoft/TypeScript/wiki/JSDoc-support-in-JavaScript">uses comments</a> to specify types on regular JavaScript declarations. Just like in TypeScript, these annotations are <em>completely optional</em>, and inference will usually pick up the slack from there. But in this mode, your code is still runnable and doesn't need to go through any new transformations. You can try this all out without even needing to touch your current build tools. If you've already got TypeScript installed (<code style="color: #a31515">npm install -g typescript</code>), getting started is easy! First create a <code style="color: #a31515">tsconfig.json</code> in your project's root directory: <div class="highlight highlight-source-json">
    <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>noEmit<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>allowJs<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>
    },
    <span style="color: #a31515"><span class="pl-pds">"</span>include<span class="pl-pds">"</span></span>: [
        <span style="color: #a31515"><span class="pl-pds">"</span>./src/<span class="pl-pds">"</span></span>
    ]
}</pre>
  </div>
  
  <blockquote>
    <span style="font-size: 12pt">Note: We're assuming our files are in <code style="color: #a31515">src</code>. Your folder names might be different.</span>
  </blockquote> Now all you need to do to type-check a file is to add a comment with 
  
  <code style="color: #a31515">// @ts-check</code> to the top. Now run <code style="color: #a31515">tsc</code> from the same folder as your <code style="color: #a31515">tsconfig.json</code> <strong>and that's it</strong>. <div class="highlight highlight-source-js">
    <pre><span style="color: #148A14">// @ts-check</span>

<span style="color: #148A14">/**</span>
<span style="color: #148A14"> * <span style="color: #0000ff">@param</span> <span style="color: #267F99">{string}</span> <span class="pl-smi">input</span></span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">function</span> <span class="pl-en">foo</span>(<span class="pl-smi">input</span>) {
    <span class="pl-smi">input</span>.<span class="pl-en">tolowercase</span>()
<span style="color: #148A14">    //    ~~~~~~~~~~~ Error! Should be toLowerCase</span>
}</pre>
  </div> We just assumed you didn't want to bring TypeScript into your build pipeline at all, but TypeScript is very flexible about how you want to set up your project. Maybe you wanted to have 
  
  <em>all</em> JavaScript files in your project checked with the <code style="color: #a31515">checkJs</code> flag instead of using <code style="color: #a31515">// @ts-check</code> comments. Maybe you wanted TypeScript to also compile down your ES2015+ code while checking it. Here's a <code style="color: #a31515">tsconfig.json</code> that does just that: <div class="highlight highlight-source-json">
    <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>target<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>es5<span class="pl-pds">"</span></span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>module<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>commonjs<span class="pl-pds">"</span></span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>allowJs<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>checkJs<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>outDir<span class="pl-pds">"</span></span>: <span style="color: #a31515"><span class="pl-pds">"</span>./lib<span class="pl-pds">"</span></span>
    },
    <span style="color: #a31515"><span class="pl-pds">"</span>include<span class="pl-pds">"</span></span>: [
        <span style="color: #a31515"><span class="pl-pds">"</span>./src/**/*<span class="pl-pds">"</span></span>
    ]
}</pre>
  </div>
  
  <blockquote>
    <span style="font-size: 12pt">Note: Since TypeScript is creating new files, we had to set <code style="color: #a31515">outDir</code> to another folder like <code style="color: #a31515">lib</code>. That might not be necessary if you use tools like Webpack, Gulp, etc.</span>
  </blockquote> Next, you can 
  
  <a href="https://www.typescriptlang.org/docs/handbook/declaration-files/consumption.html">start using TypeScript declaration files (<code style="color: #a31515">.d.ts</code> files)</a> for any of your favorite libraries and benefit from any new code you start writing. We think you'll be especially happy getting code completion and error checking based on these definitions, and chances are, <a href="https://code.visualstudio.com/docs/languages/javascript#_automatic-type-acquisition">you may've already tried it</a>. This JavaScript checking mode also allows for two other comments in <code style="color: #a31515">.js</code> files: <ol>
    <li>
      <code style="color: #a31515">// @ts-nocheck</code> to disable a file from being checked when <code style="color: #a31515">--checkJs</code> is on
    </li>
    <li>
      <code style="color: #a31515">// @ts-ignore</code> to ignore errors on the following line.
    </li>
  </ol> You might already be thinking of this experience as something similar to linting; however, that doesn't mean we're trying to replace your linter! We see this as something complementary that can run side-by-side with existing tools like 
  
  <a href="http://eslint.org/">ESLint</a> on your JavaScript. Each tool can play to its strengths. If you're already using TypeScript, we're sure you have a JavaScript codebase lying around you can turn this on to quickly catch some real bugs in. But if you're new to TypeScript, we think that this mode will really help show you what TypeScript has to offer without needing to jump straight in or commit. <h2 id="language-server-plugin-support">
    Language server plugin support
  </h2> One of TypeScript's goals is to deliver a state-of-the-art editing experience to the JavaScript world. This experience is something our users have long enjoyed, whether it came to using traditional JavaScript constructs, newer ECMAScript features, or even JSX. However, in the spirit of separation of concerns, TypeScript avoided special-casing certain content such as templates. This was a problem that we'd discussed deeply with the Angular team - we wanted to be able to deliver the same level of tooling to Angular users for their templates as we did in other parts of their code. That included completions, go-to-definition, error reporting, etc. After working closely with the Angular team, we're happy to announce that TypeScript 2.3 officially makes a language server plugin API available. This API allows plugins to augment the regular editing experience that TypeScript already delivers. What all of this means is that you can get an enhanced editing experience for many different workloads. You can see the progress of 
  
  <a href="https://marketplace.visualstudio.com/items?itemName=Angular.ng-template">Angular's Visual Studio Code Plugin here</a> which can greatly enhance the experience for Angular users. But importantly, note here is that this is a <em>general</em> API - that means that a plugin can be written for many different types of content. As an example, there's <em>already</em> a <a href="https://github.com/angelozerr/tslint-language-service">TSLint</a> language server plugin, as well as a <a href="https://github.com/Quramy/ts-graphql-plugin">TypeScript GraphQL language server plugin</a>! <a href="https://github.com/octref/vetur/">The Vetur plugin</a> has also delivered a better TypeScript and JavaScript editing experience within <code style="color: #a31515">.vue</code> files for Vue.js through our language server plugin model. We hope that TypeScript will continue to empower users of different toolsets. If you're interested in providing an enhanced experience for your users, you can check out <a href="https://github.com/RyanCavanaugh/sample-ts-plugin">this example plugin</a>. <h2 id="default-type-arguments">
    Default type arguments
  </h2> To explain this feature, let's take a simplified look at React's component API. A React component will have 
  
  <code style="color: #a31515">props</code> and potentially some <code style="color: #a31515">state</code>. You could encode this like follows: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">Component</span>&lt;<span style="color: #267F99">P</span>, <span style="color: #267F99">S</span>&gt; {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> Here 
  
  <code style="color: #a31515">P</code> is the type of <code style="color: #a31515">props</code> and <code style="color: #a31515">S</code> is the type of <code style="color: #a31515">state</code>. However, much of the time, <code style="color: #a31515">state</code> is never used in a component. In those cases, we can just write the type as <code style="color: #a31515">object</code> or <code style="color: #a31515">{}</code>, but we have to do so explicitly: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">FooComponent</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">React</span>.<span style="color: #267F99">Component</span>&lt;<span style="color: #267F99">FooProps</span>, <span style="color: #0000ff">object</span>&gt; {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> This may not be surprising. It's fairly common for APIs to have some concept of default values for information you don't care about. Enter default type arguments. Default type arguments allow us to free ourselves from thinking of unused generics. In TypeScript 2.3, we can declare 
  
  <code style="color: #a31515">React.Component</code> as follows: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">Component</span>&lt;<span style="color: #267F99">P</span>, <span style="color: #267F99">S</span> = <span style="color: #0000ff">object</span>&gt; {
    <span style="color: #148A14">// ...</span>
}</pre>
  </div> And now whenever we write 
  
  <code style="color: #a31515">Component&lt;FooProps&gt;</code>, we're implicitly writing <code style="color: #a31515">Component&lt;FooProps, object&gt;</code>. Keep in mind that a type parameter's default isn't necessarily the same as its constraint (though the default has to be assignable to the constraint). <h2 id="generator-and-async-generator-support">
    Generator and async generator support
  </h2> Previously, TypeScript didn't support compiling generators or working with iterators. With TypeScript 2.3, it not only supports both, it also brings support for ECMAScript's new 
  
  <em>async</em> generators and <em>async</em> iterators. This is an opt-in feature when using the <code style="color: #a31515">--downlevelIteration</code> flag. You can <a href="https://blogs.msdn.microsoft.com/typescript/2017/04/10/announcing-typescript-2-3-rc/">read more about this on our RC blog post</a>. This functionality means TypeScript more thoroughly supports the latest version of ECMAScript when targeting older versions. It also means that TypeScript can now work well with libraries like <a href="https://github.com/redux-saga/redux-saga">redux-saga</a>. <h2 id="easier-startup-with-better-help-richer-init-and-quicker-strictness">
    Easier startup with better help, richer init, and quicker strictness
  </h2> Another one of the common pain-points we heard from our users was around the difficulty of getting started in general, and of the discoverability of new options. We had found that people were often unaware that TypeScript could work on JavaScript files, or that it could catch nullability errors. We wanted to make TypeScript more approachable, easier to explore, and more helpful at getting you to the most ideal experience. First, TypeScript's 
  
  <code style="color: #a31515">--help</code> output has been improved so that options are grouped by their topics, and more involved/less common options are skipped by default. To get a complete list of options, you can type in <code style="color: #a31515">tsc --help --all</code>. Second, because users are often unaware of the sorts of options that TypeScript <em>does</em> make available, we've decided to take advantage of TypeScript's <code style="color: #a31515">--init</code> output so that potential options are explicitly listed out in comments. As an example, <code style="color: #a31515">tsconfig.json</code> output will look something like the following: <div class="highlight highlight-source-json">
    <pre>{
  <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
    <span style="color: #148A14">/*  Basic  Options */</span>
    <span style="color: #a31515">"target"</span>: <span style="color: #a31515">"es5"</span>,              <span style="color: #148A14">/* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'. */</span>
    <span style="color: #a31515">"module"</span>: <span style="color: #a31515">"commonjs"</span>,         <span style="color: #148A14">/* Specify module code generation: 'commonjs', 'amd', 'system', 'umd' or 'es2015'. */</span>
    <span style="color: #148A14">// "lib": [],                 /* Specify library files to be included in the compilation:  */</span>
    <span style="color: #148A14">// "allowJs": true,           /* Allow javascript files to be compiled. */</span>
    <span style="color: #148A14">// "checkJs": true,           /* Report errors in .js files. */</span>
    <span style="color: #148A14">// "jsx": "preserve",         /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */</span>
    <span style="color: #148A14">// "declaration": true,       /* Generates corresponding '.d.ts' file. */</span>
    <span style="color: #148A14">// "sourceMap": true,         /* Generates corresponding '.map' file. */</span>
    <span style="color: #148A14">// ...</span>
  }
}</pre>
  </div> We believe that changing our 
  
  <code style="color: #a31515">--init</code> output will make it easier to make changes to your <code style="color: #a31515">tsconfig.json</code> down the line, and will make it quicker to discover TypeScript's capabilities. Finally, we've added the <code style="color: #a31515">--strict</code> flag, which enables the following settings <ul>
    <li>
      <code style="color: #a31515">--noImplicitAny</code>
    </li>
    <li>
      <code style="color: #a31515">--strictNullChecks</code>
    </li>
    <li>
      <code style="color: #a31515">--noImplicitThis</code>
    </li>
    <li>
      <code style="color: #a31515">--alwaysStrict</code> (which enforces JavaScript strict mode in all files)
    </li>
  </ul> This 
  
  <code style="color: #a31515">--strict</code> flag represents a set of flags that the TypeScript team believes will lead to the most optimal developer experience in using the language. In fact, all new projects started with <code style="color: #a31515">tsc --init</code> will have <code style="color: #a31515">--strict</code> turned on by default. <h2 id="enjoy">
    Enjoy!
  </h2> You can read up our full 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">what's new in TypeScript</a> page on our Wiki, and <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">read our Roadmap</a> to get a deeper look at what we've done and what's coming in the future! We always appreciate constructive feedback to improve TypeScript however we can. In fact, TypeScript 2.3 was especially driven by feedback from both existing TypeScript users as well as people in the general JavaScript community. If we reached out to you at any point, we'd like to especially thank you for being helpful and willing to make TypeScript a better project for all JavaScript developers. We hope you enjoy TypeScript 2.3, and we hope that it makes coding even easier for you. If it does, consider dropping us a line in the comments below, or spreading the love on Twitter. Thanks for reading up on this new release, and happy hacking!
</div>