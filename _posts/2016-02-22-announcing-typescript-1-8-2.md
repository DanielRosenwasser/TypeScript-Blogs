---
ID: 582
post_title: Announcing TypeScript 1.8
author: Bowden Kelly
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-8-2/
published: true
post_date: 2016-02-22 21:31:29
---
Today we are thrilled to announce the release of TypeScript 1.8. In the <a href="https://blogs.msdn.microsoft.com/typescript/2016/01/28/announcing-typescript-1-8-beta/" target="_blank">TypeScript 1.8 Beta release blog post</a>, we highlighted some of the key features that are now available to TypeScript users - how [JavaScript in TypeScript compilation][1] provides a great way forward to start converting your JavaScript projects to TypeScript, [improvements][2] to [JSX/TSX support][3], and our move to [ChakraCore][4]. This post supplements the above and details how TypeScript 1.8 provides full support for module augmentation, delivers a stronger type system with string literal types, and catches even more common bugs with smarter control flow analysis. TypeScript 1.8 is available for [Visual Studio 2015][5], [Visual Studio 2013][6], NuGet ([Compiler][7] and [MSBuild task][8]), [npm][9], and straight from the [source][10]. 
## Module Augmentation TypeScript 1.8 allows users to design more modular libraries via module augmentation. This allows library authors to distribute their libraries in a piecemeal fashion. Previously, TypeScript made the assumption that modules wouldn't change. With module augmentation, users have the ability to extend existing modules such that consumers can specify if they want to import the whole module or just a subset. This can be accomplished by simply adding an ambient module declaration and extending any existing types. 

<pre><span style="color: #008000">// scale.ts</span>
<span style="color: #0000ff">export class</span> Scale {
    weightOnEarth(mass) {}
}
<span style="color: #008000">// advancedScale.ts</span>
<span style="color: #0000ff">import</span> { Scale } <span style="color: #0000ff">from</span> <span style="color: #a31515">"./scale"</span> ;</pre>

<pre><span style="color: #008000">// create augmentation for Scale</span>
<span style="color: #0000ff">declare module</span> <span style="color: #a31515">"./scale"</span> {
    <span style="color: #008000">// Augment Core class via interface merging</span>
    <span style="color: #0000ff">interface</span> Scale {
        weightOnMoon(mass); <span style="color: #008000">// not everyone needs moon weight</span>
    }
}
Scale.prototype.weightOnMoon = <span style="color: #008000">/* insert implementation */</span>;

<span style="color: #008000">// consumer.ts</span>
<span style="color: #0000ff">import</span> { Scale } <span style="color: #0000ff">from</span> <span style="color: #a31515">"./scale"</span>;
<span style="color: #0000ff">import</span> <span style="color: #a31515">"./advancedScale"</span>;

<span style="color: #0000ff">let</span> scale: Scale;
scale.weightOnMoon(10);  <span style="color: #008000">// ok</span></pre>

## String Literal Types It is very common for JavaScript libraries to consume a string as a configuration parameter, and usually in such cases you want to restrict the possible strings to a certain set. Take the following example of a UI element that can specify an easing method: 

<pre><span style="color: #0000ff">declare class</span> UIElement {
    animate(options: AnimationOptions): <span style="color: #0000ff">void</span>;
}

<span style="color: #0000ff">interface</span> AnimationOptions {
    deltaX: <span style="color: #0000ff">number</span>;
    deltaY: <span style="color: #0000ff">number</span>;
    easing: <span style="color: #0000ff">string</span>;  <span style="color: #008000">// Can be "ease-in", "ease-out", "ease-in-out"</span>
}
<span style="color: #0000ff">new</span> UIElement().animate({ deltaX: 100, deltaY: 100, easing: <span style="color: #a31515">"out"</span> }); <span style="color: #008000">// No error</span></pre> With regular strings, there is no protection from this class of error. However, starting with TypeScript 1.8, strings in a type position will become 

*string literal types*. Only exact string matches are assignable to string literal types, and like any other type, they can be used in union types as well. So if we rewrite the `AnimationOptions` interface with string literal types, the API users now get type protection: 
<pre><span style="color: #0000ff">interface</span> AnimationOptions {
    deltaX: <span style="color: #0000ff">number</span>;
    deltaY: <span style="color: #0000ff">number</span>;
    easing: <span style="color: #a31515">"ease-in"</span> | <span style="color: #a31515">"ease-out"</span> | <span style="color: #a31515">"ease-in-out"</span>;
}

<span style="color: #008000">// Error: Type '"out"' is not assignable to type '"ease-in" | "ease-out" | "ease-in-out"'</span>
<span style="color: #0000ff">new</span> UIElement().animate({ deltaX: 100, deltaY: 100, easing: <span style="color: #a31515">"out"</span> });</pre>

## Smarter Control Flow Analysis It is common for complex logic that results in many branching code paths to produce hard to find bugs. TypeScript 1.8 delivers better control flow analysis to help you catch these at compile time. 

### Unreachable Code JavaScript's automatic semicolon insertion is fairly useful, but it can also lead to unwanted results at times. The example below, which actually returns 

`undefined`, highlights a common bug that is now detected by the TypeScript compiler. 
<pre><span style="color: #0000ff">function</span> importantData() {
    <span style="color: #0000ff">return</span>          <span style="color: #008000">// Automatic semicolon insertion triggered with newline</span>
    {
        x: <span style="color: #a31515">"thing"</span>  <span style="color: #008000">// Error: Unreachable code detected.</span>
    }
}</pre> If for whatever reason you are experiencing too many false positives, 

<a href="http://github.com/microsoft/typescript/issues" target="_blank">please let us know</a>, but you can toggle this feature off with the `--allowUnreachableCode` flag. 
### Implicit Returns In JavaScript, reaching the end of a function with no return statement results in implicitly returning 

`undefined`. Sometimes this is the intended behavior, but often times it can be a red flag for gaps in complex logic. Unlike unreachable code, this check is **off** by default. To get the added safety, just add the `--noImplicitReturns` flag. 
<pre><span style="color: #0000ff">function</span> isPizza(food) {   <span style="color: #008000">// Error: Not all code paths return a value.</span>
    <span style="color: #0000ff">if</span> (food === <span style="color: #a31515">"pizza"</span>) {
        <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
    }
    <span style="color: #0000ff">else</span> <span style="color: #0000ff">if</span> (food === <span style="color: #a31515">"pie"</span>) {
        <span style="color: #0000ff">return</span> <span style="color: #0000ff">true</span>;
    }
    <span style="color: #008000">// implicitly returns `undefined`</span>
}</pre> TypeScript 1.8 also includes control logic for unused labels and case clause fall-through. You can read more about those 

<a href="https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#control-flow-analysis-errors" target="_blank">here</a>. 
## So Much More! If you want the deep dive on all the new features released with 1.8, check out our 

<a href="https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#typescript-18" target="_blank">what's new page</a>. As always, we encourage you to weigh in on the future of TypeScript by browsing our <a href="https://github.com/microsoft/typescript/issues" target="_blank">existing issues</a>, throwing us a <a href="https://github.com/microsoft/typescript/pulls" target="_blank">pull request</a>, or just hanging out with the team on <a href="https://gitter.im/microsoft/typescript" target="_blank">gitter</a>. <div>
</div>

 [1]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#including-js-files-with---allowjs
 [2]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#custom-jsx-factories-using---reactnamespace
 [3]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#stateless-function-components-in-react
 [4]: https://github.com/Microsoft/Chakracore
 [5]: https://www.microsoft.com/en-us/download/details.aspx?id=48593
 [6]: https://www.microsoft.com/en-us/download/details.aspx?id=48739
 [7]: https://www.nuget.org/packages/Microsoft.TypeScript.Compiler/
 [8]: https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/
 [9]: https://www.npmjs.com/package/typescript
 [10]: https://github.com/Microsoft/TypeScript/releases