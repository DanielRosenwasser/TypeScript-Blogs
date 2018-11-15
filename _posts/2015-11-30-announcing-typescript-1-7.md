---
ID: 511
post_title: Announcing TypeScript 1.7
author: 'Gaurav Seth [MSFT]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-7/
published: true
post_date: 2015-11-30 12:48:00
---
Today, we are thrilled to announce the release of TypeScript 1.7 along with the availability of [<span style="color: #4078c0">Visual Studio 2015 Update 1.</span>][1] This release [<span style="color: #4078c0">enables async/await by default for ECMAScript 6 (ES6) targets</span>][2]. It also adds support for polymorphic 'this' typing, proposed ECMAScript 2016 exponentiation syntax, and ES6 module targeting. For a complete change list, check out our [<span style="color: #4078c0">roadmap</span>][3] on GitHub.

As always, you can get your hands on TypeScript 1.7 for [Visual Studio 2015 Update 1][4], [<span style="color: #4078c0">Visual Studio 2013</span>][5], [<span style="color: #4078c0">on npm</span>][6], or straight from the [<span style="color: #4078c0">source</span>][7].

**Async/Await for ES6 targets**

With the 1.7 release, TypeScript now supports [Async functions][8] for targets that have [<span style="color: #4078c0">ES6 generator</span>][9] support enabled (e.g. node.js v4 and above). Functions can now be prefixed with the `async` keyword designating it as an asynchronous function. The `await` keyword can then be used to stop execution until an `async` function's promise is fulfilled. Following is a simple example:

<span style="font-family: courier new,courier"><span style="color: #993300">"use strict"</span>;</span>  
<span style="color: #008000;font-family: courier new,courier">// printDelayed is a 'Promise<void>'</span>  
<span style="font-family: courier new,courier"><span style="color: #0000ff">async function</span> printDelayed(elements: string[]) {</span>  
<span style="font-family: courier new,courier"><span style="color: #0000ff">    for</span> (<span style="color: #0000ff">const</span> element <span style="color: #0000ff">of</span> elements) {</span>  
<span style="font-family: courier new,courier">        await delay(200);</span>  
<span style="font-family: courier new,courier">        console.log(element);</span>  
<span style="font-family: courier new,courier">    }</span>  
<span style="font-family: courier new,courier">}</span>  
   
<span style="font-family: courier new,courier"><span style="color: #0000ff">async function</span> delay(milliseconds: number) {</span>  
<span style="font-family: courier new,courier"><span style="color: #0000ff">    return</span> <span style="color: #0000ff">new</span> Promise<<span style="color: #0000ff">void</span>>(resolve => {</span>  
<span style="font-family: courier new,courier">        setTimeout(resolve, milliseconds);</span>  
<span style="font-family: courier new,courier">    });</span>  
<span style="font-family: courier new,courier">}</span>  
   
<span style="font-family: courier new,courier">printDelayed([<span style="color: #993300">"Hello"</span>, <span style="color: #993300">"beautiful"</span>, <span style="color: #993300">"asynchronous"</span>, <span style="color: #993300">"world"</span>]).then(() => {</span>  
<span style="font-family: courier new,courier">    console.log();</span>  
<span style="font-family: courier new,courier">    console.log(<span style="color: #993300">"Printed every element!"</span>);</span>  
<span style="font-family: courier new,courier">});</span>

We are working on bringing async/await support in TypeScript for other targets, including a breadth of browsers, which might not have ES6 generators support. For more information on current implementation of async/await and how to use it, see our [<span style="color: #4078c0">previous blog post</span>][2].

**Polymorphic* this***** Typing**

After much [<span style="color: #4078c0">community discussion and feedback</span>][10], TypeScript 1.7 adds a new polymorphic this type. A this type can be used in classes and interfaces to represent *some type that is a subtype of the containing type* (rather than the containing type itself). This feature makes patterns such as hierarchical fluent APIs much easier to express.

<span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> Model {</span>  
<span style="font-family: courier new,courier">    setupBase(): <span style="color: #0000ff">this</span>;</span>  
<span style="font-family: courier new,courier">}</span>  
   
<span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> AdvancedModel extends Model {</span>  
<span style="font-family: courier new,courier">    setupAdvanced(): <span style="color: #0000ff">this</span>;</span>  
<span style="font-family: courier new,courier">}</span>  
   
<span style="font-family: courier new,courier"><span style="color: #0000ff">declare function</span> createModel(): AdvancedModel;</span>  
<span style="font-family: courier new,courier">newModel = newModel.setupBase().setupAdvanced(); <span style="color: #008000">// fluent style works</span></span>

For a deep dive on this typing, checkout the [<span style="color: #4078c0">TypeScript Wiki</span>][11].

As a part of supporting the feature, TypeScript 1.7 has made changes in inferring the type from this. In a class, the type of the value this will be inferred to the this type, and subsequent assignments from values of the original type can fail. As a workaround, you could add a type annotation for this. A code sample with recommended work around, along with a list of other [<span style="color: #4078c0">potentially breaking changes</span>][12] is available at GitHub.

**ES6 Module Emitting**

TypeScript 1.7 adds es6 to the list of options available for the --module flag and allows you to specify the module output when targeting ES6. This provides more flexibility to target exactly the features you want in specific runtimes. For example, it is now a breeze to target Node.js v4 and beyond, which doesn't support ES6 modules (but does support several other ES6 features).

<span style="color: #993300;font-family: courier new,courier">//tsconfig.json targeting node.js v4 and beyond</span>  
<span style="font-family: courier new,courier">{</span>  
<span style="font-family: courier new,courier"><span style="color: #993300">    "compilerOptions"</span>: {</span>  
<span style="font-family: courier new,courier"><span style="color: #993300">        "module"</span>: <span style="color: #993300">"commonjs"</span>,</span>  
<span style="font-family: courier new,courier"><span style="color: #993300">        "target"</span>: <span style="color: #993300">"es6"</span></span>  
<span style="font-family: courier new,courier">    }</span>  
<span style="font-family: courier new,courier">}</span>

**ES7 Exponentiation**

Finally, a little syntactic sugar! The ECMAScript committee recently moved the [<span style="color: #4078c0">Exponentiation Operator proposal</span>][13] to [<span style="color: #4078c0">stage 3</span>][14]. So we decided it was ready for TypeScript to adopt, and added support for it in TypeScript 1.7.

<div>
  <span style="font-family: courier new,courier"><span style="color: #0000ff">let</span> squared = 2 ** 2;  <span style="color: #008000">// same as: 2 * 2</span></span><br /><span style="font-family: courier new,courier"><span style="color: #0000ff">let</span> cubed = 2 ** 3;  <span style="color: #008000">// same as: 2 * 2 * 2</span></span>
</div>

<div>
  <span style="font-family: courier new,courier"><span style="color: #0000ff">let</span> num = 2;</span><br /><span style="font-family: courier new,courier">num **= 2; <span style="color: #008000">// same as: num = num * num;</span></span>
</div>

Say goodbye to <span style="font-family: courier new,courier">Math.pow()</span>!

**What's Next?**

We are excited to announce all the new improvements we've made in this release, and as always, we would love to hear your feedback. Everything we do is easily viewable on our [<span style="color: #4078c0">Github</span>][15]. If you're interested in weighing in on the future of TypeScript, we encourage you to check out our existing [<span style="color: #4078c0">issues</span>][16], throw us a [<span style="color: #4078c0">pull request</span>][17], or just come hang out with the team on [<span style="color: #4078c0">gitter</span>][18].

 [1]: http://blogs.msdn.com/b/visualstudio/archive/2015/11/30/visual-studio-update-1-rtm.aspx
 [2]: http://blogs.msdn.com/b/typescript/archive/2015/11/03/what-about-async-await.aspx
 [3]: https://github.com/Microsoft/TypeScript/wiki/Roadmap
 [4]: https://www.visualstudio.com/news/vs2015-update1-vs "http://go.microsoft.com/fwlink/?LinkId=691129"
 [5]: http://download.microsoft.com/download/4/4/3/443F86B7-A89F-48E6-AC96-0AAC2A910A29/TS1.7.3-VSUOOB.41125.00/TypeScript_Dev12.exe
 [6]: https://www.npmjs.com/package/typescript
 [7]: https://github.com/Microsoft/TypeScript/releases
 [8]: http://tc39.github.io/ecmascript-asyncawait/ "http://tc39.github.io/ecmascript-asyncawait/"
 [9]: http://www.ecma-international.org/ecma-262/6.0/#sec-generator-function-definitions
 [10]: https://github.com/Microsoft/TypeScript/issues/229
 [11]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#this-typing
 [12]: https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes#TypeScript1.7
 [13]: https://github.com/rwaldron/exponentiation-operator
 [14]: https://tc39.github.io/process-document/
 [15]: https://github.com/Microsoft/TypeScript
 [16]: https://github.com/microsoft/typescript/issues
 [17]: https://github.com/Microsoft/TypeScript/pulls
 [18]: https://gitter.im/Microsoft/TypeScript