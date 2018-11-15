---
ID: 925
post_title: Announcing TypeScript 2.2 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-2-rc/
published: true
post_date: 2017-02-02 18:59:11
---
TypeScript 2.2 is just around the corner, and today we're announcing its release candidate! If you're first hearing about it, TypeScript is a language that just takes JavaScript and adds optional static types. Being built on JavaScript means that you don't have to learn much more beyond what you know from JavaScript, and all your existing code continues to work with TypeScript. Meanwhile, the optional types that TypeScript adds can help you catch pesky bugs and work more efficiently by enabling great tooling support. To try out the RC today, you can use [NuGet][1], or using npm just run <div class="highlight highlight-source-shell">
  <pre>npm install -g typescript@rc</pre>
</div> You can also get the TypeScript release candidate 

[for Visual Studio 2015][2] (if you have [Update 3][3]). Other built-in editor support will be coming with our proper 2.2 release, but you can look at guides on how to enable newer versions of TypeScript in [Visual Studio Code][4] and [Sublime Text 3][5]. To clue you in on what's new, here's a few noteworthy features to get an idea about what to keep an eye out for in this release candidate. 
# The <code style="font-size: 29px">object</code> type There are times where an API allows you to pass in any sort of value 

*except* for a primitive. For example, consider [`Object.create`][6], which throws an exception unless you pass in an object or `null` for its first argument. <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">// All of these throw errors at runtime!</span>
<span style="color: #267F99">Object</span>.<span class="pl-en">create</span>(<span class="pl-c1">undefined</span>);
<span style="color: #267F99">Object</span>.<span class="pl-en">create</span>(<span style="color: #09885A">1000</span>);
<span style="color: #267F99">Object</span>.<span class="pl-en">create</span>(<span class="pl-s"><span class="pl-pds">"</span>hello world<span class="pl-pds">"</span></span>);</pre>
</div> If we try to come up with the type of the first parameter, a naive approach might be 

`Object | null`. Unfortunately, this doesn't quite work. Because of the way that [structural types][7] work in TypeScript, `number`, `string`, and `boolean` are all assignable to `Object`. To address this, we've created the new `object` type (and notice all of those lowercase letters!). A more obvious name might be the "non-primitive" type. The `object` type is "empty" - it has no properties just like the `{}` type. That means that almost everything is assignable to `object` *except* for primitives. In other words, `number`, `boolean`, `string`, `symbol`, `null`, and `undefined` won't be compatible. But that means that we can now correctly type `Object.create`'s first parameter as `object | null`! We anticipate that the `object` primitive type will help catch a large class of bugs, and more accurately model real world code. We owe a big thanks to [Herrington Darkholme][8] for lending a hand and implementing this feature! 
# Improved support for mixins and composable classes The mixin pattern is fairly common in the JavaScript ecosystem, and in TypeScript 2.2 we've made some adjustments in the language to supporting it even better. To get this done, we removed some restrictions on classes in TypeScript 2.2, like being able to extend from a value that constructs an intersection type. It also adjusts some functionality in the way that signatures on intersection types get combined. The result is that you can write a function that 

1.  takes a constructor
2.  declares a class that extends that constructor
3.  adds members to that new class
4.  and returns the class itself. For example, we can write the 

`Timestamped` function which takes a class, and extends it by adding a `timestamp` member. <div class="highlight highlight-source-ts">
  <pre><span style="color: #148A14">/** Any type that can construct *something*. */</span>
<span style="color: #0000ff">export</span> <span style="color: #0000ff">type</span> <span style="color: #267F99">Constructable</span> = <span style="color: #0000ff">new</span> (...<span class="pl-v">args</span>: <span style="color: #0000ff">any</span>[]) =&gt; <span style="color: #0000ff">{}</span>;

<span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> <span style="color: #267F99">Timestamped</span>&lt;<span style="color: #267F99">BC</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Constructable</span>&gt;(<span class="pl-v">Base</span>: <span style="color: #267F99">BC</span>) {
    <span style="color: #0000ff">return</span> <span style="color: #0000ff">class</span> <span style="color: #0000ff">extends</span> <span class="pl-e">Base</span> {
        timestamp = <span style="color: #0000ff">new</span> <span style="color: #267F99">Date</span>();
    };
}</pre>
</div> Now we can take any class and pass it through 

`Timestamped` to quickly compose a new type. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">Point</span> {
    x: <span style="color: #0000ff">number</span>;
    y: <span style="color: #0000ff">number</span>;
    <span style="color: #0000ff">constructor</span>(<span class="pl-v">x</span>: <span style="color: #0000ff">number</span>, <span class="pl-v">y</span>: <span style="color: #0000ff">number</span>) {
        <span style="color: #0000ff">this</span>.<span class="pl-c1">x</span> = <span class="pl-smi">x</span>;
        <span style="color: #0000ff">this</span>.<span class="pl-c1">y</span> = <span class="pl-smi">y</span>;
    }
}

<span style="color: #0000ff">const</span> <span style="color: #267F99">TimestampedPoint</span> = <span style="color: #267F99">Timestamped</span>(<span style="color: #267F99">Point</span>);

<span style="color: #0000ff">const</span> p = <span style="color: #0000ff">new</span> <span style="color: #267F99">TimestampedPoint</span>(<span style="color: #09885A">10</span>, <span style="color: #09885A">10</span>);
<span class="pl-smi">p</span>.<span class="pl-c1">x</span> + <span class="pl-smi">p</span>.<span class="pl-c1">y</span>;
<span class="pl-smi">p</span>.<span class="pl-smi">timestamp</span>.<span class="pl-c1">getMilliseconds</span>();</pre>
</div> Similarly, we could write a 

`Tagged` function which adds a `tag` member. These functions actually make it very easy to compose extensions. Making a special 3D point that's tagged and timestamped is pretty clean. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">class</span> <span style="color: #267F99">SpecialPoint</span> <span style="color: #0000ff">extends</span> <span style="color: #267F99">Tagged</span>(<span style="color: #267F99">Timestamped</span>(<span class="pl-c1">Point</span>)) {
    z: <span class="pl-c1">number</span>;
    <span style="color: #0000ff">constructor</span>(<span class="pl-v">x</span>: <span class="pl-c1">number</span>, <span class="pl-v">y</span>: <span class="pl-c1">number</span>, <span class="pl-v">z</span>: <span class="pl-c1">number</span>) {
        <span style="color: #0000ff">super</span>(<span class="pl-smi">x</span>, <span class="pl-smi">y</span>);
        <span style="color: #0000ff">this</span>.<span class="pl-c1">z</span> = <span class="pl-smi">z</span>;
    }
}</pre>
</div>

# A new JSX emit mode: <code style="font-size: 29px">react-native</code> In TypeScript 2.1 and earlier, the 

`--jsx` flag could take on two values: 
*   `preserve` which leaves JSX syntax alone and generates `.jsx` files.
*   `react` which transforms JSX into calls to `React.createElement` and generates `.js` files. TypeScript 2.2 has a new JSX emit mode called 

`react-native` which sits somewhere in the middle. Under this scheme, JSX syntax is left alone, but generates `.js` files. This new mode accomodates React Native's loader which expects all input files to be `.js` files. It also satisfies cases where you want to just leave your JSX syntax alone but get `.js` files out from TypeScript. 
# What's next for 2.2? While we can't list everything that we've worked on here on this post, you can see what else we have in store 

[here on the TypeScript Roadmap][9]. We're counting on your feedback to make TypeScript 2.2 a solid release! Please feel free to leave us feedback here, or [file an issue on GitHub][10] if you run into anything you may think is a bug.

 [1]: https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/2.2.0
 [2]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS-2.2-dev14update3-20170201.4/TypeScript_Dev14Full.exe
 [3]: https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs
 [4]: https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions
 [5]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [6]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create
 [7]: https://en.wikipedia.org/wiki/Structural_type_system
 [8]: https://github.com/HerringtonDarkholme/
 [9]: https://github.com/Microsoft/TypeScript/wiki/Roadmap
 [10]: https://github.com/Microsoft/TypeScript/issues/new