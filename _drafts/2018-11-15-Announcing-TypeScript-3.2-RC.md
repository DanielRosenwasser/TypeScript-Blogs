---
ID: 1946
post_title: Announcing TypeScript 3.2 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/?p=1946
published: false
---
Today we're announcing TypeScript 3.2 RC, the release candidate of our next version of TypeScript.

To get started using the RC, you can get it [through NuGet][1], or use npm with the following command:

<pre><code class="sh">npm install -g typescript@rc
</code></pre>

> As a note for NuGet users, we have some changes in TypeScript 3.2. First, TypeScript 3.2 and future releases will only be shipping an MSBuild package, not a standalone compiler package. Second, while our NuGet packages previously shipped with the Chakra JavaScript engine to run the compiler, the MSBuild package now depends on a globally invokable version of Node to be present. While machines with newer versions of Visual Studio will not be impacted, testing/CI machines and users with Visual Studio 2015 will need to install Node and will likely see a message like the following:
> 
>     The build task could not find node.exe which is required to run the TypeScript compiler. Please install Node and ensure that the system path contains its location.
>     

You can also get editor support for TypeScript 3.2 by:

*   [Downloading for Visual Studio 2017][2] (for version 15.2 or later)
*   [Downloading for Visual Studio 2015][2] (with [Update 3][3])
*   Following directions for [Visual Studio Code][4] and [Sublime Text][5].

Let's look at some of what's coming in TypeScript 3.2!

## `strictBindCallApply`

As you might've guessed from the title of this section, TypeScript 3.2 introduces stricter checking for `bind`, `call`, and `apply`. But what does that mean?

Well in JavaScript, `bind`, `call`, and `apply` are methods on functions that allow us to do things like partially apply arguments, call functions with a different value for `this`, or call functions with an array for their arguments.

Unfortunately, in its earlier days, TypeScript lacked the power to model these functions, and `bind`, `call`, and `apply` were all typed to take any number of arguments, and returned `any`. Additionally, ES2015's arrow functions and rest/spread arguments gave us a new syntax that made it easier to express what some of these methods do - and in a more efficient way as well.

Still, demand to model these patterns in a type-safe way led us to revisit this problem recently. We realized that two features opened up the right abstractions to accurately type `bind`, `call`, and `apply` without any hard-coding:

1.  [`this` parameter types][6] from TypeScript 2.0
2.  [Modeling parameter lists with tuple types][7] from TypeScript 3.0

Combined, the two of of them can ensure our uses of `bind`, `call`, and `apply` are more strictly checked when we use a new flag called `strictBindCallApply`. When using this new flag, the methods on callable objects are described by a new global type called `CallableFunction` which declares stricter versions of the signatures for `bind`, `call`, and `apply`. Similarly, any methods on constructable (but not callable) objects are described by a new global type called `NewableFunction`.

As an example, we can look at how `Function.prototype.apply` acts under this behavior:

<pre><code class="ts">function foo(a: number, b: string): string {
    return a + b;
}

let a = foo.apply(undefined, [10]);              // error: too few argumnts
let b = foo.apply(undefined, [10, 20]);          // error: 2nd argument is a number
let c = foo.apply(undefined, [10, "hello", 30]); // error: too many arguments
let d = foo.apply(undefined, [10, "hello"]);     // okay! returns a string
</code></pre>

Needless to say, whether you do any sophisticated metaprogramming, or you use simple patterns like binding methods in your class instances (`this.foo = this.foo.bind(this)`), this feature can help catch a lot of bugs. For more details, you can check out [the original pull request here][8].

## Object spread on generic types

JavaScript supports a handy way of copying existing properties from an existing object into a new one called "spreads". To spread an existing object into a new object, you define an element with three consecutive periods (`...`) like so:

<pre><code class="ts">let person = { name: "Daniel", location: "New York City" };

// My secret revealed, I have two clones!
let shallowCopyOfPerson = { ...person };
let shallowCopyOfPersonWithDifferentLocation = { ...person, location: "Seattle" };
</code></pre>

TypeScript does a pretty good job here when it has enough information about the type. The type system closely tries to model the behavior of spreads and overwrites new properties, tries to ignore methods, etc. But unfortunately up until now it wouldn't work with generics at all.

<pre><code class="ts">function merge&lt;T, U&gt;(x: T, y: U) {
    // Previously an error!
    return { ...x, ...y };
}
</code></pre>

This was an error because we had no way to express the return type of `merge`. There was no syntax (nor semantics) that could express two unknown types being spread into a new one.

We could have come up with a new concept in the type system called an "object spread type", and in fact [we had a proposal for exactly that][9]. Essentially this would be a new type operator that looks like `{ ...T, ...U }` to reflect the syntax of an object spread.  
When both `T` and `U` are known, that type would flatten down to some new object type. <!-- While that sounds great, the reality is that this would introduce new rules when relating types, and bring in a lot of complexity. We explored whether we could build on other existing types in the type system like conditional types and mapped object types. While we and our community were able to create a pretty accurate \`Spread\` type alias to model spread expressions, it was a bit... well, baroque, and relating any two spread --> 

However this is pretty complex and requires adding new rules to type relationships and inference. While we explored several different avenues, we recently arrived at two conclusions:

1.  For most uses of spreads in JavaScript, users were fine modeling the behavior with intersection types (i.e. `Foo & Bar`).
2.  `Object.assign` - a function that exhibits most of the behavior of spreading objects - is already modeled using intersection types, and we've seen very little negative feedback around that.

Given that intersections model the common cases, and that they're relatively easy to reason about for both users and the type system, TypeScript 3.2 now permits object spreads on generics and models them using intersections:

<pre><code class="ts">// Returns 'T & U'
function merge&lt;T, U&gt;(x: T, y: U) {
    return { ...x, ...y };
}

// Returns '{ name: string, age: number, greeting: string } & T'
function foo&lt;T&gt;(obj: T) {
    let person = {
        name: "Daniel",
        age: 26
    };

    return { ...person, greeting: "hello", ...obj };
}
</code></pre>

## Object rest on generic types

Object rest patterns are sort of the dual of object spreads. Instead of creating a new object with some extra/overridden properties, it creates a new object that *lacks* some specified properties.

<pre><code class="ts">let { x, y, z, ...rest } = obj;
</code></pre>

In the above, the most intuitive way to look at this code is that `rest` copies over all the properties from `obj` *apart* from `x`, `y`, and `z`. For the same reason as above, because we didn't have a good way to describe the type of `rest` when `obj` is generic, we didn't support this for a while.

Here we also considered a new rest operator, but we saw we already had the facilities for describing the above: our `Pick` and `Exclude` helper types in `lib.d.ts` To reiterate, `rest` basically picks off all of the properties on `obj` except for `x`, `y`, and `z`. So for the following:

<pre><code class="ts">interface XYZ { x: any; y: any; z: any; }

function dropXYZ&lt;T extends XYZ&gt;(obj: T) {
    let { x, y, z, ...rest } = obj;
    return rest;
}
</code></pre>

If we want to consider the properties of `T` (i.e. `keyof T`) except for `x`, `y`, and `z`, we can write `Exclude<keyof T, "x" | "y" | "z">`. We then want to pick those properties back off of the original type `T`, which gives us

    Pick<T, Exclude<keyof T, "x" | "y" | "z">>`.
    

While it's not the most beautiful type (hey, I'm no George Clooney myself), we can wrap it in a helper type like `DropXYZ`:

<pre><code class="ts">interface XYZ { x: any; y: any; z: any; }

type DropXYZ&lt;T&gt; = Pick&lt;T, Exclude&lt;keyof T, keyof XYZ&gt;&gt;;

function dropXYZ&lt;T extends XYZ&gt;(obj: T): DropXYZ&lt;T&gt; {
    let { x, y, z, ...rest } = obj;
    return rest;
}
</code></pre>

## BigInt

BigInts are part of an upcoming proposal in ECMAScript that allow us to model theoretically arbitrarily large integers. TypeScript 3.2 brings type-checking for BigInts, as well as support for emitting BigInt literals when targeting `esnext`.

BigInt support in TypeScript introduces a new primitive type called the `bigint` (all lowercase). You can get a `bigint` by calling the `BigInt()` function or by writing out a BigInt literal by adding an `n` to the end of any integer numeric literal:

<pre><code class="ts">let foo: bigint = BigInt(100); // the BigInt function
let bar: bigint = 100n;        // a BigInt literal

// *Slaps roof of fibonacci function*
// This bad boy returns ints that are *so* big!
function fibonacci(n: bigint) {
    let result = 1n;
    for (let last = 0n, i = 0n; i &lt; n; i++) {
        const current = result;
        result += last;
        last = current;
    }
    return result;
}

fibonacci(10000n)
</code></pre>

While you might imagine close interaction between `number` and `bigint`, the two are separate domains.

<pre><code class="ts">declare let foo: number;
declare let bar: bigint;

foo = bar; // error: Type 'bigint' is not assignable to type 'number'.
bar = foo; // error: Type 'number' is not assignable to type 'bigint'.
</code></pre>

As specified in ECMAScript, mixing `number`s and `bigint`s in arithmetic operations is an error. You'll have to explicitly convert values to `BigInt`s.

<pre><code class="ts">console.log(3.141592 * 10000n);     // error
console.log(3145 * 10n);            // error
console.log(BigInt(3145) * 10n);    // okay!
</code></pre>

Also important to note is that `bigint`s produce a new string when using the `typeof` operator: the string `"bigint"`. Thus, TypeScript correctly narrows using `typeof` as you'd expect.

<pre><code class="ts">function whatKindOfNumberIsIt(x: number | bigint) {
    if (typeof x === "bigint") {
        console.log("'x' is a bigint!");
    }
    else {
        console.log("'x' is a floating-point number");
    }
}
</code></pre>

We'd like to extend a huge thanks to [Caleb Sander][10] for all the work on this feature. We're grateful for the contribution, and we're sure our users are too!

### Caveats

As we mentioned, BigInt support is only available for the `esnext` target. It may not be obvious, but because BigInts have different behavior for mathematical operators like `+`, `-`, `*`, etc., providing functionality for older targets where the feature doesn't exist (like `es2017` and below) would involve rewriting each of these operations. TypeScript would need to dispatch to the correct behavior depending on the type, and so every addition, string concatenation, multiplication, etc. would involve a function call.

For that reason, we have no immediate plans to provide downleveling support. On the bright side, Node 11 and newer versions of Chrome already support this feature, so you'll be able to use BigInts there when targeting `esnext`.

Certain targets may include a polyfill or BigInt-like runtime object. For those purposes you may want to add `esnext.bigint` to the `lib` setting in your compiler options.

## Breaking changes

### JSX resolution changes

Our logic for resolving JSX invocations has been unified with our logic for resolving function calls. While this has simplified the compiler codebase and improved some use-cases, there may be some differences which we may need to reconcile. These changes are likely unintentional so they are not breaking changes *per se*, but upgraders should note take note of any issues they encounter and [report them][11].

### `lib.d.ts` changes

#### More specific types

Certain parameters no longer accept `null`, or now accept more specific types as per the corresponding specifications that describe the DOM.

#### More platform-specific deprecations

Certain properties that are WebKit-specific have been deprecated. They are likely to be removed in a new version.

#### `wheelDelta` and friends have been removed.

`wheelDeltaX`, `wheelDelta`, and `wheelDeltaZ` have all been removed as they is a deprecated properties on `WheelEvent`s.

As a solution, you can use `deltaX`, `deltaY`, and `deltaZ` instead. If older runtimes are a concern, you can include a file called `legacy.d.ts` in your project and write the following in it:

<pre><code class="ts">// legacy.d.ts

interface WheelEvent {
     readonly wheelDelta: number;
     readonly wheelDeltaX: number;
     readonly wheelDeltaZ: number;
}
</code></pre>

## What's next

We hope you give TypeScript 3.2 RC a try and give us some feedback on your experience. In the meanwhile, we'll be listening to the community and polishing the 3.2 release over the next 2 weeks. Happy hacking!

- Daniel Rosenwasser and the TypeScript team

 [1]: https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild
 [2]: TODO
 [3]: https://www.visualstudio.com/en-us/news/releasenotes/vs2015-update3-vs
 [4]: https://code.visualstudio.com/Docs/languages/typescript#_using-newer-typescript-versions
 [5]: https://github.com/Microsoft/TypeScript-Sublime-Plugin/#note-using-different-versions-of-typescript
 [6]: https://github.com/Microsoft/TypeScript/pull/6739
 [7]: https://github.com/Microsoft/TypeScript/pull/24897
 [8]: https://github.com/Microsoft/TypeScript/pull/27028
 [9]: https://github.com/Microsoft/TypeScript/issues/10727
 [10]: https://github.com/calebsander
 [11]: https://github.com/Microsoft/TypeScript/issues/new/choose