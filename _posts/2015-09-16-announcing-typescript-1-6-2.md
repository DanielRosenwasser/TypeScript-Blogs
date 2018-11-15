---
ID: 1675
post_title: Announcing TypeScript 1.6
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-6-2/
published: true
post_date: 2015-09-16 13:00:00
---
<span>Today, we're happy to announce the release of TypeScript 1.6.  This release adds support <span>for React/JSX, class expressions, and a rich set of new capabilities in the type system. It also provides stricter type checking for object literals.</span></span>

<span>You can download TypeScript 1.6 for <a title="TypeScript 1.6 for VS2015" href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS1.6.2-D14OOB.23313.00/TypeScript_Full.exe">Visual Studio 2015</a>, <a title="TypeScript 1.6 for VS2013" href="http://download.microsoft.com/download/4/4/3/443F86B7-A89F-48E6-AC96-0AAC2A910A29/TS1.6.2-VUOOB.40914.00/TypeScript_Dev12.exe">Visual Studio 2013</a>, on <a title="TypeScript npm package" href="https://www.npmjs.com/package/typescript">npm</a>, or as <a title="TypeScript 1.6 source code" href="https://github.com/Microsoft/TypeScript/releases/tag/v1.6.2">source</a>.</span>

# <span>React/JSX</span>

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/5282.react.png" alt="" width="200" />][1]

<span>Designed with feedback from React experts and the React team, we've built full-featured support for React typing and the JSX support in React.  </span>Below, you can see TypeScript code happily coexisting with JSX syntax within a single file with the new .tsx extension. This allows React developers to intermingle HTML-like syntax with TypeScript code.

<span><a href="https://devblogs.microsoft.com/00/00/01/56/67/2500.6177.Renaming.gif"><img src="https://devblogs.microsoft.com/00/00/01/56/67/2500.6177.Renaming.gif" alt="" border="0" /></a></span>

Our goal was to make it feel natural to work with React/JSX and to have all the type-checking and autocomplete capabilities of TypeScript.  This allows you a rich editing experience for working with React and JSX when using VS, VS Code, and Sublime. 

# <span>Class expressions</span>

This release also makes it possible to write class expressions, as we continue to round out the ES6 support in TypeScript.  Similar to class declarations, class expressions allow you to create new classes.  Unlike class declarations, you can use class expressions wherever you use an expression.  For example, you can now create a class and use it in your <span style="font-family: 'courier new', courier">extends</span> clause.

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> StateHandler <span style="color: #0000ff">extends</span> class { reset() { <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>; } } {<br />   constructor() {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">     super</span>();</span>  
<span style="font-family: 'courier new', courier">   }</span>  
<span style="font-family: 'courier new', courier">}</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> g = <span style="color: #0000ff">new</span> StateHandler();</span>  
<span style="font-family: 'courier new', courier">g.reset();</span>

<span>This class can be anonymous and still has all the same capabilities of class declarations. </span>

# User defined type guards

In earlier versions of TypeScript, you could use <span style="font-family: 'courier new', courier">if </span>statements to narrow the type. For example, you could use:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">if</span> (<span style="color: #0000ff">typeof</span> x === <span style="color: #800000">"number"</span>) { … }</span>

<span>This helped type information flow into common ways of working with types at runtime (inspired by some of the other projects doing typechecking of JS). While this approach is powerful, we wanted to push it a bit further.  In 1.6, you can now create your own type guard functions:</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">interface</span> Animal {name: <span style="color: #0000ff">string</span>; }<br /><span style="color: #0000ff">interface</span> Cat <span style="color: #0000ff">extends</span> Animal { meow(); }</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> isCat(a: Animal): a <span style="color: #0000ff">is</span> Cat {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  return</span> a.name === <span style="color: #800000">'kitty'</span>;</span>  
<span style="font-family: 'courier new', courier">}</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> x: Animal;</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">if</span>(isCat(x)) {</span>  
<span style="font-family: 'courier new', courier">  x.meow(); <span style="color: #008000">// OK, x is Cat in this block</span></span>  
<span style="font-family: 'courier new', courier">}</span>

<span>This allows you to work with not only <span style="font-family: 'courier new', courier">typeof</span> and <span style="font-family: 'courier new', courier">instanceof</span> checks, which need a type that JavaScript understands, but now you can work with interfaces and do custom analysis.  Guard functions are denoted by their “<span style="font-family: 'courier new', courier">a is X</span>” return type, which returns boolean and signals to the compiler if what the expected type now is.</span>

# Intersection types

<span>Common patterns in JS that haven’t been easy to express in TypeScript are mixins and extending existing classes with new methods.  To help with this, we’re adding a new type operator ‘&’ that will combine two types together.  While it was possible to do this before by creating a new interface that inherited from two other types, this tended to be clunky and couldn’t be used in the case you wanted to use it most: combining generic types.</span>

This new & operator, called intersection, creates anonymous combinations of types.

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> extend<T, U>(first: T, second: U): T & U {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  let</span> result = <T & U> {};</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  for</span> (<span style="color: #0000ff">let</span> id <span style="color: #0000ff">in</span> first) {</span>  
<span style="font-family: 'courier new', courier">    result[id] = first[id];</span>  
<span style="font-family: 'courier new', courier">  }</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  for</span> (<span style="color: #0000ff">let</span> id <span style="color: #0000ff">in</span> second) {</span>  
<span style="font-family: 'courier new', courier">    if (!result.hasOwnProperty(id)) {</span>  
<span style="font-family: 'courier new', courier">      result[id] = second[id];</span>  
<span style="font-family: 'courier new', courier">    }</span>  
<span style="font-family: 'courier new', courier">  }</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  return</span> result;</span>  
<span style="font-family: 'courier new', courier">}</span>

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> x = extend({ a: <span style="color: #800000">"hello"</span> }, { b: 42 });</span>  
<span style="font-family: 'courier new', courier">x.a; <span style="color: #008000">// works</span></span>  
<span style="font-family: 'courier new', courier">x.b; <span style="color: #008000">// works </span></span>

<div>
  <h1>
    Abstract classes
  </h1>
  
  <p>
    <span>A </span><a href="http://typescript.codeplex.com/workitem/list/basic?field=Votes&direction=Descending&issuesToDisplay=All&keywords=&emailSubscribedItemsOnly=false"><span>long-standing feature request</span></a><span> for TypeScript has been supporting abstract classes.  Similar in some ways to interfaces, abstract classes give you a way of creating a base class, complete with default implementations, that you can build from with the intention of it never being used directly outside of the class hierarchy. </span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">abstract</span> <span style="color: #0000ff">class</span> A {<br />  foo(): <span style="color: #0000ff">number</span> { <span style="color: #0000ff">return</span> this.bar(); }</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">  abstract</span> bar(): <span style="color: #0000ff">number</span>;</span><br /><span style="font-family: 'courier new', courier">}</span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> a = <span style="color: #0000ff">new</span> A();  <span style="color: #008000">// error, Cannot create an instance of the abstract class 'A'</span></span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> B <span style="color: #0000ff">extends</span> A {<br />  bar() { <span style="color: #0000ff">return</span> 1; }</span><br /><span style="font-family: 'courier new', courier">}</span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> b = <span style="color: #0000ff">new</span> B();  <span style="color: #008000">// success, all abstracts are defined</span></span>
  </p>
  
  <h1>
    Generic type aliases
  </h1>
  
  <p>
    Leading up to TypeScript 1.6, type aliases were restricted to being simple aliases that shortened long type names.  Unfortunately, without being able to make these generic, they had limited use.  We now allow type aliases to be generic, giving them full expressive capability.
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">type</span> switcharoo<T, U> = (u: U, t:T)=>T;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> f: switcharoo<<span style="color: #0000ff">number</span>, <span style="color: #0000ff">string</span>>;</span><br /><span style="font-family: 'courier new', courier">f(<span style="color: #800000">"bob"</span>, 4);</span>
  </p>
  
  <h1>
    Potentially-breaking changes
  </h1>
  
  <p>
    Along with all the new features, we also spent some time tightening and fixing up a few areas of the system to generally work better, help you catch more errors, and be closer to how other tools work.  In this release we now require that an object literal should be matched directly.  We’ve also updated the module resolution logic to work more closely to what you would expect for the type of output module you selected.  You can read the <a href="https://github.com/Microsoft/TypeScript/wiki/Breaking-Changes">full list of potential breaking changes</a> with their mitigations.
  </p>
  
  <h2>
    Object literal strictness
  </h2>
  
  <p>
    We’ve made object compatibility stricter to help catch a class of common bugs.  You can learn more about this in the <a href="http://blogs.msdn.com/b/typescript/archive/2015/09/02/announcing-typescript-1-6-beta-react-jsx-better-error-checking-and-more.aspx">1.6 beta post</a>.
  </p>
  
  <h2>
    Improvements to module resolution
  </h2>
  
  <p>
    We’ve changed module resolution when doing CommonJS output to work more closely to how Node does module resolution.  If a module name is non-relative, we now follow these steps to find the associated typings:
  </p>
  
  <ol>
    <li>
      Check in <span style="font-family: 'courier new', courier">node_modules</span> for<span style="font-family: 'courier new', courier"> <module name>.d.ts</span>
    </li>
    <li>
      Search <span style="font-family: 'courier new', courier">node_modules\<module name>\package.json</span> for a typings field
    </li>
    <li>
      Look for <span style="font-family: 'courier new', courier">node_modules\<module name>\index.d.ts</span>
    </li>
    <li>
      Then we go one level higher and repeat the process
    </li>
  </ol>
  
  <p>
    <strong>Please note</strong>: when we search through node_modules, we assume these are the packaged node modules which have type information and a corresponding .js file.  As such, we resolve only .d.ts files (not .ts file) for non-relative names.
  </p>
  
  <p>
    Previously, we  treated all module names as relative paths, and therefore we would never properly look in node_modules.  If you prefer the previous behavior, you can use the compiler flag “--moduleResolution classic”.  We will continue to improve module resolution, including improvements to AMD, in upcoming releases.
  </p>
  
  <h1>
    Looking ahead
  </h1>
  
  <p>
    We’re excited to reveal all the new improvements to TypeScript and to hear your <a href="https://github.com/microsoft/typescript/issues">feedback</a>.  There’s lots to come, with more ES6 and ES7 features ahead.  And we’re always open if you want to <a href="https://github.com/Microsoft/TypeScript/pulls">jump in</a>.
  </p>
</div>

 [1]: https://devblogs.microsoft.com/00/00/01/56/67/5282.react.png