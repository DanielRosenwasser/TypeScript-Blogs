---
ID: 1668
post_title: 'TypeScript 1.4 sneak peek: union types, type guards, and more'
author: Ryan Cavanaugh
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescript-1-4-sneak-peek-union-types-type-guards-and-more/
published: true
post_date: 2014-11-18 11:30:00
---
With TypeScript 1.3 out the door, we're focused on adding more type system and ECMAScript 6 features to TypeScript. Let's take a quick look at some of the new features you'll be able to use in the next release of TypeScript. All these features are live in the master branch on our [GitHub repository][1] if you'd like to check them out yourself today. 

With these features, we can more accurately and easily work with variables and expressions that may have different types at runtime. Together, these features help reduce the need for explicit type annotations, type assertions, and use of the 'any' type. Type definition file (.d.ts) authors can use these to more precisely describe external libraries. For those following the development of the compiler, you'll notice we're already using these features in the compiler today.

## Union types

### Overview

Union types are a powerful way to express a value that can be one of several types. For example, you might have an API for executing a program that takes a command-line as either a string or a string[]. You can now write:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">interface</span> <span style="color: #3366ff">RunOptions</span> {</span>  
<span style="font-family: 'courier new', courier">  program: <span style="color: #0000ff">string</span>;</span>  
<span style="font-family: 'courier new', courier">  commandline: <span style="color: #0000ff">string</span>[]|<span style="color: #0000ff">string</span>;</span>  
<span style="font-family: 'courier new', courier">}</span>

Assignment to union types works very intuitively. Anything you could assign to one of the union type's members is assignable to the union:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> opts: <span style="color: #3366ff">RunOptions</span> = <span style="color: #008000">/* ... */</span>;</span>  
<span style="font-family: 'courier new', courier">opts.commandline = <span style="color: #800000">'-hello world'</span>; <span style="color: #008000">// OK</span></span>  
<span style="font-family: 'courier new', courier">opts.commandline = [<span style="color: #800000">'-hello', 'world'</span>]; <span style="color: #008000">// OK</span></span>  
<span style="font-family: 'courier new', courier">opts.commandline = [42]; <span style="color: #008000">// Error, number is not string or string[]</span></span>

When reading from a union type, you can see any properties that are shared by them:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">if</span>(opts.commandline.length === 0) { <span style="color: #008000">// OK, string and string[] both have 'length' property</span></span>  
<span style="font-family: 'courier new', courier">  console.log(<span style="color: #800000">"it's empty"</span>);</span>  
<span style="font-family: 'courier new', courier">}</span>

Using *type guards*, you can easily work with a variable of a union type:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> formatCommandline(c: <span style="color: #0000ff">string<span>[]</span></span>|<span style="color: #0000ff">string</span>) {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">  if</span>(<span style="color: #0000ff">typeof</span> c === <span style="color: #800000">'string'</span>) {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">    return</span> c.trim();</span>  
<span style="font-family: 'courier new', courier">  } <span style="color: #0000ff">else</span> {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">    return</span> c.join(<span style="color: #800000">' '</span>);</span>  
<span style="font-family: 'courier new', courier">  }</span>  
<span style="font-family: 'courier new', courier">}</span>

## Type Guards

A common pattern in JavaScript is to use <span style="font-family: 'courier new', courier">typeof</span> or <span style="font-family: 'courier new', courier">instanceof</span> to examine the type of an expression at runtime. TypeScript now understands these conditions and will change type inference accordingly when used in an <span style="font-family: 'courier new', courier">if</span> block.

Using <span style="font-family: 'courier new', courier">typeof</span> to test a variable:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> x: <span style="color: #0000ff">any</span> = <span style="color: #008000">/* ... */</span>;<br /><span style="color: #0000ff">if</span>(<span style="color: #0000ff">typeof</span> x === <span style="color: #800000">'string'</span>) {<br />   console.log(x.subtr(1)); <span style="color: #008000">// Error, 'subtr' does not exist on 'string'</span><br />}<br /><span style="color: #008000">// x is still any here</span><br />x.unknown(); <span style="color: #008000">// OK</span></span>

**EDIT**: The above example does not give the error mentioned in the 1.4 release.  This will likely be improved in future releases.

Using <span style="font-family: 'courier new', courier">instanceof</span> with classes and union types:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> <span style="color: #3366ff">Dog</span> { woof() { } }<br /><span style="color: #0000ff">class</span> <span style="color: #3366ff">Cat</span> { meow() { } }<br /><span style="color: #0000ff">var</span> pet: <span style="color: #3366ff">Dog</span>|<span style="color: #3366ff">Cat</span> = <span style="color: #008000">/* ... */</span>;<br /><span style="color: #0000ff">if</span>(pet <span style="color: #0000ff">instanceof</span> <span style="color: #3366ff">Dog</span>) {<br />   pet.woof(); <span style="color: #008000">// OK</span><br />} <span style="color: #0000ff">else</span> {<br />   pet.woof(); <span style="color: #008000">// Error</span><br />}</span>

### Stricter Generics

With union types able to represent a wide range of type scenarios, we've decided to improve the strictness of certain generic calls. Previously, code like this would (surprisingly) compile without error:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> equal<<span style="color: #3366ff">T</span>>(lhs: <span style="color: #3366ff">T</span>, rhs: <span style="color: #3366ff">T</span>): <span style="color: #0000ff">boolean</span> {<br />   <span style="color: #0000ff">return</span> lhs === rhs;<br />}<br /><br /><span style="color: #008000">// Previously: No error</span><br /><span style="color: #008000">// New behavior: Error, no best common type between 'string' and 'number'</span><br /><span style="color: #0000ff">var</span> e = equal(42, <span style="color: #800000">'hello'</span>);</span>

### Better Type Inference

Union types also allow for better type inference in arrays and other places where you might have multiple kinds of values in a collection:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> x = [1, <span style="color: #800000">'world'</span>]; <span style="color: #008000">// x: Array<string|number></span><br />x[0] = <span style="color: #800000">'hello'</span>; <span style="color: #008000">// OK</span><br />x[0] = false; <span style="color: #008000">// Error, boolean is not string or number</span></span>

## Type Aliases

You can now define an alias for a type using the <span style="font-family: 'courier new', courier">type</span> keyword:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">type</span> PrimitiveArray = <span style="color: #3366ff">Array</span><<span style="color: #0000ff">string</span>|<span style="color: #0000ff">number</span>|<span style="color: #0000ff">boolean</span>>;<br /><span style="color: #0000ff">type</span> MyNumber = <span style="color: #0000ff">number</span>;<br /><span style="color: #0000ff">type</span> NgScope = ng.<span style="color: #3366ff">IScope</span>;<br /><span style="color: #0000ff">type</span> Callback = () => <span style="color: #0000ff">void</span>;</span>

Type aliases are exactly the same as their original types; they are simply alternative names.

In an upcoming post, we'll talk about ECMAScript 6 features that we're bringing into TypeScript. To learn more, and to try out the features mentioned in this blog post, check out the 'master' branch on the [TypeScript GitHub site][1], and let us know what you think.

 [1]: http://www.github.com/Microsoft/TypeScript