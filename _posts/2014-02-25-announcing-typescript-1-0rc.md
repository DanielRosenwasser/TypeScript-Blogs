---
ID: 113
post_title: Announcing TypeScript 1.0RC
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-0rc/
published: true
post_date: 2014-02-25 08:08:00
---
We’re happy to mark another significant milestone for the TypeScript project with the release of the 1.0 Release Candidate.  Since the first release in October of 2012, the TypeScript language has grown to support generics, which enables rich typing of JavaScript libraries.  Last December, the release focused on better performance and reliability for larger codebases.  Today, we're making the 1.0RC release available, which represents the culmination of this work as a feature-complete TypeScript 1.0 language with a spec-conformant compiler and a production-level language service capable of working with codebases with hundreds of thousands of lines of code.

For developers using Visual Studio 2013, the Spring Update CTP2 includes TypeScript support, making TypeScript a first-class Visual Studio language.  The TypeScript 1.0RC release is also immediately available for Visual Studio 2012 and 2013 development as a [standalone power tool][1] and as a cross-platform tool via the [npm package][1].  As always, you can access the source via [CodePlex][2].  *(please note: The 1.0RC release is also called '0.9.7' in the installer and compiler)*

Among the usual set of bugfixes, TypeScript 1.0RC focuses on a few key areas based on user feedback on 0.9.5: a simpler type system, an improved lib.d.ts, and a more natural declaration merge order.

# Simpler Type System

For the 1.0RC release, we’ve simplified the type system in two ways: the ‘any’ type now works more consistently as a wildcard, and we’ve also made generics work better with complex types, informed by real-world user examples trying to type JavaScript promises.  By making these simplifications, the type system becomes easier to use and easier to reason about.

As an example, the TypeScript 1.0RC type system now allows the user to use ‘any’ as a way around typechecking when subclassing and implementing interfaces, in addition to being able to use it as a wildcard to satisfy generic constraints.  

<p style="padding-left: 30px">
  <span style="font-family: courier new,courier"><span style="color: #0000ff">class</span> Shape {</span><br /><span style="font-family: courier new,courier">    area: <span style="color: #0000ff">number</span>;</span><br /><span style="font-family: courier new,courier">}</span>
</p>

<p style="padding-left: 30px">
  <span style="font-family: courier new,courier"><span style="color: #0000ff">class</span> ImaginaryShape <span style="color: #0000ff">extends</span> Shape {</span><br /><span style="font-family: courier new,courier">    area: <span style="color: #0000ff">any</span>;  <span style="color: #008000">// causes errors in 0.9.5, but valid in 1.0RC</span></span><br /><span style="font-family: courier new,courier">}</span>
</p>

Second, we’ve made it easier to understand when generic types are compatible.  We’ve replaced the more complex, and error-prone, approach of 0.9.5 with the simpler rule that a generic type is compatible with another type if, once instantiated with ‘any’ for all generic type parameters, the resulting type is compatible.  This allows greater flexibility when working with the types of complex generic types that occur in common JavaScript patterns, including fully-typed generic promise typings, as in this example.

<p style="padding-left: 30px">
  <span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> Promise<Value> {</span><br /><span style="font-family: courier new,courier">    result: Value;</span><br /> <br /><span style="font-family: courier new,courier">    then<T2>(f: (v: Value) => Promise<T2>): Promise<T2>;</span><br /><span style="font-family: courier new,courier">    then<T2>(f: (v: Value) => T2): Promise<T2>;</span><br /><span style="font-family: courier new,courier">}</span><br /> <br /><span style="font-family: courier new,courier"><span style="color: #0000ff">class</span> PromiseImpl<Value> <span style="color: #0000ff">implements</span> Promise<Value> {</span><br /> <br /><span style="font-family: courier new,courier">    result: Value;</span><br /> <br /><span style="font-family: courier new,courier">    then<T2>(f: (v: Value) => Promise<T2>): Promise<T2>;</span><br /><span style="font-family: courier new,courier">    then<T2>(f: (v: Value) => T2): Promise<T2>;</span><br /><span style="font-family: courier new,courier">    then<T2>(f: (v: Value) => <span style="color: #0000ff">any</span>): Promise<T2> {</span><br /><span style="font-family: courier new,courier"><span style="color: #0000ff">        return</span> undefined;</span><br /><span style="font-family: courier new,courier">    }</span><br /><span style="font-family: courier new,courier">}</span>
</p>

# Declaration Merging

We’ve also made declaration merging of overloads use a more natural order.  Starting with TypeScript 1.0RC, overloads from later interfaces will merge before overloads of earlier interfaces.  This allows you to reference a library like jQuery and then follow it with references to the jQuery plugins you want to use, and the jQuery plugins would now get the expected higher precedence during overload resolution, as in an example like this:

<p style="padding-left: 30px">
  <span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> MyLib {</span><br /><span style="font-family: courier new,courier">    handleInput(x: <span style="color: #0000ff">number</span>): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">    handleInput(x: <span style="color: #0000ff">string</span>): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">}</span><br /><span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> MyLib {</span><br /><span style="font-family: courier new,courier">    handleInput(x: Date): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">    handleInput(x: { abc: <span style="color: #0000ff">string</span> }): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">}</span>
</p>

effectively becomes this definition when merged:

<p style="padding-left: 30px">
  <span style="font-family: courier new,courier"><span style="color: #0000ff">interface</span> MyLib {</span><br /><span style="font-family: courier new,courier">    handleInput(x: Date): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">    handleInput(x: { abc: <span style="color: #0000ff">string</span> }): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">    handleInput(x: <span style="color: #0000ff">number</span>): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">    handleInput(x: <span style="color: #0000ff">string</span>): <span style="color: #0000ff">void</span>;</span><br /><span style="font-family: courier new,courier">}</span>
</p>

This new merge order also makes it easier to extend the built-in types and overloads in lib.d.ts, where the user simply needs to add the new overloads using declaration merging without having to change how lib.d.ts is initially referenced.

# Improvements to lib.d.ts

A key value of TypeScript is that it offers a rich set of typings out of the box for JavaScript and DOM APIs via the lib.d.ts file that ships as part of TypeScript.  With TypeScript 1.0RC, we’ve updated lib.d.ts to include typings for touch and WebGL development.  This helps brings it up-to-date with the advanced HTML5 features available across platforms and devices.

# Path to 1.0

It’s been an amazing journey, and your feedback on the project has helped ensure that TypeScript is a high-quality JavaScript development experience.  We’d like to give a huge “thank you!” to the community that’s grown up around TypeScript as it has matured.  TypeScript 1.0RC is a mature toolset, shaped by a community of users that have used TypeScript to build a rich variety of applications.  We’d love to hear your feedback on this release candidate to help us make TypeScript 1.0 the best it can be.

 [1]: http://www.typescriptlang.org/#Download
 [2]: https://typescript.codeplex.com/SourceControl/latest