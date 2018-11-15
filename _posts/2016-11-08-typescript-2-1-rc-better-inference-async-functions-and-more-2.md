---
ID: 1679
post_title: 'TypeScript 2.1 RC: Better Inference, Async Functions, and More'
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescript-2-1-rc-better-inference-async-functions-and-more-2/
published: true
post_date: 2016-11-08 17:33:28
---
Today we're happy to announce our release candidate for TypeScript 2.1! If you aren't familiar with it, [TypeScript][1] is a language that adds optional static types to JavaScript, and brings new features from ES6 and later to whatever JavaScript runtime you're using. As usual you can get the RC through NuGet, or just by running <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@rc</pre>
</div> You can then easily use the RC release with 

[Visual Studio Code][2] or [our Sublime Text Plugin][3]. You can also grab [the TypeScript 2.1 installer for Visual Studio 2015][4] after getting [Update 3][5]. While TypeScript 2.1 has a lot of great features coming up, we'd like to highlight how much more powerful TypeScript 2.1's inference will be, as well as how much easier it will be to write asynchronous code in all runtimes. 
## <a id="user-content-smarter-inference" class="anchor"></a>Smarter Inference TypeScript 2.1 now makes it easier to model scenarios where you might 

*incrementally* initialize variables. Since a lot of code is written like this in JavaScript, this makes it even easier to migrate existing codebases to TypeScript. To understand better, let's start off by talking about the `any` type. Most of the time, if TypeScript can't figure out the type of a variable, it will choose the `any` type to be as flexible as possible without troubling you. We often call this an *implicit* `any` (as opposed to an *explicit* one, where you would have written out the type). <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">let</span> x;      <span style="color: #148A14">// implicitly 'any'</span>
<span style="color: #00f">let</span> y <span>=</span> []; <span style="color: #148A14">// implicitly 'any[]'</span>

<span style="color: #00f">let</span> z<span style="color: #00f">:</span> <span style="color: #267F99">any</span>; <span style="color: #148A14">// explicitly 'any'.</span></pre>
</div> From that point on, you can do anything you want with those values. For many people, that behavior was too loose, which is why the 

`--noImplicitAny` option will warn you whenever a type couldn't be inferred. With TypeScript 2.0 we built out a foundation of using control flow analysis to track the flow of types throughout your program. Because that analysis examines the assignments of every variable, we've leveraged that same foundation in TypeScript 2.1 to more deeply examine the type of any variable that seems like it's destined for a better type. Instead of just choosing `any`, TypeScript will infer types based on what you end up assigning later on. Let's take the following example. <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">let</span> x;

<span style="color: #148A14">// We can still assign anything we want to 'x'.</span>
<span class="pl-en">x</span> <span>=</span> () <span>=&gt;</span> <span style="color: #09885A">42</span>;

<span style="color: #148A14">// After that last assignment, TypeScript 2.1 knows that 'x'</span>
<span style="color: #148A14">// has the type '() =&gt; number', so it can be called.</span>
<span class="pl-en">x</span>();

<span style="color: #148A14">// But now we'll get an error that we can't add a number to a function!</span>
<span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span class="pl-smi">x</span> <span>+</span> <span style="color: #09885A">42</span>);
<span style="color: #148A14">//          ~~~~~~</span>
<span style="color: #148A14">// error: Operator '+' cannot be applied to types '() =&gt; number' and 'number'.</span>

<span style="color: #148A14">// TypeScript still allows us to assign anything we want to 'x'.</span>
<span class="pl-smi">x</span> <span>=</span> <span style="color: #a31515"><span class="pl-pds">"</span>Hello world!<span class="pl-pds">"</span></span>;

<span style="color: #148A14">// But now it also knows that now 'x' is a 'string'!</span>
<span class="pl-smi">x</span>.<span class="pl-c1">toLowerCase</span>();</pre>
</div> When it comes to assignments, TypeScript will still trust you and allow you to assign anything you want to 

`x`. However, for any other uses, the type checker will know better by climbing up and looking at whatever you've actually done with `x`. The same sort of tracking is now also done for empty arrays! This means better completions: <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">let</span> puppies <span>=</span> [];

<span class="pl-smi">puppies</span>.<span class="pl-c1">push</span>(<span style="color: #00f">new</span> <span class="pl-en">Puppy</span>());

<span style="color: #00f">for</span> (<span style="color: #00f">let</span> pup <span style="color: #00f">of</span> <span class="pl-smi">puppies</span>) {
    <span class="pl-smi">pup</span>.<span class="pl-en">bark</span>();
    <span style="color: #148A14">//  ^^^^ Get completion on 'bark'</span>
}</pre>
</div> And it also means that TypeScript can catch more obvious errors: 

<div class="highlight highlight-source-ts">
  <pre><span class="pl-smi">puppies</span>[<span style="color: #09885A">1</span>] <span>=</span> <span style="color: #00f">new</span> <span class="pl-en">Kitty</span>();

<span style="color: #00f">for</span> (<span style="color: #00f">let</span> pup <span style="color: #00f">of</span> <span class="pl-smi">puppies</span>) {
    <span class="pl-smi">pup</span>.<span class="pl-en">bark</span>();
    <span style="color: #148A14">//  ~~~~ error: 'bark' does not exist on type 'Kitty'</span>
}</pre>
</div><video style="width: 75%" autoplay="autoplay" loop="loop" src="http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2016/11/cfa-any-array-puppy-kitty-dark22.mp4"> </video> The end result of all this is that you'll see 

*way fewer* implicit `any` errors in the future, and get much better tooling support. 
## <a id="user-content-downlevel-async-functions" class="anchor"></a>Downlevel Async Functions Support for down-level asynchronous functions (or async/await) is coming in 2.1, and you can use it in today's release candidate! 

`async`/`await` is a new feature in ECMAScript 2017 that allows users to write code around promises without needing to use callbacks. `async` functions can be written in a style that looks synchronous, but acts asynchronously, using the `await` keyword. This feature was supported before TypeScript 2.1, but only when targeting ES6/ES2015. TypeScript 2.1 brings the capability to ES3 and ES5 runtimes, meaning you'll be free to take advantage of it no matter what environment you're using. For example, let's take the following function named `delay`, which returns a promise and waits for a certain amount of time before resolving: <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">function</span> delay(<span style="color: #000000">milliseconds</span><span style="color: #00f">:</span> <span style="color: #267F99">number</span>) {
    <span style="color: #00f">return</span> <span style="color: #00f">new</span> <span class="pl-en">Promise</span>&lt;<span style="color: #267F99">void</span>&gt;(<span style="color: #000000">resolve</span> <span>=&gt;</span> {
      <span class="pl-c1">setTimeout</span>(<span class="pl-smi">resolve</span>, <span class="pl-smi">milliseconds</span>);
    });
}</pre>
</div> Let's try to work on a simple-sounding task. We want to write a program that prints 

`"Hello"`, three dots, and then `"World!"`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">function</span> welcome() {
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Hello<span class="pl-pds">"</span></span>);

    <span style="color: #00f">for</span> (<span style="color: #00f">let</span> i <span>=</span> <span style="color: #09885A">0</span>; <span class="pl-smi">i</span> <span style="color: #00f">&lt;</span> <span style="color: #09885A">3</span>; <span class="pl-smi">i</span><span>++</span>) {
        <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>.<span class="pl-pds">"</span></span>);
    }

    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>World!<span class="pl-pds">"</span></span>);
}</pre>
</div> This turned out to be about as simple as it sounded. Now let's say we want to use our 

`delay` function to pause before each dot. Without `async`/`await`, we'd have to write something like the following: <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">function</span> dramaticWelcome() {
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Hello<span class="pl-pds">"</span></span>);

    (<span style="color: #00f">function</span> loop(<span style="color: #000000">i</span>){
        <span style="color: #00f">if</span> (<span class="pl-smi">i</span> <span style="color: #00f">&lt;</span> <span style="color: #09885A">3</span>) {
            <span class="pl-en">delay</span>(<span style="color: #09885A">500</span>).<span class="pl-en">then</span>(() <span>=&gt;</span> {
                <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>.<span class="pl-pds">"</span></span>);
                <span class="pl-en">loop</span>(<span class="pl-smi">i</span> <span>+</span> <span style="color: #09885A">1</span>);
            });
        }
        <span style="color: #00f">else</span> {
            <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>World!<span class="pl-pds">"</span></span>);
        }
    })(<span style="color: #09885A">0</span>);
}</pre>
</div> This doesn't look quite as simple any more! What about if we tried using 

`async` functions to make this code more readable? First, we need to make sure our runtime has an ECMAScript-compliant `Promise` available globally. That might involve grabbing [a polyfill][6] for `Promise`, or relying on one that you might have in the runtime that you're targeting. We also need to make sure that TypeScript knows `Promise` exists by setting our `lib` flag to something like `"dom", "es2015"` or `"dom", "es2015.promise", "es5"`: <div class="highlight highlight-source-json">
  <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span>: {
        <span style="color: #a31515"><span class="pl-pds">"</span>lib<span class="pl-pds">"</span></span>: [<span style="color: #a31515"><span class="pl-pds">"</span>dom<span class="pl-pds">"</span></span>, <span style="color: #a31515"><span class="pl-pds">"</span>es2015.promise<span class="pl-pds">"</span></span>, <span style="color: #a31515"><span class="pl-pds">"</span>es5<span class="pl-pds">"</span></span>]
    }
}</pre>
</div> Now we can rewrite our code to use 

`async` and `await`: <div class="highlight highlight-source-ts">
  <pre><span style="color: #00f">async</span> <span style="color: #00f">function</span> dramaticWelcome() {
    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>Hello<span class="pl-pds">"</span></span>);

    <span style="color: #00f">for</span> (<span style="color: #00f">let</span> i <span>=</span> <span style="color: #09885A">0</span>; <span class="pl-smi">i</span> <span style="color: #00f">&lt;</span> <span style="color: #09885A">3</span>; <span class="pl-smi">i</span><span>++</span>) {
        <span style="color: #00f">await</span> <span class="pl-en">delay</span>(<span style="color: #09885A">500</span>);
        <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>.<span class="pl-pds">"</span></span>);
    }

    <span class="pl-c1">console</span>.<span class="pl-c1">log</span>(<span style="color: #a31515"><span class="pl-pds">"</span>World!<span class="pl-pds">"</span></span>);
}</pre>
</div> Notice how similar this is compared to our synchronous version! Despite its looks, this function is actually 

*asynchronous*, and won't block other code from running in between each pause. In fact, the two versions of `dramaticWelcome` basically boil down to the same code, but with `async` & `await`, TypeScript does the heavy lifting for us. 
## <a id="user-content-next-steps" class="anchor"></a>Next Steps TypeScript 2.1 RC has plenty of other features, and we'll have even more coming for 2.1 proper. You can take a look at 

[our roadmap][7] to see what else is in store. We hope you give it a try and enjoy it!

 [1]: https://typescriptlang.org/
 [2]: https://code.visualstudio.com/docs/languages/typescript#_using-newer-typescript-versions
 [3]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [4]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS2.1-dev14update3-20161206.2/TypeScript_Dev14Full.exe
 [5]: https://msdn.microsoft.com/en-us/library/mt752379.aspx
 [6]: https://github.com/stefanpenner/es6-promise
 [7]: https://github.com/Microsoft/TypeScript/wiki/Roadmap