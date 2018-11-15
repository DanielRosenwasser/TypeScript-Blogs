---
ID: 1824
post_title: Announcing TypeScript 1.4
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-4/
published: true
post_date: 2015-01-16 10:30:00
---
Today we're happy to announce TypeScript 1.4. With TypeScript 1.4, we've continued to build new features that help you work with more JavaScript patterns, create richer typings, and use new ES6 features.

You can try these improvements out as part of [Visual Studio 2015 CTP5][1], [Visual Studio 2013][2], [NPM][3], and as [source][4].

# Type System Improvements

## Union Types

JavaScript functions may take a number of possible argument types. Up to now, we’ve supported this using function overloads. Starting with TypeScript 1.4, we’ve generalized this capability and now allow you to specify that that a value is one of a number of different types using a union type:

<pre><span style="color: #0603d8">function</span> <span style="color: black">f</span>(<span class="pl-vpf">x</span>: <span style="color: #0603d8">number</span> | <span style="color: #0603d8">number</span>[]) {
  <span style="color: #0603d8">if</span> (<span style="color: #0603d8">typeof</span> x <span style="color: #0603d8">===</span> <span style="color: #901319"><span style="color: #901319">"</span>number<span style="color: #901319">"</span></span>) {
    <span style="color: #0603d8">return</span> x <span style="color: #0603d8">+</span> <span class="pl-c1">10</span>;
  }
  <span style="color: #0603d8">else</span> {
    <span style="color: #006a0a">// return sum of numbers</span>
  }
}</pre>

Once you have a value of a union type, you can use a typeof and instanceof checks to use the value in a type-safe way. You'll notice we use this in the above example and can treat x as a number type inside of the if-block.

Union types are a new kind of type and work any place you specify a type.

## Type Aliases

You can now define an alias for a type using the type keyword:

<pre><span style="color: #0000ff">type</span> PrimitiveArray <span style="color: #0603d8">=</span> <span class="pl-s3">Array</span><span style="color: #0603d8">&lt;</span><span style="color: #0603d8">string</span>|<span style="color: #0603d8">number</span>|<span style="color: #0603d8">boolean</span><span style="color: #0603d8">&gt;</span>;
<span style="color: #0000ff">type</span> MyNumber <span style="color: #0603d8">=</span> <span style="color: #0603d8">number</span>;
<span style="color: #0000ff">type</span> NgScope <span style="color: #0603d8">=</span> ng.IScope;
<span style="color: #0000ff">type</span> Callback <span style="color: #0603d8">=</span> () <span style="color: #0603d8">=&gt;</span> <span style="color: #0603d8">void</span>;</pre>

Type aliases are exactly the same as their original types; they are simply alternative names. You can use these aliases to better document your code and aid readability.

## Const Enums

For heavy uses of enums, it’s helpful to have an even more restricted form that we know is safe to always inline. This helps with performance, code size, and with working with cases where the enum aliases themselves may not be exported. Starting with TypeScript 1.4, you’ll be able to make const enums.

<pre><span class="pl-s" style="color: #0000ff">const </span><span style="color: #0603d8">enum</span> Color { Blue, Green, Red };
<span style="color: #0603d8">var</span> c <span style="color: #0603d8">=</span> Color.Blue;</pre>

When compiled, the const enum is removed, leaving ‘var c = 0 /* Blue */’.

## And more…

You can get also more information and examples about updates to the type system on our [TypeScript 1.4 sneak peek blog post][5].

# ECMAScript 6 Support

In addition to the type system improvements, one of the main goals for the upcoming TypeScript 2.0 release is to fully support the ECMAScript 6 standard. With TypeScript 1.4, we take another step towards this goal. In this release, we’ve added a new ES6 output mode, support for let and const, and support for ES6 template strings.

## ES6 output mode

In previous TypeScript releases, we included a ‘--target' commandline option that allows you to choose between ES3 and ES5 output modes. We’ve added a new ‘ES6’ option to the targets commandline option:

    > tsc --target ES6 myfile.js 

This option will be required for ES6 features that cannot be output to ES3 or ES5, just as ES5-only features, such as getters and setters, have required the ES5 target option.

We’re in the process of updating the existing language features from ES6, such as lambdas and classes, to support ES6 mode by outputting their native forms.  You may need to install an [additional .d.ts file][6] for this to work.

## Let/Const

The ES6 ‘let’ feature is similar to ‘var’, but it aims to simplify the mental model for the variable’s scope. With ‘let’, you can scope variables to blocks of code rather than whole functions. For example:

<pre><span style="color: #0603d8">function</span> <span style="color: black">f</span>() {
  <span style="color: #0000ff">let</span> total <span style="color: #0603d8">=</span> <span class="pl-c1">0</span>;
  <span style="color: #0000ff">let</span> x <span style="color: #0603d8">=</span> <span class="pl-c1">5</span>;
  <span style="color: #0603d8">for</span> (let x <span style="color: #0603d8">=</span> <span class="pl-c1">1</span>; x <span style="color: #0603d8">&lt;</span> <span class="pl-c1">10</span>; x<span style="color: #0603d8">++</span>) {
    total <span style="color: #0603d8">+=</span> x;
  }
  <span style="color: black">console</span>.<span class="pl-s3">log</span>(x);
}

f(); <span style="color: #006a0a">// outputs 5</span></pre>

Notice how the two ‘let x’ statements do not conflict. This is because the one used in the loop is in a different scope than the one outside of the loop. If we re-wrote this using vars, all ‘x’ vars effectively combine into one, leading to rather confusing output.

<pre><span style="color: #0603d8">function</span> <span style="color: black">f</span>() {
  <span style="color: #0603d8">var</span> total <span style="color: #0603d8">=</span> <span class="pl-c1">0</span>;
  <span style="color: #0603d8">var</span> x <span style="color: #0603d8">=</span> <span class="pl-c1">5</span>;
  <span style="color: #0603d8">for</span> (<span style="color: #0603d8">var</span> x <span style="color: #0603d8">=</span> <span class="pl-c1">1</span>; x <span style="color: #0603d8">&lt;</span> <span class="pl-c1">10</span>; x<span style="color: #0603d8">++</span>) {
    total <span style="color: #0603d8">+=</span> x;
  }
  <span style="color: black">console</span>.<span class="pl-s3">log</span>(x);
}

f(); <span style="color: #006a0a">// outputs 10</span></pre>

TypeScript now supports using ‘let’ and ‘const’ in addition to ‘var’. These currently require the ES6 output mode, but we’re are investigating relaxing this restriction in future versions.

## Template Strings

ECMAScript 6 has further improved on string interpolation in JavaScript by adding template strings. These special strings can freely mix in expressions, allowing a lighter syntax when pieces of a string depend on associated values:

<pre><span style="color: #0603d8">var</span> rectangle <span style="color: #0603d8">=</span> { height: <span class="pl-c1">20</span>, width: <span class="pl-c1">10</span> };
<span style="color: #0603d8">var</span> areaMessage <span style="color: #0603d8">=</span> <span style="color: #800000">`Rectangle area is </span><span style="color: #0603d8">$</span>{rectangle.<span class="pl-sc">height</span> <span style="color: #0603d8">*</span> rectangle.<span class="pl-sc">width</span>}<span style="color: #800000">`</span>;</pre>

With the 1.4 release, TypeScript now supports ES6 template strings and can also compile them down to ES3/ES5 expressions.

# Looking Ahead

We’re excited to bring new features into TypeScript, including more ECMAScript 6 features and our work on [async/await][7].  As always, we’d love to hear your feedback. You can get involved by trying out the release, sending us a [bug][8], playing with the [source][9], and sending us a [pull request][10].

 [1]: http://go.microsoft.com/fwlink/?LinkId=400496
 [2]: https://visualstudiogallery.msdn.microsoft.com/2d42d8dc-e085-45eb-a30b-3f7d50d55304
 [3]: https://www.npmjs.com/package/typescript
 [4]: https://github.com/Microsoft/TypeScript
 [5]: http://blogs.msdn.com/b/typescript/archive/2014/11/18/what-s-new-in-the-typescript-type-system.aspx
 [6]: https://github.com/Microsoft/TypeScript/issues/1696
 [7]: https://github.com/Microsoft/TypeScript/issues/1664
 [8]: https://github.com/microsoft/typescript/issues
 [9]: https://github.com/microsoft/typescript
 [10]: https://github.com/microsoft/typescript/pulls