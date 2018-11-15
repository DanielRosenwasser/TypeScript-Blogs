---
ID: 1245
post_title: Announcing TypeScript 2.7 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-7-rc/
published: true
post_date: 2018-01-17 18:14:41
---
<div id="readme" class="readme blob instapaper_body">
  <article class="markdown-body entry-content">Today we're publishing the Release Candidate of TypeScript 2.7. To get started with the RC, you can access it <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild" rel="nofollow">through NuGet</a>, or use npm with the following command: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript@rc</pre>
  </div> Visual Studio 2015 users (who have 
  
  <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs" rel="nofollow">Update 3</a>) can install TypeScript 2.7 RC <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.7.0-TS-release-dev14update3-20180110.2/TypeScript_Dev14Full.exe" rel="nofollow">from here</a>, and Visual Studio 2017 users using version 15.2 or later will be able to get TypeScript by simply <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.7.0-TS-release-dev14update3-20180110.2/TypeScript_SDK.exe" rel="nofollow">installing it from here</a>. You can also get the Release Candidate working with <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions" rel="nofollow">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a>. While we have many new features and fixes, we have a few highlights for the RC we think are especially important. <h2 id="definite-assignment-checks-for-class-properties">
    Definite Assignment Checks for Class Properties
  </h2> TypeScript 2.7 introduces a new flag called 
  
  <code style="color: #a31515">--strictPropertyInitialization</code>. This flag performs checks to ensure that each instance property of a class gets initialized in the constructor body, or by a property initializer. For example <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    foo<span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    bar <span class="pl-k">=</span> <span style="color: #a31515"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>;
    baz<span class="pl-k">:</span> <span style="color: #0000ff">boolean</span>;
<span style="color: #148A14">//  ~~~</span>
<span style="color: #148A14">//  Error! Property 'baz' has no initializer and is not definitely assigned in the constructor.</span>
    <span style="color: #0000ff">constructor</span>() {
        <span style="color: #0000ff">this</span>.<span class="pl-smi">foo</span> <span class="pl-k">=</span> <span style="color: #09885A">42</span>;
    }
}</pre>
  </div> In the above, if we truly meant for 
  
  <code style="color: #a31515">baz</code> to potentially be <code style="color: #a31515">undefined</code>, we should have declared it with the type <code style="color: #a31515">boolean | undefined</code>. There are certain scenarios where properties can be initialized indirectly (perhaps by a helper method or dependency injection library), in which case you can use the new <em>definite assignment assertion modifiers</em> for your properties. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    <span class="pl-smi">foo</span><span class="pl-k">!</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #148A14">// ^</span>
    <span style="color: #148A14">// Notice this '!' modifier.</span>
    <span style="color: #148A14">// This is the "definite assignment assertion"</span>

    <span style="color: #0000ff">constructor</span>() {
        <span style="color: #0000ff">this</span>.<span class="pl-en">initialize</span>();
    }

    initialize() {
        <span style="color: #0000ff">this</span>.<span class="pl-smi">foo</span> <span class="pl-k">=</span> <span style="color: #09885A">0</span>;
    }
}</pre>
  </div> Keep in mind that 
  
  <code style="color: #a31515">--strictPropertyInitialization</code> will be turned on along with other <code style="color: #a31515">--strict</code> mode flags, which can impact your project. You can set the <code style="color: #a31515">strictPropertyInitialization</code> setting to <code style="color: #a31515">false</code> in your <code style="color: #a31515">tsconfig.json</code>'s <code style="color: #a31515">compilerOptions</code>, or <code style="color: #a31515">--strictPropertyInitialization false</code> on the command line to turn off this checking. <h2 id="fixed-length-tuples">
    Fixed Length Tuples
  </h2> In TypeScript 2.6 and earlier, 
  
  <code style="color: #a31515">[number, string, string]</code> was considered a subtype of <code style="color: #a31515">[number, string]</code>. This was motivated by TypeScript's structural nature; the first and second elements of a <code style="color: #a31515">[number, string, string]</code> are respectively subtypes of the first and second elements of <code style="color: #a31515">[number, string]</code>. However, after examining real world usage of tuples, we noticed that most situations in which this was permitted was typically undesirable. Thanks to a pull request from <a href="https://github.com/tycho01">Tycho Grouwstra</a>, tuple types now encode their arity into the type of their respective <code style="color: #a31515">length</code> property, and tuples of different arities are no longer assignable to each other. This is accomplished by leveraging numeric literal types, which now allow tuples to be distinct from tuples of different arities. Conceptually, you might consider the type <code style="color: #a31515">[number, string]</code> to be equivalent to the following declaration of <code style="color: #a31515">NumStrTuple</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">NumStrTuple</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; {
    <span style="color: #09885A">0</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #09885A">1</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
    length<span class="pl-k">:</span> <span style="color: #09885A">2</span>; <span style="color: #148A14">// using the numeric literal type '2'</span>
}</pre>
  </div> Note that this is a breaking change. If you need to resort to the original behavior in which tuples only enforce a minimum size, you can use a similar declaration that does not explicitly define a 
  
  <code style="color: #a31515">length</code> property, falling back to <code style="color: #a31515">number</code>. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">MinimumNumStrTuple</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Array</span>&lt;<span style="color: #0000ff">number</span> <span class="pl-k">|</span> <span style="color: #0000ff">string</span>&gt; {
    <span style="color: #09885A">0</span><span class="pl-k">:</span> <span style="color: #0000ff">number</span>;
    <span style="color: #09885A">1</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>;
}</pre>
  </div>
  
  <h2>
    Improved narrowing for <code style="color: #a31515;font-size: 29px">in</code> and <code style="color: #a31515;font-size: 29px">instanceof</code>
  </h2> Thanks to GitHub user 
  
  <a href="https://github.com/ideahunter">IdeaHunter</a>, the <code style="color: #a31515">in</code> operator now acts as a narrowing expression for types, narrowing out types that don't explicitly declare properties of a given name. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">A</span> { a<span class="pl-k">:</span> <span style="color: #0000ff">number</span> };
<span style="color: #0000ff">interface</span> <span style="color: #267F99">B</span> { b<span class="pl-k">:</span> <span style="color: #0000ff">string</span> };

<span style="color: #0000ff">function</span> foo(<span class="pl-v">x</span><span class="pl-k">:</span> <span style="color: #267F99">A</span> <span class="pl-k">|</span> <span style="color: #267F99">B</span>) {
    <span style="color: #0000ff">if</span> (<span style="color: #a31515"><span class="pl-pds">"</span>a<span class="pl-pds">"</span></span> <span style="color: #0000ff">in</span> <span class="pl-smi">x</span>) {
        <span style="color: #0000ff">return</span> <span class="pl-smi">x</span>.<span class="pl-smi">a</span>;
    }
    <span style="color: #0000ff">return</span> <span class="pl-smi">x</span>.<span class="pl-smi">b</span>;
}</pre>
  </div> Furthermore, the 
  
  <code style="color: #a31515">instanceof</code> operator is now leverages the inheritance chain instead of relying on structural compatibility, more accurately reflecting whether how <code style="color: #a31515">instanceof</code> may behave at runtime. <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Error! </span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">class</span> <span style="color: #267F99">C</span> {
    foo <span class="pl-k">=</span> <span style="color: #09885A">1</span>;
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">class</span> <span style="color: #267F99">D</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">C</span> {
    bar <span class="pl-k">=</span> <span style="color: #09885A">2</span>;
}

<span style="color: #0000ff">export</span> <span style="color: #0000ff">class</span> <span style="color: #267F99">E</span> {
    foo <span class="pl-k">=</span> <span style="color: #09885A">3</span>;
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> x<span class="pl-k">:</span> <span style="color: #267F99">C</span> <span class="pl-k">|</span> <span style="color: #267F99">D</span> <span class="pl-k">|</span> <span style="color: #267F99">E</span>;

<span style="color: #0000ff">if</span> (<span class="pl-smi">x</span> <span style="color: #0000ff">instanceof</span> <span class="pl-c1">E</span>) {
    <span class="pl-smi">x</span> <span style="color: #148A14">// 'E', but previously 'D | E'</span>
}
<span style="color: #0000ff">else</span> {
    <span class="pl-smi">x</span> <span style="color: #148A14">// 'C | D', but previously 'C'</span>
}</pre>
  </div> While we've witnessed positive changes in most codebases, be aware that any change in inference and narrowing can impact your compilation. Changes to type declarations, as well as type assertions, may be necessary. 
  
  <h2 id="breaking-changes">
    Breaking Changes
  </h2> This release brings some minor breaking changes: 
  
  <ul>
    <li>
      Tuples now have fixed numeric <code style="color: #a31515">length</code> properties.
    </li>
    <li>
      <code style="color: #a31515">instanceof</code> and <code style="color: #a31515">in</code> now have slightly different narrowing behavior.
    </li>
    <li>
      Inferences from generic signatures now use base constraint types of type parameters instead of <code style="color: #a31515">any</code>.
    </li>
    <li>
      <a href="https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement/setSelectionRange" rel="nofollow">The <code style="color: #a31515">setSelectionRange</code> API</a> now only accepts <code style="color: #a31515">"forward" | "backward" | "none"</code>.
    </li>
    <li>
      <code style="color: #a31515">allowSyntheticDefaultImports</code> no longer synthesizes default imports from TypeScript implementation files (i.e. <code style="color: #a31515">.ts</code> and <code style="color: #a31515">.tsx</code>).
    </li>
  </ul> Additionally, as mentioned above, users with the 
  
  <code style="color: #a31515">--strict</code> setting on will automatically be opted in to <code style="color: #a31515">--strictPropertyInitialization</code> which errors on properties which are not directly initialized on their declarations or in constructor bodies. While easy to opt out of by explicitly turning this check off, your code may be impacted. You can get a detailed look from our list of <a href="https://github.com/Microsoft/TypeScript/issues?q=is%3Aissue+milestone%3A%22TypeScript+2.7%22+label%3A%22Breaking+Change%22+is%3Aclosed">breaking changes issues for TypeScript 2.7</a> on GitHub, or keep track of general breaking changes on our <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">Breaking Changes wiki page</a>. <h2 id="whats-next">
    What's next?
  </h2> A more comprehensive list of this release, as well as our future plans, can be found 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">on our roadmap</a>. We anticipate the full release of TypeScript 2.7 in the next few weeks, and in the meantime, we hope you'll be able to help give us all feedback about the RC so that we can ensure a terrific release for all TypeScript users. Feel free to <a href="https://github.com/Microsoft/TypeScript/issues/new">drop us a line on GitHub if you run into any problems</a>, and let others know how you feel about this RC on Twitter and in the comments below! </article>
</div>