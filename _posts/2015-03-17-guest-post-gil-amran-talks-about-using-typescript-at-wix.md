---
ID: 1670
post_title: 'Guest Post: Gil Amran talks about using TypeScript at Wix'
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/guest-post-gil-amran-talks-about-using-typescript-at-wix/
published: true
post_date: 2015-03-17 08:45:00
---
I'm pleased to share a contributed post from one of TypeScript's community members.  Today, guest writer Gil Amran from the [Wix][1] development team talks about using TypeScript to build WixStores, some of the advantages and challenges of using TypeScript, and what they learned doing so. A big "thanks!" to Gil for telling us about the process at Wix and for detailing his teams' experiences in this blog post.   <hr style="width: 200px" width="200" />

#### <span style="font-size: 2em">Developing Large-Scale Applications with TypeScript</span>

<p dir="ltr">
  <span>As a front-end developer, you’ve probably heard about TypeScript. Maybe you even tried using it. But not many developers know what it is like to build a large-scale project from scratch using TypeScript.</span> A year ago, with a team of eight front-end developers, we started rebuilding the entire Wix eCommerce solution; the new product is called WixStores. Most of the developers worked on the codebase at the same time. And working together without breaking one another’s code was a challenge. The WixStores application was divided into several small projects. We started using TypeScript for only one of the projects. The others used pure JavaScript. After seeing the benefits of TypeScript, we decided to use it for all of the projects.
</p>

# <span>Developing Large-Scale Applications Today</span>

<p dir="ltr">
  <span>Large-scale web applications are usually divided into several layers: the framework (such as AngularJS, Ember, or Backbone), the view (HTML with CSS, SASS, or LESS), and the language.</span> This blog post is focused on the language layer. Obviously, writing pure JavaScript is an option. But in a large-scale web application, it is less than ideal. Instead, there are languages that compile to JavaScript. These languages are very helpful as you will see next, and there are already a few of them. The most well known are CoffeeScript, ClojureScript, Dart, and TypeScript.  
</p>

# <span>So, Why Choose TypeScript?</span>

## <span>Technology Decoupling</span>

<p dir="ltr">
  <span>CoffeeScript and Dart are languages that compile to JavaScript. Unfortunately, the generated JavaScript does not look anything like the original code. It can be hard to understand, read, and debug. On the other hand, TypeScript is a superset of JavaScript. TypeScript generates JavaScript code that is easy to read and debug, and that looks very much like the original TypeScript code. This means that if (for any reason) you wish to return to plain JavaScript, you can take the generated JavaScript and work with it directly. In other words, there is no dependency on TypeScript, so it is easy to stop using it. With CoffeeScript and Dart it would be difficult to do that. </span>
</p>

## <span>Type Safety</span>

<p dir="ltr">
  <span>As the name suggests, TypeScript is a strongly typed language, and type safety is the most important added value that TypeScript offers. When creating a small to medium JavaScript application with one or two developers, it’s OK to go without any type safety. But when the application grows, that growth might lead to messy code that is very hard to maintain and debug.</span> Taking advantage of types means that you will get type errors at compile time instead of runtime (or not at all). And when I’m saying “types,” I don’t just mean numbers or strings, I mean interfaces with a very clear definition. For example: <span style="font-family: 'courier new', courier">interface CatalogAPI {</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">  productsCount  : number;</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">  defaultCategory  : Category;</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">  loadProductDetails(productId : string):ng.IPromise<DetailedProduct>;</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">  getProducts(maxProducts? : number):Product[];</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">  updateProduct(product : Product | DetailedProduct):boolean;</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">}</span> Here are three benefits of type safety:
</p>

### <span>It’s Optional</span>

<p dir="ltr">
  <span>Type safety is a wonderful feature, but you don’t have to use it if you don’t want to.</span> On the WixStores team, we were strict about types when defining a class API. We wanted to be sure that the developer who calls an API will know how to work with it. Any misuse of the API or breaking changes to the API will raise compile errors. Because TypeScript type safety is optional, in the internal function implementation, we allowed the freedom to work with no types. It was up to each developer to decide whether to use it.
</p>

### <span>It’s Just Another Test</span>

<p dir="ltr">
  <span>The result of TypeScript’s compilation is of course JavaScript. The most obvious difference between the TypeScript code and the generated code is that the types have been removed—they are used during the compilation phase only. You can think of it as another test that runs at build time to verify that all the function calls are valid. We nicknamed it WarningScript. :-)</span>
</p>

### <span>API Definitions and External Libraries</span>

<p dir="ltr">
  <span>Interfaces are great for describing APIs. For example:</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">function listProducts(products) { ... }</span>
</p>

<p dir="ltr">
  <span>This function definition does not tell you whether </span><span>products</span><span> is a list of actual product objects or a list of product IDs. Also, you cannot tell whether this function returns a result. The only way to get this information is to investigate the actual code, which wastes time.</span> On the other hand, an API like this:
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">function listProducts(products:Product[]):Product[] { ... }</span>
</p>

<p dir="ltr">
  <span>Tells you at a glance that </span><span>products</span><span> is an array of </span><span>Product </span><span>objects. The result is an array of </span><span>Product</span><span> objects. As you can see, the API can fully describe the function implementation without having to investigate the implementation. And most IDEs can work with this information and will give you real code completion and not just an estimation.</span> Working with external libraries like Underscore or jQuery might also be a challenge; you can look up the API by searching the web or investigating the library code. But when working with types it is much easier to find out how to work with the library correctly. You can find definition files for almost any library at <a href="https://github.com/borisyankov/DefinitelyTyped">DefinitelyTyped</a>. Again, IDE code completion and compile-time type checking will use this information to alert you of any misuse of the API. While it is possible to address this problem by annotating code with JSDoc-style comments, this approach relies on developers to read the comments to understand API contracts. However, comments cannot truly replace tooling support to guard against lack of types.
</p>

## <span>ES6</span>

<p dir="ltr">
  <span>ECMAScript 6 is a great leap forward for the JavaScript language. There are loads of cool features, and the code is much more organized and readable.</span> So far, TypeScript implemented a few of its features like modules, classes, arrow functions, and more. TypeScript (see the <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">roadmap</a>) is closing the gap even further and will include almost all ES6 features with the addition of metadata and annotations. This means that you can use these features today and the compiler will generate the required code (same as <a href="https://github.com/google/traceur-compiler">Traceur</a>).
</p>

## <span>Community</span>

<p dir="ltr">
  <span>TypeScript was created about three years ago and is constantly being updated. Lately, TypeScript is getting more and more attention. The AngularJS team and Microsoft recently </span><a href="http://blogs.msdn.com/b/somasegar/archive/2015/03/05/typescript-lt-3-angular.aspx"><span>announced</span></a><span> that Angular 2 will be written in TypeScript. Facebook is also pushing in the same direction with </span><a href="http://flowtype.org"><span>F</span></a><a href="http://flowtype.org/"><span>low</span></a><span>, an alternate static type checker for JavaScript.</span>
</p>

<p dir="ltr">
  <span><br /></span>
</p>

# Can It Be That Good? What Are the Cons?

## <span>Boilerplate Code</span>

<p dir="ltr">
  <span>Working with types, classes, and interfaces can lead to over-engineering. Many Java developers feel that they waste too much time on boilerplate code just to define a class. This can also happen in TypeScript if you are not careful. When a language gives you tools, you can abuse them.</span> Generics (type safety feature) are a good example. We did not create our own classes that use generics, but we did use generics from external libraries like AngularJS Promises.
</p>

## <span>IDE Support</span>

<p dir="ltr">
  <span>There are plugins for almost all the common IDEs, including Sublime, IntelliJ, WebStorm, Vim, and even Emacs. But they are not perfect. Sometimes you will see an error in the IDE but not at compile time (in other words, a bug in the IDE plugin). In addition, you would expect the IDE to take advantage of the type definitions to let the developer navigate more easily within the code. But some IDEs do not do it.</span> On the plus side, the TypeScript team is now working on better tooling support and soon we will see much better IDE support.
</p>

## <span>Popularity </span>

<p dir="ltr">
  <span>Despite all the attention that TypeScript is getting lately, it is gaining popularity, so it’s not easy to find solutions to problems. For example, at first I didn’t understand whether to use internal or external modules, and I could not find enough information online about this topic. However, I expect that as more developers begin using TypeScript, more solutions will be available via Stack Overflow, Google, and other online resources. </span> Also, some developers may need to learn how to use the language, unless they are up to date with ES6 or came from .NET.
</p>

# <span>Conclusion</span>

<p dir="ltr">
  <span>As I’ve mentioned throughout, large-scale web applications are not easy to deal with when using plain JavaScript. Yes, ES6 will help organize large projects (any front-end developer should get familiar with it as soon as he can), but this is not enough for large-scale applications. Type safety can help reduce the time wasted on simple human errors, and much like TDD, it gives you the comfort of knowing that your code is covered by tests.</span> From type safety, ES6 support, and the current attention that TypeScript is getting, to the simplicity of reading and debugging the JavaScript code it generates, TypeScript is currently the best pick for large-scale applications. I don’t see us going back to coding in plain JavaScript. So, if your team is larger than two or three people, give TypeScript a try when developing your next large-scale web application. If you don’t like it, just use the generated code.
</p>

 [1]: http://www.wix.com/