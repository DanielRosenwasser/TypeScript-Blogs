---
ID: 1678
post_title: Announcing TypeScript 2.0 RC
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-2-0-rc-2/
published: true
post_date: 2016-08-30 16:12:07
---
TypeScript 2.0 is *almost* out, and today we're happy to show just how close we are with our release candidate! If you haven't used TypeScript yet, check out [the intro tutorial on our website][1] to get started. To start using the RC now, you can download [TypeScript 2.0 RC for Visual Studio 2015][2] (which requires [VS Update 3][3]), grab it through NuGet, or use npm: 
    npm install -g typescript@rc
     Visual Studio Code users can 

[follow the steps here][4] to use the RC. This RC gives an idea of what the full version of 2.0 will look like, and we're looking for broader feedback to stabilize and make 2.0 a solid release. Overall, the RC should be stable enough for general use, and we don't expect any major new features to be added past this point. On the other hand, lots of stuff has been added since 2.0 beta was released, so here's a few features that you might not have heard about since then. 
## <a href="https://gist.github.com/DanielRosenwasser/1d8230ed6376de515942e82a45888a47#tagged-unions" id="user-content-tagged-unions" class="anchor"></a>Tagged Unions Tagged unions are an exciting new feature that brings functionality from languages like F#, Swift, Rust, and others to JavaScript, while embracing the way that people write JavaScript today. This feature is also called 

*discriminated unions*, *disjoint unions*, or *algebraic data types*. But what's in the feature is much more interesting than what's in the name. Let's say you have two types: `Circle` and `Square`. You then have a union type of the two named `Shape`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Circle</span> {
    <span style="color: #000000">kind</span>: <span style="color: #a31515">"circle"</span>;
    <span style="color: #000000">radius</span>: <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Square</span> {
    <span style="color: #000000">kind</span>: <span style="color: #a31515">"square"</span>;
    <span style="color: #000000">sideLength</span>: <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">Shape</span> <span style="color: #0000ff">= </span><span style="color: #267F99">Circle</span> <span style="color: #0000ff">|</span> <span style="color: #267F99">Square</span>;</pre>
</div> Notice that both 

`Circle` and `Square` have a field named `kind` which has a string literal type. That means the `kind` field on a `Circle` will always contain the string `"circle"`. Each type has a common field, but has been *tagged* with a unique value. In TypeScript 1.8, writing a function to get the area of a `Shape` required a type assertions for each type in `Shape`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> <span class="pl-en">getArea</span>(<span style="color: #000000">shape</span>: <span style="color: #267F99">Shape</span>) {
    <span style="color: #0000ff">switch</span> (shape.kind) {
        <span style="color: #0000ff">case</span> <span style="color: #a31515">"circle"</span>:
            <span style="color: #148A14">// Convert from 'Shape' to 'Circle'</span>
            <span style="color: #0000ff">let</span> <span style="color: #000000">c</span> = shape <span style="color: #0000ff">as</span> <span style="color: #267F99">Circle</span>;
            <span style="color: #0000ff">return</span> Math.PI <span style="color: #0000ff">*</span> c.radius <span style="color: #0000ff">**</span> <span style="color: #09885A">2</span>;

        <span style="color: #0000ff">case</span> <span style="color: #a31515">"square"</span>:
            <span style="color: #148A14">// Convert from 'Shape' to 'Square'</span>
            <span style="color: #0000ff">let</span> <span style="color: #000000">sq</span> = shape <span style="color: #0000ff">as</span> <span style="color: #267F99">Square</span>;
            <span style="color: #0000ff">return</span> sq.sideLength <span style="color: #0000ff">**</span> <span style="color: #09885A">2</span>;
    }
}</pre>
</div> Notice we made up intermediate variables for 

`shape` just to keep this a little cleaner. In 2.0, that isn't necessary. The language *understands* how to discriminate based on the `kind` field, so you can cut down on the boilerplate. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">function</span> <span class="pl-en">getArea</span>(<span style="color: #000000">shape</span>: <span style="color: #267F99">Shape</span>) {
    <span style="color: #0000ff">switch</span> (shape.kind) {
        <span style="color: #0000ff">case</span> <span style="color: #a31515">"circle"</span>:
            <span style="color: #148A14">// 'shape' is a 'Circle' here.</span>
            <span style="color: #0000ff">return</span> Math.PI <span style="color: #0000ff">*</span> shape.radius <span style="color: #0000ff">**</span> <span style="color: #09885A">2</span>;

        <span style="color: #0000ff">case</span> <span style="color: #a31515">"square"</span>:
            <span style="color: #148A14">// 'shape' is a 'Square' here.</span>
            <span style="color: #0000ff">return</span> shape.sideLength <span style="color: #0000ff">**</span> <span style="color: #09885A">2</span>;
    }
}</pre>
</div> This is totally valid, and TypeScript can use control flow analysis to figure out the type at each branch. In fact, you can use 

`--noImplicitReturns` and the upcoming `--strictNullChecks` feature to make sure these checks are exhaustive. Tagged unions make it way easier to get type safety using JavaScript patterns you'd write today. For example, libraries like Redux will often use this pattern when processing actions. 
## <a href="https://gist.github.com/DanielRosenwasser/1d8230ed6376de515942e82a45888a47#more-literal-types" id="user-content-more-literal-types" class="anchor"></a>More Literal Types String literal types are a feature we showed off back in 1.8, and were tremendously useful. Like you saw above, we were able to leverage them to bring you tagged unions. We wanted to give some more love to types other than just 

`string`. In 2.0, each unique `boolean`, `number`, and enum member will have its own type! <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">type</span> <span style="color: #267F99">Digit</span> <span style="color: #0000ff">= </span><span style="color: #09885A">0</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">1</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">2</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">3</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">4</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">5</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">6</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">7</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">8</span> <span style="color: #0000ff">|</span> <span style="color: #09885A">9</span>;
<span style="color: #0000ff">let</span> <span style="color: #000000">nums</span>: <span style="color: #267F99">Digit</span>[] <span style="color: #0000ff">=</span> [<span style="color: #09885A">1</span>, <span style="color: #09885A">2</span>, <span style="color: #09885A">4</span>, <span style="color: #09885A">8</span>];

<span style="color: #148A14">// Error! '16' isn't a 'Digit'!</span>
nums.push(<span style="color: #09885A">16</span>);</pre>
</div> Using tagged unions, we can express some things a little more naturally. 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">interface</span> <span style="color: #267F99">Success</span>&lt;<span style="color: #267F99">T</span>&gt; {
    <span style="color: #000000">success</span>: <span style="color: #0000ff">true</span>;
    <span style="color: #000000">value</span>: <span style="color: #267F99">T</span>;
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">Failure</span> {
    <span style="color: #000000">success</span>: <span style="color: #0000ff">false</span>;
    <span style="color: #000000">reason</span>: <span style="color: #0000ff">string</span>;
}

<span style="color: #0000ff">type</span> <span style="color: #267F99">Result</span>&lt;<span style="color: #267F99">T</span>&gt; <span style="color: #0000ff">= </span><span style="color: #267F99">Success</span>&lt;<span style="color: #267F99">T</span>&gt; <span style="color: #0000ff">|</span> <span style="color: #267F99">Failure</span>;</pre>
</div> The 

`Result<T>` type here represents something that can potentially fail. If it succeeds, it has a `value`, and if it fails, it contains a reason for failure. The `value` field can only be used when `success` is `true`. <div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">declare</span> <span style="color: #0000ff">function</span> <span class="pl-en">tryGetNumUsers</span>(): <span style="color: #267F99">Result</span>&lt;<span style="color: #0000ff">number</span>&gt;;

<span style="color: #0000ff">let</span> <span style="color: #000000">result</span> = tryGetNumUsers();
<span style="color: #0000ff">if</span> (result.success <span style="color: #0000ff">===</span> <span style="color: #0000ff">true</span>) {
    <span style="color: #148A14">// 'result' has type 'Success&lt;number&gt;'</span>
    console.log(<span style="color: #a31515">`Server reported </span><span style="color: #0000ff">${</span>result.value<span style="color: #0000ff">}</span><span style="color: #a31515"> users`</span>);
}
<span style="color: #0000ff">else</span> {
    <span style="color: #148A14">// 'result' has type 'Failure'</span>
    console.error(<span style="color: #a31515">"Error fetching number of users!"</span>, result.reason);
}</pre>
</div> You may've noticed that enum members get their own type too! 

<div class="highlight highlight-source-ts">
  <pre><span style="color: #0000ff">enum</span> <span style="color: #267F99">ActionType</span> { Append, Erase }

<span style="color: #0000ff">interface</span> <span style="color: #267F99">AppendAction</span> { 
    <span style="color: #000000">type</span>: ActionType.Append;
    <span style="color: #000000">text</span>: <span style="color: #267F99">string</span>;
}

<span style="color: #0000ff">interface</span> <span style="color: #267F99">EraseAction</span> {
    <span style="color: #000000">type</span>: ActionType.Erase;
    <span style="color: #000000">numChars</span>: <span style="color: #0000ff">number</span>;
}

<span style="color: #0000ff">function</span> <span class="pl-en">updateText</span>(<span style="color: #000000">currentText</span>: <span style="color: #267F99">string</span>, <span style="color: #000000">action</span>: <span style="color: #267F99">AppendAction</span> <span style="color: #0000ff">|</span> <span style="color: #267F99">EraseAction</span>) {
    <span style="color: #0000ff">if</span> (action.type <span style="color: #0000ff">===</span> ActionType.Append) {
        <span style="color: #148A14">// 'action' has type 'AppendAction'</span>
        <span style="color: #0000ff">return</span> currentText <span style="color: #0000ff">+</span> action.text;
    }
    <span style="color: #0000ff">else</span> {
        <span style="color: #148A14">// 'action' has type 'EraseAction'</span>
        <span style="color: #0000ff">return</span> currentText.slice(<span style="color: #09885A">0</span>, <span style="color: #0000ff">-</span>action.numChars);
    }
}</pre>
</div>

## <a href="https://gist.github.com/DanielRosenwasser/1d8230ed6376de515942e82a45888a47#globs-includes-and-excludes" id="user-content-globs-includes-and-excludes" class="anchor"></a>Globs, Includes, and Excludes When we first introduced the 

`tsconfig.json` file, you told us that manually listing files was a pain. TypeScript 1.6 introduced the `exclude` field to alleviate this; however, the consensus has been that this was just not enough. It's a pain to write out every single file path, and you can run into issues when you forget to exclude new files. TypeScript 2.0 finally adds support for globs. Globs allow us to write out wildcards for paths, making them as granular as you need without being tedious to write. You can use them in the new `include` field as well as the existing `exclude` field. As an example, let's take a look at this `tsconfig.json` that compiles all our code except for our tests: <div class="highlight highlight-source-json">
  <pre>{
    <span style="color: #a31515"><span style="color: #a31515">"</span>include<span style="color: #a31515">"</span></span>: [
        <span style="color: #a31515"><span style="color: #a31515">"</span>./src/**/*.ts<span style="color: #a31515">"</span></span>
    ],
    <span style="color: #a31515"><span style="color: #a31515">"</span>exclude<span style="color: #a31515">"</span></span>: [
        <span style="color: #a31515"><span style="color: #a31515">"</span>./src/tests/**<span style="color: #a31515">"</span></span>
    ]
}</pre>
</div> TypeScript's globs support the following wildcards: 

*   `*` for 0 or more non-separator characters (such as `/` or `\`).
*   `?` to match exactly one non-separator character.
*   `**/` for any number of subdirectories

## <a href="https://gist.github.com/DanielRosenwasser/1d8230ed6376de515942e82a45888a47#next-steps" id="user-content-next-steps" class="anchor"></a>Next Steps Like we mentioned, TypeScript 2.0 is not far off, but using the RC along with 2.0's new features will play a huge part in that release for the broader community. Feel free to reach out to us about any issues [through GitHub][5]. We would love to hear any and all feedback as you try things out. Enjoy!

 [1]: https://www.typescriptlang.org/docs/tutorial.html
 [2]: http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS2.0.2-TS-release20-nightly-20160828.1/TypeScript_Dev14Full.exe
 [3]: https://msdn.microsoft.com/en-us/library/mt752379.aspx
 [4]: https://code.visualstudio.com/docs/languages/typescript#_using-newer-typescript-versions
 [5]: https://github.com/Microsoft/TypeScript