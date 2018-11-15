---
ID: 1105
post_title: Announcing TypeScript 2.5 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-5-rc/
published: true
post_date: 2017-08-17 17:53:38
---
<div style="font-size: 16px">
  <h1>
    Announcing TypeScript 2.5 RC
  </h1> Today we're happy to announce our RC of TypeScript 2.5. To get started with the latest stable version of TypeScript, you can grab it
  
  <span> </span><a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild">through NuGet</a>, or use the following command with npm: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript@rc</pre>
  </div> Visual Studio 2015 users (who have
  
  <span> </span><a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>)<span> </span><a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.5.0-TS-release-dev14update3-20170816.2/TypeScript_Dev14Full.exe">can install TypeScript 2.5 RC from here</a>, and Visual Studio 2017 users using version 15.2 or later will be able to get TypeScript<span> </span><a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.5.0-TS-release-dev14update3-20170816.2/TypeScript_SDK.exe">by simply installing it from here</a>. While TypeScript 2.5 will be available for other editors soon, in the meantime you can configure<span> </span><a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a><span> </span>and<span> </span><a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a><span> </span>to use a newer version.<span> </span><a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a><span> </span>may have different approaches to trying TypeScript 2.5 RC out. Let's dive in and get an overview of what's coming in TypeScript 2.5! <h2>
    <a href="https://gist.github.com/DanielRosenwasser/caa1c02f1c462d78da656bf47ae2dd0c#optional-catch-binding-parameters" id="user-content-optional-catch-binding-parameters" class="anchor"></a>Optional<span> </span><code style="color: #a31515;font-size: 29px">catch</code><span> </span>binding parameters
  </h2> There are times when coding where you expect something might fail by throwing an error, but where you might not care what that error is. For example: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> contents;
<span style="color: #0000ff">try</span> {
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-smi">fs</span>.<span class="pl-en">readFileSync</span>(<span style="color: #a31515"><span class="pl-pds">'</span>.config_file<span class="pl-pds">'</span></span>).<span class="pl-c1">toString</span>(<span style="color: #a31515"><span class="pl-pds">'</span>utf8<span class="pl-pds">'</span></span>);
}
<span style="color: #0000ff">catch</span> (<span class="pl-smi">unusedError</span>) {
    <span style="color: #148A14">// File might not exist, just fall back to some defaults.</span>
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-en">createDefaultContents</span>();
}</pre>
  </div> Notice that
  
  <span> </span><code>unusedError</code><span> </span>is never referenced in the above example. TypeScript 2.5 introduces a late-stage ECMAScript feature to make the<span> </span><code>catch</code><span> </span>binding optional in<span> </span><code>try</code>/<code>catch</code><span> </span>statements. That means we can just omit<span> </span><code>unusedError</code><span> </span>altogether: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">let</span> contents;
<span style="color: #0000ff">try</span> {
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-smi">fs</span>.<span class="pl-en">readFileSync</span>(<span style="color: #a31515"><span class="pl-pds">'</span>.config_file<span class="pl-pds">'</span></span>).<span class="pl-c1">toString</span>(<span style="color: #a31515"><span class="pl-pds">'</span>utf8<span class="pl-pds">'</span></span>);
}
<span style="color: #0000ff">catch</span> {
    <span style="color: #148A14">// File might not exist, just fall back to some defaults.</span>
    <span class="pl-smi">contents</span> <span class="pl-k">=</span> <span class="pl-en">createDefaultContents</span>();
}</pre>
  </div> Thanks to
  
  <span> </span><a href="https://github.com/tinganho">Tingan Ho</a><span> </span>for his contribution on this feature! <h2>
    <a href="https://gist.github.com/DanielRosenwasser/caa1c02f1c462d78da656bf47ae2dd0c#deduplicated-and-redirected-packages" id="user-content-deduplicated-and-redirected-packages" class="anchor"></a>Deduplicated and redirected packages
  </h2> When importing using the
  
  <span> </span><code>Node</code><span> </span>module resolution strategy in TypeScript 2.5, the compiler will now check whether files originate from "identical" packages. If a file originates from a package with a<span> </span><code>package.json</code><span> </span>containing the same<span> </span><code>name</code><span> </span>and<span> </span><code>version</code><span> </span>fields as a previously encountered package, then TypeScript will redirect itself to the top-most package. This helps resolve problems where two packages might contain identical declarations of classes, but which contain<span> </span><code>private</code><span> </span>members that cause them to be structurally incompatible. It also reduces the memory and runtime footprint of the compiler and language service. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/caa1c02f1c462d78da656bf47ae2dd0c#the---preservesymlinks-compiler-flag" id="user-content-the---preservesymlinks-compiler-flag" class="anchor"></a>The<span> </span><code style="color: #a31515;font-size: 29px">--preserveSymlinks</code><span> </span>compiler flag
  </h2> TypeScript 2.5 brings the
  
  <span> </span><code>preserveSymlinks</code><span> </span>flag, which parallels the behavior of<span> </span><a href="https://nodejs.org/api/cli.html#cli_preserve_symlinks">the<span> </span><code>--preserve-symlinks</code><span> </span>flag in Node.js</a>. This flag also exhibits the opposite behavior to Webpack's<span> </span><code>resolve.symlinks</code><span> </span>option (i.e. setting TypeScript's<span> </span><code>preserveSymlinks</code><span> </span>to<span> </span><code>true</code><span> </span>parallels setting Webpack's<span> </span><code>resolve.symlinks</code><span> </span>to<span> </span><code>false</code>, and vice-versa). In this mode, references to modules and packages (e.g.<span> </span><code>import</code>s and<span> </span><code>/// &lt;reference type="..." /&gt;</code><span> </span>directives) are all resolved relative to the location of the symbolic link file, rather than relative to the path that the symbolic link resolves to. For a more concrete example, we'll defer to<span> </span><a href="https://nodejs.org/api/cli.html#cli_preserve_symlinks">the documentation on the Node.js website</a>. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/caa1c02f1c462d78da656bf47ae2dd0c#what-next" id="user-content-what-next" class="anchor"></a>What next?
  </h2> While we'll continue to improve TypeScript before the full 2.5 release, we also have a few other ideas for TypeScript 2.6 that are under way. Some things we'll be working on in the meantime are improvements to TypeScript's
  
  <span> </span><code>--watch</code><span> </span>mode, and integration with other tools that have file-watching functionality. Our aim is to ensure that as codebases grow larger, TypeScript can give lightning-fast turnaround between the time that you save a file, and the time that you can reload your project. You can keep an eye on<span> </span><a href="https://github.com/Microsoft/TypeScript/pull/17669">our pull request for changes to<span> </span><code>--watch</code></a><span> </span>for the work we've done so far. Thanks for reading, and we hope you give our release candidate a try!
</div>