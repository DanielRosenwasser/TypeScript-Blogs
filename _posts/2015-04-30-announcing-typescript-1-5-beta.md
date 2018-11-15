---
ID: 1672
post_title: Announcing TypeScript 1.5 Beta
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-5-beta/
published: true
post_date: 2015-04-30 11:00:00
---
<p dir="ltr">
  We have continued work on the TypeScript 1.5 release, and today we are one step closer.  You can now try out the TypeScript 1.5 beta as a download for <a href="https://visualstudiogallery.msdn.microsoft.com/3e5ba71c-abea-4d00-b81b-a62de3ad3d53">VS 2015 RC</a>, <a href="https://visualstudiogallery.msdn.microsoft.com/107f89a0-a542-4264-b0a9-eb91037cf7af">VS 2013</a>, <a href="https://www.npmjs.com/package/typescript">npm</a>, or as <a href="https://github.com/Microsoft/TypeScript">source</a>.The beta represents further progress to the final 1.5 release, with many bugfixes, support for a new metadata API that works with decorators, and updates to bring lib.d.ts up-to-date.
</p>

# Decorator Metadata TypeScript 1.5 beta introduces the new metadata API for working with decorators, allowing you to add and read metadata on declarations.  You can see how this works in the example below. First, you need to install the polyfill for the new metadata API.  We've made this available on NPM: 

<span style="font-family: 'courier new', courier">> npm install reflect-metadata</span> Next, you can use this metadata as in the example below: <span style="font-family: 'courier new', courier"><span style="color: #008000">///<reference path="node_modulesreflect-metadatareflect-metadata.d.ts"/></span><br /><br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> <span style="color: #800000">"reflect-metadata"</span>;<br /><br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> MyClassDecorator(value: <span style="color: #0000ff">string</span>) {<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">  return</span> <span style="color: #0000ff">function</span> (target: Function) {<br /></span><span style="font-family: 'courier new', courier">      Reflect.defineMetadata(<span style="color: #800000">"MyClassDecorator"</span>, value, target);<br /></span><span style="font-family: 'courier new', courier">  }<br /></span><span style="font-family: 'courier new', courier">}</span> <p dir="ltr">
  <span style="font-family: 'courier new', courier">@MyClassDecorator(<span style="color: #800000">"my metadata"</span>)</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> MyClass { }</span><br /><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> myClass = <span style="color: #0000ff">new</span> MyClass();</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">let</span> value: <span style="color: #0000ff">string</span> = Reflect.getMetadata(<span style="color: #800000">"MyClassDecorator"</span>, myClass.constructor);</span><br /><span style="font-family: 'courier new', courier">console.log(value); <span style="color: #008000">// outputs "my metadata"</span></span> You can compile this example using: <span style="font-family: 'courier new', courier">> tsc myFile.ts --module commonjs --target ES5</span> We've also added a new commandline option that will allow you to look at the types being passed into your decorators.   <span style="font-family: 'courier new', courier">> tsc myFile.ts --module commonjs --target ES5  --emitDecoratorMetadata</span> This is a being developed to help support the metadata needs of rich libraries, like the upcoming <a href="http://angular.io">Angular 2</a> and <a href="http://aurelia.io">Aurelia</a> releases, which can use the metadata to automatically set up features like dependency injection.
</p>

# Looking Ahead

Your feedback is important to us.  As we close in on completing the 1.5 release, your feedback will be crucial to helping us make sure the new features are at the highest quality they can be.  We'd love to [hear from you][1].

 [1]: https://github.com/microsoft/typescript/issues