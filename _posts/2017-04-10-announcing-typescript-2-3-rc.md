---
ID: 1035
post_title: Announcing TypeScript 2.3 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-3-rc/
published: true
post_date: 2017-04-10 16:05:30
---
<div style="font-size: 16px">
  The TypeScript 2.3 Release Candidate is here today! This release brings more ECMAScript features, new settings to make starting projects easier, and more. To get started with the release candidate, you can grab it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/2.2.0">through NuGet</a> or over npm through <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript@rc</pre>
  </div> You can also get TypeScript 
  
  <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS-2.3.0-TS-release-dev14update-320170404.3/TypeScript_Dev14Full.exe">for Visual Studio 2015</a> (if you have <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>). Our team is working on supporting Visual Studio 2017 in the near future, with <a href="https://blogs.msdn.microsoft.com/typescript/2017/03/27/typescripts-new-release-cadence/">details available on our previous blog post</a>. Other editor support will be coming with the proper release, but you can follow instructions to enable newer versions of TypeScript in <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text 3</a>. In this post we'll take a closer look at the new <code>--strict</code> option along with async generator and iterator support, but to see a more detailed list of our release, check out <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap#23-may-2017">the TypeScript Roadmap</a>. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/f87b80033d3308ca4e7fc0815d2b5f82#the---strict-option" id="user-content-the---strict-option" class="anchor"></a>The <code style="font-size: 29px">--strict</code> option
  </h2> By default, TypeScript's type system is as lenient as possible to allow users to add types 
  
  <em>gradually</em>. But have you ever started a TypeScript project with all the strictest settings you could think of? While TypeScript has options for enabling different levels of strictness, it's very common to <em>start</em> at the strictest settings so that TypeScript can provide the best experience. The problem with this is that the compiler has grown to have a <em>lot</em> of different options. <code>--noImplicitAny</code>, <code>--strictNullChecks</code>, <code>--noImplicitThis</code>, and <code>--alwaysStrict</code> are just a few of the more common strictness options that you need to remember when starting a new project. Unfortunately if you can't remember these, it just makes TypeScript harder to use. That's why in TypeScript 2.3, we're introducing the <code>--strict</code> flag. The <code>--strict</code> flag enables these common strictness options implicitly. If you ever need to opt out, you can explicitly turn these options off yourself. For example, a <code>tsconfig.json</code> with all <code>--strict</code> options enabled except for <code>--noImplicitThis</code> would look like the following: <div class="highlight highlight-source-ts">
    <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>strict<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>noImplicitThis<span class="pl-pds">"</span></span>: <span style="color: #0000ff">false</span>
    }
}</pre>
  </div> In the future 
  
  <code>--strict</code> may include other strict checks that we believe will benefit all users, but can be manually toggled off by disabling them explicitly (as mentioned above.) <h2>
    <a href="https://gist.github.com/DanielRosenwasser/f87b80033d3308ca4e7fc0815d2b5f82#downlevel-generator--iterator-support" id="user-content-downlevel-generator--iterator-support" class="anchor"></a>Downlevel generator & iterator support
  </h2> Prior to TypeScript 2.3, generators were not supported when targeting ES3 & ES5. This stemmed from the fact that support for generators implied that other parts of the language like 
  
  <code>for</code>-<code>of</code> loops could play well with iterators, which wasn't the case. TypeScript assumed these constructs could only work on arrays when targeting ES3/ES5, and because generalizing the emit would lead to drastic changes in output code. Something as conceptually simple as a <code>for</code>-<code>of</code> loop would have to handle cases that might never come up in practice and could add slight overhead. In TypeScript 2.3, we've put the work in for users to start working with generators. The new <code>--downlevelIteration</code> flag gives users a model where emit can stay simple for most users, and those in need of general iterator & generator support can opt in. As a result, TypeScript 2.3 makes it significantly easier to use libraries like <a href="https://github.com/redux-saga/redux-saga">redux-saga</a>, where support for generators is expected. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/f87b80033d3308ca4e7fc0815d2b5f82#async-generators--iterators" id="user-content-async-generators--iterators" class="anchor"></a>Async generators & iterators
  </h2> With our support for regular generator & iterator support, TypeScript 2.3 brings support for 
  
  <em>async</em> generators and <em>async</em> iterators. You can read more about these features <a href="https://github.com/tc39/proposal-async-iteration">on the TC39 proposal</a>, but we'll try to give a brief explanation and example. Async iterators are an upcoming ECMAScript feature that allows iterators to produce results asynchronously. They can be cleanly consumed from asynchronous functions with a new construct called <em>async <code>for</code> loops</em>. These have the syntax <pre><span style="color: #0000ff">for await</span> (<span style="color: #0000ff">let</span> item <span style="color: #0000ff">of</span> items) {
    <span style="color: #148A14">/*...*/</span>
}</pre>
  
  <em>Async generators</em> are generators which can <code>await</code> at any point. They're declared using a syntax like <pre><span style="color: #0000ff">async function*</span> asyncGenName() {
    <span style="color: #148A14">/*...*/</span>
}</pre> Let's take a quick look at an example that use both of these constructs together. 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Returns a Promise that resolves after a certain amount of time.</span>
<span style="color: #0000ff">function</span> sleep(<span class="pl-v">milliseconds</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>) {
    <span style="color: #0000ff">return</span> <span style="color: #0000ff">new</span> <span style="color: #267F99">Promise</span>&lt;<span style="color: #0000ff">void</span>&gt;(<span class="pl-v">resolve</span> <span class="pl-k">=&gt;</span> {
        <span class="pl-c1">setTimeout</span>(<span class="pl-smi">resolve</span>, <span class="pl-smi">milliseconds</span>);
    });
}

<span style="color: #148A14">// This converts the iterable into an async iterable.</span>
<span style="color: #148A14">// Each element is yielded back with a delay.</span>
<span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span><span style="color: #0000ff">*</span> getItemsReallySlowly&lt;<span style="color: #267F99">T</span>&gt;(<span class="pl-v">items</span><span class="pl-k">:</span> <span style="color: #267F99">Iterable</span>&lt;<span style="color: #267F99">T</span>&gt;) {
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">const</span> item <span style="color: #0000ff">of</span> <span class="pl-smi">items</span>) {
        <span style="color: #0000ff">await</span> <span class="pl-en">sleep</span>(<span style="color: #09885A">500</span>);
        <span style="color: #0000ff">yield</span> <span class="pl-smi">item</span>;
    }
}

<span style="color: #0000ff">async</span> <span style="color: #0000ff">function</span> speakLikeSloth(<span class="pl-v">items</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>[]) {
    <span style="color: #148A14">// Awaits before each iteration until a result is ready.</span>
    <span style="color: #0000ff">for</span> <span style="color: #0000ff">await</span> (<span style="color: #0000ff">const</span> item <span style="color: #0000ff">of</span> <span class="pl-en">getItemsReallySlowly</span>(<span class="pl-smi">items</span>)) {
        <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span class="pl-smi">item</span>);
    }
}

<span class="pl-en">speakLikeSloth</span>(<span style="color: #a31515"><span class="pl-pds">"</span>never gonna give you up never gonna let you down<span class="pl-pds">"</span></span>.<span class="pl-c1">split</span>(<span style="color: #a31515"><span class="pl-pds">"</span> <span class="pl-pds">"</span></span>))</pre>
  </div> Keep in mind that our support for async iterators relies on support for 
  
  <code>Symbol.asyncIterator</code> to exist at runtime. You may need to <a href="https://github.com/zloirock/core-js/blob/v2.4.1/modules/es7.symbol.async-iterator.js">polyfill <code>Symbol.asyncIterator</code></a>, which for simple purposes can be as simple as <div class="highlight highlight-source-ts">
    <pre>(<span style="color: #267F99">Symbol</span> <span style="color: #0000ff">as</span> <span style="color: #0000ff">any</span>).<span class="pl-smi">asyncIterator</span> <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>.<span class="pl-smi">asyncIterator</span> <span class="pl-k">||</span> <span style="color: #267F99">Symbol</span>.<span class="pl-en">from</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Symbol.asyncIterator<span class="pl-pds">"</span></span>);</pre>
  </div> or even 
  
  <div class="highlight highlight-source-ts">
    <pre>(<span style="color: #267F99">Symbol</span> <span style="color: #0000ff">as</span> <span style="color: #0000ff">any</span>).<span class="pl-smi">asyncIterator</span> <span class="pl-k">=</span> <span style="color: #267F99">Symbol</span>.<span class="pl-smi">asyncIterator</span> <span class="pl-k">||</span> <span style="color: #a31515"><span class="pl-pds">"</span>__@@asyncIterator__<span class="pl-pds">"</span></span>;</pre>
  </div> If you're using ES5 and earlier, you'll also need to use the 
  
  <code>--downlevelIteration</code> flag. Finally, your TypeScript <code>lib</code> option will need to include <code>"esnext"</code>. <h2>
    <a href="https://gist.github.com/DanielRosenwasser/f87b80033d3308ca4e7fc0815d2b5f82#enjoy" id="user-content-enjoy" class="anchor"></a>Enjoy!
  </h2> Keep an eye out for the full release of TypeScript 2.3 later this month which will have 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap#23-may-2017">many more features coming</a>. For our Visual Studio 2017 users: as we mentioned above, we're working hard to ensure future TypeScript releases will be available for you soon. We apologize for this inconvenience, but can assure you that a solution will be made available. We appreciate any and all constructive feedback, and welcome you to leave comments below and <a href="https://github.com/Microsoft/TypeScript/issues/new">file issues on GitHub</a> if needed.
</div>