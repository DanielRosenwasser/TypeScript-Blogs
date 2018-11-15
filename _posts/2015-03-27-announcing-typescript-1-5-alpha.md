---
ID: 313
post_title: Announcing TypeScript 1.5 Alpha
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-5-alpha/
published: true
post_date: 2015-03-27 15:20:00
---
<p dir="ltr">
  <span style="font-size: 12px">Today we’re announcing TypeScript 1.5 Alpha, the first preview of the TypeScript 1.5 release.  This release shows off many of the features that will be in the final TypeScript 1.5 release.  In the alpha release, you’ll be able to use three new capabilities of the TypeScript tools: a richer ES6 experience, decorators, and a new </span><a style="font-size: 12px" href="https://github.com/Microsoft/TypeScript-Sublime-Plugin">Sublime Text plugin</a><span style="font-size: 12px">.</span>
</p>

<p dir="ltr">
  You can try this alpha out today by installing the new compiler available on <a href="https://www.npmjs.com/package/typescript">npm</a>.
</p>

## <span>Richer ES6 experience</span>

<p dir="ltr">
  <span>In TypeScript 1.5, we’re adding a number of new ES6 features.  These features work together with the TypeScript type system to give you helpful tooling when working with the new ES6 code patterns.</span>
</p>

### <span>Modules</span>

<p dir="ltr">
  <span>The module syntax of ES6 is a powerful way of working with modules.  You can interact with modules by importing whole modules or by working with individual exports directly.</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> * <span style="color: #0000ff">as</span> Math <span style="color: #0000ff">from</span> <span style="color: #993300">"my/math"</span>;<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> { add, subtract } <span style="color: #0000ff">from</span> <span style="color: #993300">"my/math"</span>;</span>
</p>

<p dir="ltr">
  ES6 also supports a range of functionality in specifying exports.  You can export declarations, such as classes or functions.  You can also export a ‘default’ to import the module directly.  For example:
</p>

<p dir="ltr">
  <span style="color: #339966;font-family: 'courier new', courier">// math.ts</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">export function</span> add(x, y) { <span style="color: #0000ff">return</span> x + y }<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">export function</span> subtract(x, y) { <span style="color: #0000ff">return</span> x – y }<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">export default</span> <span style="color: #0000ff">function</span> multiply(x, y) { <span style="color: #0000ff">return</span> x * y }</span>
</p>

<p dir="ltr">
  <span style="color: #339966;font-family: 'courier new', courier">// myFile.ts</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> {add, subtract} <span style="color: #0000ff">from</span> <span style="color: #993300">"math"</span>;<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> times <span style="color: #0000ff">from </span><span style="color: #993300">"math"</span>;<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> result = times(add(2, 3), subtract(5, 3));</span>
</p>

<p dir="ltr">
  <span>If you’ve been using TypeScript, you may notice that this is similar to TypeScript external modules.  This is no accident.  When we created external modules for TypeScript, we were working on the same problems.  The ES6 design takes the capabilities even further, showing a powerful, mature design.  We will continue to support external modules, but we will begin encouraging developers to use the more capable ES6 module syntax.</span>
</p>

### <span>Destructuring</span>

<p dir="ltr">
  <span>Destructuring is a handy new feature that comes as part of our ES6 support.  With it, you can pull apart, or destructure, objects and arrays. </span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> [x, y] = [10, 20];<br /></span><span style="font-family: 'courier new', courier">[x, y] = [y, x];  <span style="color: #339966">// a simple swap</span></span>
</p>

<p dir="ltr">
  <span>You can also use destructuring to handle function parameters:</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> myClient = {name: <span style="color: #800000">"Bob"</span>, height: 6};<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> greetClient({name, height: howTall}) {<br />  </span><span style="font-family: 'courier new', courier">console.log(<span style="color: #800000">"Hello, "</span> + name + <span style="color: #800000">", who is "</span> + howTall + <span style="color: #800000">" feet tall."</span>);<br /></span><span style="font-family: 'courier new', courier">}<br />greetClient(myClient); </span>
</p>

<p dir="ltr">
  In the above, <span style="font-family: 'courier new', courier">greetClient</span> takes in a single object that has a <span style="font-family: 'courier new', courier">name</span> and <span style="font-family: 'courier new', courier">height</span> property. Using the <span style="font-family: 'courier new', courier">'height: howTall'</span> syntax, we can rename the <span style="font-family: 'courier new', courier">height</span> property to <span style="font-family: 'courier new', courier">howTall</span> inside of <span style="font-family: 'courier new', courier">greetClient</span>.
</p>

### <span>And more</span>

<p dir="ltr">
  <span>We’ve also added for-of support for better iteration, let/const compiling to ES5, unicode support, an ES6 output mode, and better support for computed properties.</span>
</p>

## <span>Decorators</span>

<p dir="ltr">
  We’ve also worked with the <a href="https://angular.io/">Angular</a>, <a href="http://emberjs.com/">Ember</a>, and <a href="http://aurelia.io/">Aurelia</a> (from the makers of Durandal) teams on a decorator proposal for ES7, which we’re previewing in TypeScript 1.5 Alpha.  Decorators allow you to create a clean separation of concerns.  In this example, we see how the <span style="font-family: 'courier new', courier">@memoize</span> decorator could be used to note that a getter/setter pair can be memoized:
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> Person {<br />  </span><span style="font-family: 'courier new', courier">@memoize<br />  </span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">get</span> name() { <span style="color: #0000ff">return </span><span style="color: #800000">`${this.first} ${this.last}`</span> }</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">  set</span> name(val) {<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">    let</span> [first, last] = val.split(<span style="color: #800000">' '</span>);<br />    </span><span style="font-family: 'courier new', courier">this.first = first;<br />    </span><span style="font-family: 'courier new', courier">this.last = last;<br />  </span><span style="font-family: 'courier new', courier">}<br /></span><span style="font-family: 'courier new', courier">}</span>
</p>

<p dir="ltr">
  <span>Developers will be able to write new decorators and mix-and-match them to work with the type system.</span>
</p>

## <span>Sublime Text plugin</span>

<p dir="ltr">
  <a href="https://devblogs.microsoft.com/00/00/01/56/67/5428.sublime1.png"><img src="https://devblogs.microsoft.com/00/00/01/56/67/5428.sublime1.png" alt="" border="0" /></a>
</p>

<p dir="ltr">
  <span>Along with TypeScript 1.5 Alpha, we’re also releasing a <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin">preview of the Sublime Text plugin for TypeScript</a> to enable more developers to get editor support when authoring TypeScript.  This plugin works with both Sublime Text 2 and Sublime Text 3 and shows some of what is possible with the TypeScript type system. Sublime Text and the TypeScript plugin are available for OSX, Linux, and Windows.</span>
</p>

<p dir="ltr">
  <span><a href="https://devblogs.microsoft.com/00/00/01/56/67/4263.sublime2.png"><img src="https://devblogs.microsoft.com/00/00/01/56/67/4263.sublime2.png" alt="" width="281" height="143" border="0" /></a></span><em><br />TypeScript commands available in Sublime Text</em>
</p>

<p dir="ltr">
  <span>The Sublime Text plugin allows you to easily navigate, refactor, format, and investigate TypeScript code.  Those who tried the Sublime plugin that came with the ng-conf demo may also notice that this updated plugin is snappier, especially with larger files.</span>
</p>

<p dir="ltr">
  <span>We’re </span><span>excited to hear your feedback.  If you’d like to leave a comment, you can fill out an <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/issues">issue on the issue tracker</a></span><span>.</span><span>  Also, feel free to jump in and send us your <a href="https://github.com/Microsoft/TypeScript-Sublime-Plugin/pulls">pull requests</a> to help make the Sublime plugin even better.</span>
</p>

## <span>What’s next</span>

<p dir="ltr">
  <span>This </span><span>alpha release shows what will be possible in TypeScript 1.5 when it’s released, and we want to hear from you.  We’re working hard on TypeScript 1.5, and you can help us make it a strong release by trying it out and </span><a href="https://github.com/microsoft/typescript/issues"><span>sending us any issues</span></a><span> you find.</span>
</p>