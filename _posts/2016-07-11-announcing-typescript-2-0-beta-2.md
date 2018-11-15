---
ID: 1677
post_title: Announcing TypeScript 2.0 Beta
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-0-beta-2/
published: true
post_date: 2016-07-11 16:28:51
---
Today we're excited to roll out our beta release of TypeScript 2.0. If you're not familiar with TypeScript yet, you can start learning it today on [our website][1]. To get your hands on the beta, you can download [TypeScript 2.0 Beta for Visual Studio 2015][2] (which will require [VS 2015 Update 3][3]), or just run <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@beta</pre>
</div> This release includes plenty of new features, such as 

[our new workflow for getting `.d.ts` files][4], but here's a couple more features just to get an idea of what else is in store. 
## Non-nullable Types {#non-nullable-types}

`null` and `undefined` are two of the most common sources of bugs in JavaScript. Before TypeScript 2.0, `null` and `undefined` were in the domain of *every* type. That meant that if you had a function that took a `string`, you couldn't be sure from the type alone of whether you actually had a `string` - you might actually have `null`. In TypeScript 2.0, the new `--strictNullChecks` flag changes that. `string` just means `string` and `number` means `number`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">let</span> <span class="pl-v">foo</span>: <span style="color: #0000ff">string</span> = <span style="color: #0000ff">null</span>; <span style="color: #148A14">// Error!</span></pre>
</div> What if you 

*wanted* to make something nullable? Well we've brought two new types to the scene: `null` and `undefined`. As you might expect, `null` can only contain `null`, and `undefined` only contains `undefined`. They're not totally useful on their own, but you can use them in a union type to describe whether something *could* be `null`/`undefined`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">let</span> <span class="pl-v">foo</span>: <span style="color: #0000ff">string</span> | <span style="color: #0000ff">null</span> = <span style="color: #0000ff">null</span>; <span style="color: #148A14">// Okay!</span></pre>
</div> Because you might often know better than the type system, we've also introduced a postfix 

`!` operator that takes `null` and `undefined` out of the type of any expression. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">let</span> <span class="pl-v">strs</span>: <span style="color: #0000ff">string</span>[] | <span style="color: #0000ff">undefined</span>;

<span style="color: #148A14">// Error! 'strs' is possibly undefined.</span>
<span style="color: #0000ff">let</span> <span class="pl-v">upperCased</span> = strs.map(s =&gt; s.toUpperCase());

<span style="color: #148A14">// 'strs!' means we're sure it can't be 'undefined', so we can call 'map' on it.</span>
<span style="color: #0000ff">let</span> <span class="pl-v">lowerCased</span> = strs!.map(s =&gt; s.toLowerCase());</pre>
</div>

## <a id="user-content-control-flow-analysis-for-types" class="anchor" href="https://gist.github.com/DanielRosenwasser/dfe91fb1d64f1205c1886407010756fa#control-flow-analysis-for-types"></a>Control Flow Analysis for Types TypeScript's support for handling nullable types is possible thanks to changes in how types are tracked throughout the program. In 2.0, we've started using control flow analysis to better understand what a type has to be at a given location. For instance, consider this function. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">/**</span>
<span style="color: #148A14"> * @param recipients  An array of recipients, or a comma-separated list of recipients.</span>
<span style="color: #148A14"> * @param body        Primary content of the message.</span>
<span style="color: #148A14"> */</span>
<span style="color: #0000ff">function</span> <span class="pl-en">sendMessage</span>(<span class="pl-v">recipients</span>: <span style="color: #0000ff">string</span> | <span style="color: #0000ff">string</span>[], <span class="pl-v">body</span>: <span style="color: #0000ff">string</span>) {
    <span style="color: #0000ff">if</span> (<span style="color: #0000ff">typeof</span> recipients === <span style="color: #a31515">"string"</span>) {
        recipients = recipients.split(<span style="color: #a31515">","</span>);
    }

    <span style="color: #148A14">// TypeScript knows that 'recipients' is a 'string[]' here.</span>
    recipients = recipients.filter(isValidAddress);
    <span style="color: #0000ff">for</span> (<span style="color: #0000ff">let</span> r <span style="color: #0000ff">of</span> recipients) {
        <span style="color: #148A14">// ...</span>
    }
}</pre>
</div> Notice that after the assignment within the 

`if` block, TypeScript understood that it had to be dealing with an array of `string`s. This sort of thing can catch issues early on and save you from spending time on debugging. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">let</span> <span class="pl-v">bestItem</span>: Item;
<span style="color: #0000ff">for</span> (<span style="color: #0000ff">let</span> item <span style="color: #0000ff">of</span> items) {
    <span style="color: #0000ff">if</span> (item.id === <span style="color: #09885A">42</span>) bestItem = item;
}

<span style="color: #148A14">// Error! 'bestItem' might not have been initialized if 'items' was empty.</span>
<span style="color: #0000ff">let</span> <span class="pl-v">itemName</span> = bestItem.name;</pre>
</div> We owe a major thanks to 

[Ivo Gabe de Wolff][5] for his work and involvement in implementing this feature, which started out with his thesis project and grew into part of TypeScript itself. 
## <a id="user-content-easier-module-declarations" class="anchor" href="https://gist.github.com/DanielRosenwasser/dfe91fb1d64f1205c1886407010756fa#easier-module-declarations"></a>Easier Module Declarations Sometimes you want to just tell TypeScript that a module exists, and you might not care what its shape is. It used to be that you'd have to write something like this: 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">module</span> <span style="color: #a31515">"foo"</span> {
    <span style="color: #0000ff">var</span> <span class="pl-v">x</span>: <span style="color: #0000ff">any</span>;
    <span style="color: #0000ff">export</span> = x;
}</pre>
</div> But that's a hassle, so we made it easier and got rid of the boilerplate. In TypeScript 2.0 you can just write 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">module</span> <span style="color: #a31515">"foo"</span>;
<span style="color: #0000ff">declare</span> <span style="color: #0000ff">module</span> <span style="color: #a31515">"bar"</span>;</pre>
</div> When you're ready to finally outline the shape of a module, you can come back to these declarations and define the structure you need. What if you you depend on a package with lots of modules? Writing those out for each module might be a pain, but TypeScript 2.0 makes that easy too by allowing wildcards in these declarations! 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">module</span> <span style="color: #a31515">"foo/*"</span>;</pre>
</div> Now you can import any path that starts with 

`foo/` and TypeScript will assume it exists. You can take advantage of this if your module loader understands how to import based on a certain patterns too. For example: <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">module</span> <span style="color: #a31515">"*!text"</span> {
    <span style="color: #0000ff">const</span> <span class="pl-v">content</span>: <span style="color: #0000ff">string</span>;
    <span style="color: #0000ff">export</span> = content;
}</pre>
</div> Now whenever you import a path ending with 

`!text`, TypeScript will understand that the import should be typed as a `string`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">import</span> text = require(<span style="color: #a31515">"./hello.txt!text"</span>);
text.toLowerCase();</pre>
</div>

## <a id="user-content-next-steps" class="anchor" href="https://gist.github.com/DanielRosenwasser/dfe91fb1d64f1205c1886407010756fa#next-steps"></a>Next Steps One feature you might be wondering about is support for 

`async` functions in ES3 & ES5. Originally, this was slated for the 2.0 release; however, to reasonably implement `async`/`await`, we needed to rewrite TypeScript's emitter as a series of tree transformations. Doing so, while keeping TypeScript fast, has required a lot of work and attention to detail. While we feel confident in today's implementation, confidence is no match for thorough testing, and more time is needed for `async`/`await`to stabilize. You can expect it in TypeScript 2.1, and if you'd like to track the progress, [the pull request is currently open on GitHub][6]. TypeScript 2.0 is still packed with many useful new features, and we'll be coming out with more details as time goes on. If you're curious to hear more about what's new, you can [take a look at our wiki][7]. In the coming weeks, a more stable release candidate will be coming out, with the final product landing not too far after. We'd love to hear any feedback you have, either in the comments below or on GitHub. Happy hacking!

 [1]: https://www.typescriptlang.org/
 [2]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS2.0.0-TS-release20-nightly-20160708.1/TypeScript_Dev14Full.exe
 [3]: https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs
 [4]: https://blogs.msdn.microsoft.com/typescript/2016/06/15/the-future-of-declaration-files/
 [5]: https://github.com/ivogabe
 [6]: https://github.com/Microsoft/TypeScript/pull/9175
 [7]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript