---
ID: 1215
post_title: Announcing TypeScript 2.6
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-6/
published: true
post_date: 2017-10-31 17:04:29
---
<div style="font-size: 16px">
  TypeScript 2.6 is here in time for Halloween, but have no fear! We've got some great treats for you in this release. If you haven't heard of TypeScript, it's a language that builds on top of the most up-to-date versions of JavaScript by adding optional static types. These types don't just help catch things like typos and logic errors; they also can bring you even better tooling like editor completions, easier navigation of your codebase, and more. Best of all, you don't always have to write out your type annotations - TypeScript can often infer them for you! To learn more about TypeScript itself, you can <a href="https://www.typescriptlang.org/">visit our website</a>, but if you're already familiar with it and want to try out the release, we have it available <a href="https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild">over NuGet</a> or through npm using the following command: <div class="highlight highlight-source-shell">
    <pre>npm install -g typescript</pre>
  </div> Visual Studio 2015 users (who have 
  
  <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs">Update 3</a>) can <a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/2.6.1-TS-release-dev14update3-20171027.2/TypeScript_Dev14Full.exe">install TypeScript 2.6 from here</a>, and Visual Studio 2017 users using <a href="https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes-v15.2">version 15.2 or later</a> will be able to get TypeScript by simply <a href="http://download.microsoft.com/download/7/0/A/70A6AC0E-8934-4396-A43E-445059F430EA/2.6.1-TS-release-dev14update3-20171027.2/TypeScript_SDK.exe">installing it from here</a>. Visual Studio 2017 users should be sure to <a href="https://github.com/Microsoft/TypeScript/wiki/Updating-TypeScript-in-Visual-Studio-2017">read up on how you can configure your project to target specific versions of TypeScript</a>. TypeScript 2.6 will be available for other editors soon. In the meantime you can configure <a href="https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions">Visual Studio Code</a> and <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript">Sublime Text</a> to use a newer version. <a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">Other editors</a> may have different approaches to getting TypeScript 2.6 running. Let's see what's ready today in TypeScript 2.6! <h2>
    Contravariant parameter types with <code style="color: #a31515;font-size: 29px">--strictFunctionTypes</code>
  </h2> When comparing signatures - things that make your types 
  
  <em>callable</em> or <em>constructable</em> - TypeScript has to account for both the return types and the parameter types. Return types are easy - for a function <code style="color: #a31515">f</code> to be assignable to <code style="color: #a31515">g</code>, <code style="color: #a31515">f</code>'s return type has to be assignable to <code style="color: #a31515">g</code>'s return type. The fact that the directionality doesn't change in this comparison is called <em>covariance</em>. However, parameters are actually a different story - the correct approach is to go in the opposite direction! To see why, let's take a quick example where we assign the below function <code style="color: #a31515">g</code> to <code style="color: #a31515">f</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Animal</span> { animalProp<span class="pl-k">:</span> <span style="color: #0000ff">any</span> };
<span style="color: #0000ff">interface</span> <span style="color: #267F99">Dog</span> <span style="color: #0000ff">extends</span> <span class="pl-e">Animal</span> { dogProp<span class="pl-k">:</span> <span style="color: #0000ff">any</span> };

<span style="color: #0000ff">let</span> f <span class="pl-k">=</span> (<span class="pl-v">animal</span><span class="pl-k">:</span> <span style="color: #267F99">Animal</span>) <span class="pl-k">=&gt;</span> <span class="pl-smi">animal</span>.<span class="pl-smi">animalProp</span>;
<span style="color: #0000ff">let</span> g <span class="pl-k">=</span> (<span class="pl-v">dog</span><span class="pl-k">:</span> <span style="color: #267F99">Dog</span>) <span class="pl-k">=&gt;</span> <span class="pl-smi">dog</span>.<span class="pl-smi">dogProp</span>;

<span style="color: #148A14">// Should this succeed?</span>
<span class="pl-smi">f</span> <span class="pl-k">=</span> <span class="pl-smi">g</span>;</pre>
  </div> At a glance, we might be tempted to say that since 
  
  <code style="color: #a31515">Dog</code> is assignable to <code style="color: #a31515">Animal</code>, <code style="color: #a31515">g</code> is assignable to <code style="color: #a31515">f</code>, but that's not the case. It becomes clear once we ask the right questions about substitutability: <ul>
    <li>
      Is it okay for a value of type <code style="color: #a31515">(dog: Dog) =&gt; any</code> to say it can be used in place of a <code style="color: #a31515">(animal: Animal) =&gt; any</code>? <ul>
        <li>
          Is it okay to say my function only expects an <code style="color: #a31515">Animal</code> when it may use properties that on <code style="color: #a31515">Dog</code>? <ul>
            <li>
              Only if an <code style="color: #a31515">Animal</code> can be used in place of a <code style="color: #a31515">Dog</code> - so is <code style="color: #a31515">Animal</code> assignable to <code style="color: #a31515">Dog</code>? <ul>
                <li>
                  No! It's missing <code style="color: #a31515">dogProp</code>.
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul> So 
  
  <code style="color: #a31515">g</code> is not assignable to <code style="color: #a31515">f</code>, but is the opposite true? <ul>
    <li>
      Is it okay for a value of type <code style="color: #a31515">(animal: Animal) =&gt; any</code> to say it can be used in place of a <code style="color: #a31515">(dog: Dog) =&gt; any</code>? <ul>
        <li>
          Is it okay to say my function expects a <code style="color: #a31515">Dog</code> when it may use properties that on <code style="color: #a31515">Animal</code>? <ul>
            <li>
              Only if a <code style="color: #a31515">Dog</code> can be used in place of an <code style="color: #a31515">Animal</code> - so is <code style="color: #a31515">Dog</code> assignable to <code style="color: #a31515">Animal</code>? <ul>
                <li>
                  Yes!
                </li>
              </ul>
            </li>
          </ul>
        </li>
      </ul>
    </li>
  </ul> Notice that in asking if 
  
  <code style="color: #a31515">(animal: Animal) =&gt; any</code> is assignable to <code style="color: #a31515">(dog: Dog) =&gt; any</code>, we end up asking whether <code style="color: #a31515">Dog</code> is assignable to <code style="color: #a31515">Animal</code>. This flip in direction is called <em>contravariance</em>. While this approach is appropriate in most languages, it's difficult to reconcile it with the way that JavaScript is broadly used. Things like using arrays and describing methods in the HTML DOM hierarchy turn out to be problematic with strict contravariance. For instance, in the <code style="color: #a31515">Array&lt;T&gt;</code> type, its <code style="color: #a31515">pop</code> method <em>returns</em> a <code style="color: #a31515">T</code> and its <code style="color: #a31515">push</code> method <em>takes</em> a <code style="color: #a31515">T</code>. If TypeScript compared every function parameter contravariantly, it would make all <code style="color: #a31515">Array</code>s <em>invariant</em> on <code style="color: #a31515">T</code> since <code style="color: #a31515">T</code> occurs in both covariant and contravariant positions. In other words, <code style="color: #a31515">Array&lt;Dog&gt;</code> wouldn't be assignable to <code style="color: #a31515">Array&lt;Animal&gt;</code>, which could be challenging to work around for many scenarios. That's why TypeScript compares parameters <em>bivariantly</em> or <em>bidirectionally</em>. It means completely unrelated types will get caught, but it means that certain unsavory issues can fall through the cracks when there is enough overlap: <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">function</span> makeLowerCase(<span class="pl-v">s</span><span class="pl-k">:</span> <span style="color: #0000ff">string</span>) {
    <span style="color: #0000ff">return</span> <span class="pl-smi">s</span>.<span class="pl-c1">toLowerCase</span>();
}

<span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> foo<span class="pl-k">:</span> <span style="color: #267F99">Promise</span>&lt;<span style="color: #0000ff">string</span> <span class="pl-k">|</span> <span style="color: #0000ff">number</span>&gt;;
<span class="pl-smi">foo</span>.<span class="pl-en">then</span>(<span class="pl-smi">makeLowerCase</span>); <span style="color: #148A14">// Whoops! TypeScript allows this, but `makeLowerCase` might get a `number`.</span></pre>
  </div> That's why in TypeScript 2.6, we're bringing users a way to tighten things up with 
  
  <code style="color: #a31515">--strictFunctionTypes</code> Under this new <code style="color: #a31515">--strict</code> mode flag, any function type that doesn't originate from a method has its parameters compared strictly contravariantly. That means that the above code will fail, because when we try to pass <code style="color: #a31515">makeLowerCase</code> with the type <code style="color: #a31515">(s: string) =&gt; string</code>to <code style="color: #a31515">then</code> which expects a function of type <code style="color: #a31515">(onfulfilled: string | number) =&gt; ...</code>, we'll flip directions and try to assess whether <code style="color: #a31515">string | number</code> is assignable to <code style="color: #a31515">string</code> (which isn't the case - <code style="color: #a31515">string</code> is a subtype of <code style="color: #a31515">string | number</code>). Excluding methods from the check allows TypeScript to continue modeling the above use-cases (e.g. event handlers and simpler array handling) while still bringing this much-demanded strictness check. <a href="https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)">Covariance and contravariance</a> probably deserve a more thorough explanation. If you'd like to read a bit more, <a href="https://www.stephanboyer.com/post/132/what-are-covariance-and-contravariance">Stephan Boyer has an approachable article</a> that gives a reasonable high-level explanation. You can also read up more on <a href="https://github.com/Microsoft/TypeScript/pull/18654">the original pull request</a>. The short version is that with <code style="color: #a31515">--strictFunctionTypes</code>, you'll be able to catch many common mistakes. Keep in mind that while <code style="color: #a31515">--strictFunctionTypes</code> is opt-in, it will automatically be turned on if you're operating with the <code style="color: #a31515">--strict</code> flag on. This may introduce some breaks, so to disable the check with <code style="color: #a31515">--strict</code> on, you can specify <code style="color: #a31515">--strictFunctionTypes false</code> on the command line or in your <code style="color: #a31515">tsconfig.json</code> as so: <div class="highlight highlight-source-json">
    <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>strict<span class="pl-pds">"</span></span>: <span style="color: #0000ff">true</span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>strictFunctionTypes<span class="pl-pds">"</span></span>: <span style="color: #0000ff">false</span>
    }
}</pre>
  </div>
  
  <h2>
    Translated <code style="color: #a31515;font-size: 29px">tsc</code> via the <code style="color: #a31515;font-size: 29px">--locale</code> flag
  </h2> The standalone TypeScript compiler now provides localized (translated) messages over npm when using the 
  
  <code style="color: #a31515">--locale</code> flag. Simply pass the appropriate <a href="https://msdn.microsoft.com/en-us/library/cc233982.aspx?f=255&MSPPError=-2147217396">language tag</a> as an argument to the TypeScript compiler's <code style="color: #a31515">--locale</code> option. If the language is supported, TypeScript will provide a translated version, and fall back to This means that you can get messages in Chinese (Simplified with <code style="color: #a31515">zh-CN</code> and Traditional with <code style="color: #a31515">zh-TW</code>): <div>
    <pre>tsc --pretty --locale zh-CN

bar.ts(1,1): error TS2362: 算术运算左侧必须是 "any"、、"number" 或枚举类型。

'hello' * 1
~~~~~~~</pre>
  </div> in Spanish: 
  
  <div class="highlight highlight-text-roff">
    <pre>$ tsc --pretty --locale es

foo.ts(1,1): error TS2362: La parte izquierda de una operación aritmética debe ser de tipo "any", "number" o un tipo enum.

'hello' * 1
~~~~~~~</pre>
  </div> in Japanese: 
  
  <div class="highlight highlight-text-roff">
    <pre>$ tsc --pretty --locale ja

foo.ts(1,1): error TS2362: 算術演算の左辺には、'any' 型、'number' 型、または列挙型を指定してください。

'hello' * 1
~~~~~~~</pre>
  </div> in Russian: 
  
  <div class="highlight highlight-text-roff">
    <pre>$ tsc --pretty --locale ru

foo.ts(1,1): error TS2362: Левый операнд арифметической операции должен иметь тип any, number или тип перечисления.

'hello' * 1
~~~~~~~</pre>
  </div> and others that you'll be able to read about on 
  
  <a href="https://www.typescriptlang.org/docs/handbook/compiler-options.html">our compiler options page</a>. <h2>
    Faster <code style="color: #a31515;font-size: 29px">--watch</code> mode
  </h2> TypeScript's 
  
  <code style="color: #a31515">--watch</code> mode now acts much more incrementally when emitting modules. Given a set of changed files, <code style="color: #a31515">tsc</code> will now figure out affected set of files for which a change might be impactful. This means that only that impacted files will undergo a tree transform pass (the process of transforming code from TypeScript to ES2016 to ES2015 to ES5 to ES3), as well as the emit pass (printing out the transformed files themselves). For extremely large codebases that heavily use ECMAScript modules, this can make a significant difference. If you're not using <code style="color: #a31515">--watch</code> mode because you rely on another build tool, the good news is that we intend to provide other tools with an API so that they can also get some of the same performance wins from this change. Tools that plug TypeScript into Webpack, Gulp, and others might be able to leverage this API, and we're hoping to deliver it in one of our near future releases. <h2>
    Error suppression comments with <code style="color: #a31515;font-size: 29px">// @ts-ignore</code>
  </h2> Historically, we've avoided error suppression within TypeScript because most cases where users have asked for it could be solved through more accurate declaration files or using a type assertion to 
  
  <code style="color: #a31515">any</code>. However, over time, we have seen two motivating examples: migrating from JavaScript to TypeScript, and overcoming type-checks that live in legacy code. With TypeScript 2.6 we're bringing <code style="color: #a31515">// @ts-ignore</code> comments to TypeScript files. These comments are a light-weight way to suppress any error that occurs on the next line. For example, in the following code, TypeScript would ordinarily report an error about the <code style="color: #a31515">console.log</code> statement being unreachable. In TypeScript 2.6, the <code style="color: #a31515">// @ts-ignore</code> comment squelches the error entirely. <div class="highlight highlight-source-ts">
    <pre><span style="color: #0000ff">if</span> (<span style="color: #0000ff">false</span>) {
    <span style="color: #148A14">// @ts-ignore: Unreachable code error</span>
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span class="pl-s"><span class="pl-pds">"</span>hello<span class="pl-pds">"</span></span>);
}</pre>
  </div> While this feature is motivated by pragmatism, we encourage taking type-checking errors seriously. Our guidance here is to use suppression comments 
  
  <em>very sparingly</em>. In situations where you <em>do</em> need to use these comments, we suggest leaving a trailing explanation of why the comment is necessary like in the example above. <h2>
    Improved tooling support
  </h2> Our investment in TypeScript doesn't only involve advancing the language and compiler. Improving the language service is core to the TypeScript experience. Here's a few improvements you'll see soon in editors like Visual Studio and Visual Studio Code. 
  
  <h3>
    Quick fixes for implicit <code style="color: #a31515;font-size: 29px">any</code>s
  </h3> TypeScript can now look at use-sites to infer types for declarations whose types are implicitly 
  
  <code style="color: #a31515">any</code>. <h3>
    Refactor JSDoc to TypeScript annotations
  </h3> TypeScript now provides a refactoring to add parameter annotations from JSDoc comments. When migrating from an older JavaScript codebase, you can use this refactoring together with the implicit 
  
  <code style="color: #a31515">any</code> quick fix to type your codebase even faster. <video width="100%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/10/inferFromUsageJSdoc2.mp4"> An editor session where two untyped functions are automatically annotated - the first has its types extracted from its JSDoc, and the other's is inferred from usage given the how the first function has been annotated. </video> <h3>
    Invoke uncalled decorators
  </h3> Occasionally you might try to use a decorator without calling it first. Thankfully, TypeScript can use some basic heuristics to figure these scenarios out, and can provide a useful error message with a handy quick-fix to correct from something like 
  
  <code style="color: #a31515">@Input</code> to <code style="color: #a31515">@Input()</code>. <video width="100%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/10/uncalledDecorators.mp4"> An editor session where two untyped functions are automatically annotated - the first has its types extracted from its JSDoc, and the other's is inferred from usage given the how the first function has been annotated. </video> <h3>
    Auto-install from <code style="color: #a31515;font-size: 29px">@types</code>
  </h3> Editors will soon be able to provide a quick fix to install type declarations for untyped imports! <video width="100%" autoplay muted loop src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/10/autoInstallTypes.mp4"> An editor session where types for the lodash package aren't installed, but applying a quick fix makes them appear in 
  
  <code>node_modules/@types/lodash</code> which then provides completions. </video> <h2>
    Breaking Changes and Deprecations
  </h2> There are several minor changes that may impact your codebase. While you can read up more about them in our 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">Breaking Changes</a> section, please keep these in mind when upgrading. <ul>
    <li>
      Write only references are now considered unused under <code style="color: #a31515">--noUnusedLocals</code> and <code style="color: #a31515">--noUnusedParameters</code>.
    </li>
    <li>
      In ambient contexts (e.g. declaration files, and <code style="color: #a31515">declare module</code> blocks), expressions are now disallowed in <code style="color: #a31515">default</code>exports.
    </li>
    <li>
      Uninhabitable types resulting from intersections (<code style="color: #a31515">number & string</code>, <code style="color: #a31515">"foo" & 42</code>, etc.) will simplify to <code style="color: #a31515">never</code> when placed in a union.
    </li>
    <li>
      Various organizational changes have been made to DOM declarations in <code style="color: #a31515">lib.d.ts</code>.
    </li>
  </ul> As for deprecations, the 
  
  <code style="color: #a31515">getSymbolDisplayBuilder</code> API is now deprecated. For simple use cases, you should be able to move to <code style="color: #a31515">TypeChecker#symbolToString</code>. For more advanced use cases, we plan to bring <code style="color: #a31515">symbolToString</code> fully up to par with <code style="color: #a31515">SymbolDisplayBuilder</code> in functionality by TypeScript 2.7. In a release following TypeScript 2.7, we will be be removing <code style="color: #a31515">getSymbolDisplayBuilder</code>. <h2>
    What's next?
  </h2> To get a more complete picture of what's in TypeScript 2.6, you can check out our 
  
  <a href="https://github.com/Microsoft/TypeScript/wiki/What's-new-in-TypeScript">What's New in TypeScript</a> wiki page. You can also see what we've got coming up in our release schedule on <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">the TypeScript Roadmap</a>. It should go without saying, but let us know if you run into any problems <a href="https://github.com/Microsoft/TypeScript/issues/new">on our issue tracker</a>. And if you're enjoying this release, let us know on Twitter by using the <a href="https://twitter.com/intent/tweet?text=%23iHeartTypeScript">#iHeartTypeScript</a> hashtag. We hope that this version of TypeScript is easy to adopt, brings even more type safety, makes you more productive, and is just plain fun to use. Happy Hacking!
</div>