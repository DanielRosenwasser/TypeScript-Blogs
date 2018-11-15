---
ID: 253
post_title: 'Walkthrough: Interfaces'
author: Ryan Cavanaugh
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/walkthrough-interfaces/
published: true
post_date: 2013-01-24 13:00:00
---
When we talk about a <span class="italic">type</span> in TypeScript, we mean a collection of things that you can do with a variable (or expression). You might be able to read or write a given property, call a function, use the expression as a constructor, or index into the object. Some objects (like <tt><span class="code">Date</span></tt>) in JavaScript can do nearly all of those! In TypeScript, <span class="bold">interfaces</span> are the most flexible way of describing types.

You'll see interfaces used to describe existing JavaScript APIs, create shorthand names for commonly-used types, constrain class implementations, describe array types, and more. While they don't generate any code (and thus have no runtime cost!), they are often the key point of contact between any two pieces of TypeScript code, especially when working with existing JavaScript code or built-in JavaScript objects.

The only job of an interface in TypeScript is to <span class="bold">describe a type</span>. While <tt><span class="code">class</span></tt> and <tt><span class="code">function</span></tt> deal with implementation, <tt><span class="code">interface</span></tt> helps us keep our programs error-free by providing information about the shape of the data we work with. Because the type information is erased from a TypeScript program during compilation, we can freely add type data using interfaces without worrying about the runtime overhead.

While that sounds like a simple, one-purpose task, interfaces role in describing types becomes manifest in a large variety of ways. Let's look at some of them and how they can be used in TypeScript programs.

<a name="Title_57"></a>

# Basics

To define an interface in TypeScript, use the <tt><span class="code">interface</span></tt> keyword:

<pre><span class="keyword">interface</span>&nbsp;<span class="type-name">Greetable</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;greet(message:&nbsp;<span class="keyword">string</span>):&nbsp;<span class="keyword">void</span>;<br />}</pre>

This defines a type, <tt><span class="code">Greetable</span></tt>, that has a member function called <tt><span class="code">greet</span></tt> that takes a string argument. You can use this type in all the usual positions; for example in a parameter type annotation. Here we use that type annotation to get type safety on the <tt><span class="code">g</span></tt> parameter:

<pre><span class="keyword">function</span>&nbsp;helloEnglish(g:&nbsp;Greetable)&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;g.greet('Hello&nbsp;there!');&nbsp;<span class="comment">//&nbsp;OK</span><br />&nbsp;&nbsp;&nbsp;&nbsp;g.greet(42);&nbsp;<span class="comment">//&nbsp;Not&nbsp;OK&nbsp;--&nbsp;42&nbsp;is&nbsp;not&nbsp;a&nbsp;string</span><br />&nbsp;&nbsp;&nbsp;&nbsp;g.greep('Hi');&nbsp;<span class="comment">//&nbsp;Not&nbsp;OK&nbsp;--&nbsp;'greep'&nbsp;is&nbsp;not&nbsp;a&nbsp;member&nbsp;of&nbsp;'Greetable'</span><br />}</pre>

When this code compiles, you won't see any mention of <tt><span class="code">Greetable</span></tt> in the JavaScript code. Interfaces are only a compile-time construct and have no effect on the generated code.

<a name="Title_58"></a>

# Interfaces: TypeScript's Swiss Army Knife

Interfaces get to play a lot of roles in TypeScript code. We'll go into more detail on these after a quick overview.

<a name="Subsection_59"></a>

#### Describing an Object

Many JavaScript functions take a "settings object". For example, jQuery's <tt><span class="code">$.ajax</span></tt> takes an object that can have up to several dozen members that control its behavior, but you're only likely to pass a few of those in any given instance. TypeScript interfaces allow optional properties to help you use these sorts of objects correctly.

<a name="Subsection_60"></a>

#### Describing an Indexable Object

JavaScript freely mixes members (<tt><span class="code">foo.x</span></tt>) with indexers (<tt><span class="code">foo['x']</span></tt>), but most programmers use one or the other as a semantic hint about what kind of access is taking place. TypeScript interfaces can be used to represent what the expected type of an indexing operation is.

<a name="Subsection_61"></a>

#### Ensuring Class Instance Shape

Often, you'll want to make sure that a class you're writing matches some existing surface area. This is how interfaces are used in more traditional OOP languages like C# and Java, and we'll see that TypeScript interfaces behave very similarly when used in this role.

<a name="Subsection_62"></a>

#### Ensuring the Static Shape of a Class or Constructor Object

Interfaces normally describe the shape of an <span class="bold">instance</span> of a class, but we can also use them to describe the static shape of the class (including its constructor function). We'll cover this in a later post.

<a name="Title_63"></a>

# Describing an Object

You can also use interfaces to define the shape of objects that will typically be expressed in an object literal. Here's an example:

<a name="Subsection_64"></a>

#### Describing Simple Types

<pre><span class="keyword">interface</span>&nbsp;<span class="type-name">ButtonSettings</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;text:&nbsp;<span class="keyword">string</span>;<br />&nbsp;&nbsp;&nbsp;&nbsp;size?:&nbsp;{&nbsp;width:&nbsp;<span class="keyword">number</span>;&nbsp;height:&nbsp;<span class="keyword">number</span>;&nbsp;};<br />&nbsp;&nbsp;&nbsp;&nbsp;color?:&nbsp;<span class="keyword">string</span>;<br />}<br /><br /><span class="keyword">function</span>&nbsp;createButton(settings:&nbsp;<span class="type-name">ButtonSettings</span>)&nbsp;{&nbsp;...&nbsp;}</pre>

Note the use of the <tt><span class="code">?</span></tt> symbol after some of the names. This marks a member as being <span class="italic">optional</span>. This lets callers of <tt><span class="code">createButton</span></tt> supply only the members they care about, while maintaining the constraint that the required parts of the object are present:

<pre>createButton({&nbsp;text:&nbsp;'Submit'&nbsp;});&nbsp;<span class="comment">//&nbsp;OK</span><br />createButton({&nbsp;text:&nbsp;'Submit',&nbsp;size:&nbsp;{&nbsp;width:&nbsp;70,&nbsp;height:&nbsp;30&nbsp;}});&nbsp;<span class="comment">//&nbsp;OK</span><br />createButton({&nbsp;text:&nbsp;'Submit',&nbsp;color:&nbsp;<span class="warning">43</span>);&nbsp;<span class="comment">//&nbsp;Not&nbsp;OK:&nbsp;43&nbsp;isn't&nbsp;a&nbsp;string</span><br />createButton({&nbsp;text:&nbsp;'Submit',&nbsp;size:&nbsp;<span class="warning">{&nbsp;width:&nbsp;70&nbsp;}</span>);&nbsp;<span class="comment">//&nbsp;Not&nbsp;OK:&nbsp;size&nbsp;needs&nbsp;a&nbsp;height&nbsp;as&nbsp;well</span><br />createButton(<span class="warning">{&nbsp;color:&nbsp;'Blue'}</span>);&nbsp;<span class="comment">//&nbsp;Not&nbsp;OK:&nbsp;'text'&nbsp;member&nbsp;is&nbsp;required</span></pre>

You typically won't use optional members when defining interfaces that are going to be implemented by classes.

Here's another example that shows an interesting feature of types in TypeScript:

<pre><span class="keyword">interface</span>&nbsp;<span class="type-name">Point</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;x:&nbsp;<span class="keyword">number</span>;<br />&nbsp;&nbsp;&nbsp;&nbsp;y:&nbsp;<span class="keyword">number</span>;<br />}<br /><br /><span class="keyword">function</span>&nbsp;getQuadrant(pt:&nbsp;<span class="type-name">Point</span>)&nbsp;{&nbsp;...&nbsp;}<br /><br /><span class="keyword">var</span>&nbsp;pt&nbsp;=&nbsp;{&nbsp;x:&nbsp;0,&nbsp;y:&nbsp;-1&nbsp;};<br />getQuadrant(pt);&nbsp;<span class="comment">//&nbsp;OK:&nbsp;pt&nbsp;has&nbsp;members&nbsp;x&nbsp;and&nbsp;y&nbsp;of&nbsp;type&nbsp;number</span></pre>

Note that we didn't annotate <tt><span class="code">pt</span></tt> in any way to indicate that it's of type <tt><span class="code">Point</span></tt>. We don't need to, because type checking in TypeScript is <span class="italic">structural</span>: types are considered identical if they have the same surface area. Because <tt><span class="code">pt</span></tt> has at least the same members as <tt><span class="code">Point</span></tt>, it's suitable for use wherever a <tt><span class="code">Point</span></tt> is expected.

<a name="Subsection_65"></a>

#### Describing External Types

Interfaces are also used to describe code that is present at runtime, but not implemented in the current TypeScript project. For example, if you open the <tt><span class="code">lib.d.ts</span></tt> file that all TypeScript projects implicitly reference, you'll see an interface declaration for <tt><span class="code">Number</span></tt>:

<pre><span class="keyword">interface</span>&nbsp;<span class="type-name">Number</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;toString(radix?:&nbsp;<span class="keyword">number</span>):&nbsp;<span class="keyword">string</span>;<br />&nbsp;&nbsp;&nbsp;&nbsp;toFixed(fractionDigits?:&nbsp;<span class="keyword">number</span>):&nbsp;<span class="keyword">string</span>;<br />&nbsp;&nbsp;&nbsp;&nbsp;toExponential(fractionDigits?:&nbsp;<span class="keyword">number</span>):&nbsp;<span class="keyword">string</span>;<br />&nbsp;&nbsp;&nbsp;&nbsp;toPrecision(precision:&nbsp;<span class="keyword">number</span>):&nbsp;<span class="keyword">string</span>;<br />}</pre>

Now if we have an expression of type <tt><span class="code">Number</span></tt>, the compiler knows that it's valid to call <tt><span class="code">toPrecision</span></tt> on that expression.

<a name="Subsection_66"></a>

#### Extending Existing Types

Moreover, interfaces in TypeScript are <span class="italic">open</span>, meaning you can add your own members to an interface by simply writing another <tt><span class="code">interface</span></tt> block. If you have an external script that adds members to <tt><span class="code">Date</span></tt>, for example, you simply need to write <tt><span class="keyword">interface</span> <span class="type-name">Date</span> { <span class="comment">/*...*/ </span>}</tt> and declare the additional members.*

<span class="italic">* Note: There are some known issues with the Visual Studio editor that currently prevent this scenario from working as intended. We'll be fixing this limitation in a later release.</span>

<a name="Title_67"></a>

# Describing an Indexable Object

A common pattern in JavaScript is to use an object (e.g. <tt><span class="code">{}</span></tt>) as way to map from a set of strings to a set of values. When those values are of the same type, you can use an interface to describe that indexing into an object always produces values of a certain type (in this case, <tt><span class="code">Widget</span></tt>).

<pre><span class="keyword">interface</span>&nbsp;<span class="type-name">WidgetMap</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;[name:&nbsp;<span class="keyword">string</span>]:&nbsp;Widget;<br />}<br /><br /><span class="keyword">var</span>&nbsp;map:&nbsp;<span class="type-name">WidgetMap</span>&nbsp;=&nbsp;{};<br />map['gear']&nbsp;=&nbsp;<span class="keyword">new</span>&nbsp;<span class="type-name">GearWidget</span>();<br /><span class="keyword">var</span>&nbsp;w&nbsp;=&nbsp;map['gear'];&nbsp;<span class="comment">//&nbsp;w&nbsp;is&nbsp;inferred&nbsp;to&nbsp;type&nbsp;Widget</span></pre>

<a name="Title_68"></a>

# Ensuring Class Instance Shape

Let's extend the <tt><span class="code">Greetable</span></tt> example above:

<pre><span class="comment">/**&nbsp;Represents&nbsp;an&nbsp;object&nbsp;that&nbsp;can&nbsp;be&nbsp;greeted&nbsp;*/</span><br /><span class="keyword">interface</span>&nbsp;<span class="type-name">Greetable</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;<span class="comment">/**&nbsp;Used&nbsp;to&nbsp;welcome&nbsp;someone&nbsp;*/</span><br />&nbsp;&nbsp;&nbsp;&nbsp;greet(message:&nbsp;<span class="keyword">string</span>):&nbsp;<span class="keyword">void</span>;<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;<span class="comment">/**&nbsp;The&nbsp;preferred&nbsp;language&nbsp;of&nbsp;this&nbsp;object&nbsp;*/</span><br />&nbsp;&nbsp;&nbsp;&nbsp;language:&nbsp;<span class="keyword">string</span>;<br />}</pre>

We can implement this interface in a class using the <tt><span class="code">implements</span></tt> keyword:

<pre><span class="keyword">class</span>&nbsp;<span class="type-name">Person</span>&nbsp;<span class="highlight"><span class="keyword">implements</span>&nbsp;Greetable</span>&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;language&nbsp;=&nbsp;'English';<br /><br />&nbsp;&nbsp;&nbsp;&nbsp;greet(message:&nbsp;<span class="keyword">string</span>)&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;console.log(message);<br />&nbsp;&nbsp;&nbsp;&nbsp;}<br />}</pre>

Now we can use an instance of <tt><span class="code">Person</span></tt> wherever a <tt><span class="code">Greetable</span></tt> is expected:

<tt><span class="keyword">var</span> g: Greetable = <span class="keyword">new</span> <span class="type-name">Person</span>();</tt>

Similarly, we can take advantage of the structural typing of TypeScript to implement <tt><span class="code">Greetable</span></tt> in an object literal:

<pre><span class="keyword">var</span>&nbsp;greeter&nbsp;=&nbsp;{<br />&nbsp;&nbsp;&nbsp;&nbsp;greet:&nbsp;(message:&nbsp;<span class="keyword">string</span>)&nbsp;=&gt;&nbsp;{&nbsp;console.log(message)&nbsp;};<br />&nbsp;&nbsp;&nbsp;&nbsp;language:&nbsp;'Any';<br />};</pre>