---
ID: 1825
post_title: Announcing TypeScript 1.3
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-3/
published: true
post_date: 2014-11-12 07:30:00
---
We’re happy to announce the availability of TypeScript 1.3. TypeScript 1.3 includes two new features in the language and a new language service for Visual Studio 2015 that is built on the .NET Compiler Platform (“Roslyn”), Visual Studio's new language service that provides rich Intellisense. Roslyn makes it much easier to provide a premier editing experience for TypeScript in Visual Studio on par with the other first-class languages in Visual Studio.

TypeScript 1.3 is available as part of [Visual Studio 2015 Preview][1]. You can also install it for [Visual Studio 2013 via the power tool install][2], [NPM][3], and as [source][4].

# New Language Features<span style="font-size: 12px"> </span>

We’ve added two new language features in TypeScript 1.3: the “protected” access modifier and tuple types.

The “protected” access modifier is a [highly requested feature][5] that allows users the ability to use build even richer objected-oriented patterns in TypeScript. In this example, you can see how the protected keyword allows subclasses to gain visibility into the parent class without exposing this API to other parts of the code.

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> <span style="color: #00ccff">List</span><<span style="color: #00ccff">T</span>> {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">    private</span> contents: <span style="color: #00ccff">T</span>[];</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">    constructor</span>() {</span>  
<span style="font-family: 'courier new', courier"><span style="color: #0000ff">        this</span>.contents = [];</span>  
<span style="font-family: 'courier new', courier">    }<br />    <span style="color: #0000ff">protected</span> setElement(index: <span style="color: #0000ff">number</span>, item: <span style="color: #33cccc">T</span>) {<br />        <span style="color: #0000ff">this</span>.contents[index] = item;<br />    }</span>

<span style="font-family: 'courier new', courier">}<br /><br /><span style="color: #0000ff">class</span> <span style="color: #33cccc">Stack</span><<span style="color: #33cccc">T</span>> <span style="font-family: 'courier new', courier"><span style="color: #0000ff">extends</span> </span><span style="color: #33cccc">List</span><span><</span><span style="color: #33cccc">T</span><span>></span> {<br /></span><span style="font-family: 'courier new', courier">    currentIndex: <span style="color: #0000ff">number</span>;<br /><br />    <span style="color: #0000ff">constructor</span>() {<br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">        super</span>();<br />        <span style="color: #0000ff">this</span>.currentIndex = 0;<br />    }<br /><br /></span><span style="font-family: 'courier new', courier"><span style="color: #0000ff">    public</span> push(item: <span style="color: #33cccc">T</span>) {<br />        <span style="color: #0000ff">this</span>.setElement(<span style="color: #0000ff">this</span>.currentIndex, item);<br />        <span style="color: #0000ff">this</span>.currentIndex++;<br />    }<br />}<br /><br /><span style="color: #0000ff">var</span> stack = <span style="color: #0000ff">new</span> <span style="color: #33cccc">Stack</span><<span style="color: #0000ff">number</span>>();<br />stack.setElement(0, 1); <span style="color: #339966">// error 'setElement' is protected and only visible to subclasses </span></span>

Tuple types allow TypeScript to begin typing the new patterns we will see in the upcoming [ECMAScript 6 standard][6], where new features like array destructuring allow developers to work with arrays in richer ways. Tuple types have the advantage that now you can accurately describe the type of an array of mixed types, as in this example:

<span style="font-family: 'courier new', courier"><span style="color: #0000ff">var</span> tuple: [<span style="color: #0000ff">number</span>, <span style="color: #0000ff">string</span>] = [1, <span style="color: #ff0000">"bob"</span>];<br /><span style="color: #0000ff">var</span> secondElement = tuple[1];  <span style="color: #339966">// secondElement now has type 'string' </span></span>

 

# New Visual Studio Features<span style="font-size: 12px"> </span>

With TypeScript 1.3, we’ve built a new Visual Studio 2015 language service on Roslyn, the compiler platform that's a part of the [open source .NET Foundation][7]. Roslyn allows us to enable a number of new features for editing TypeScript code.

 

## Faster and more accurate find all references

We have made fundamental changes to how compute and report references. Computing references is now significantly faster on the first attempt. Find All References now also reports locations where a contextual type is present, and reports indexed access to properties.

# [<img src="https://devblogs.microsoft.com/00/00/01/56/67/5661.FasterAndMoreAccurateFind.PNG" alt="" border="0" />][8] 

## Enhanced completion list

TypeScript now supports the same trigger commit characters as C#, so “enter” or “tab”  both commit your selection from the completion list. TypeScript also has completion list filtering based on camel casing.

 [<img src="https://devblogs.microsoft.com/00/00/01/56/67/7673.completion_list.png" alt="" border="0" />][9] 

## Enhanced outlining

With the “Roslyn” language service everything is now collapsible –  including functions, object literals, classes, and control flow blocks.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/2161.enhanced_outlining.jpg" alt="" border="0" />][10]

## Enhanced Colorization

Classes, Interfaces, modules, enums are now colored according to their kind.  Keywords are also now colorized correctly according to their context.

# [<img src="https://devblogs.microsoft.com/00/00/01/56/67/5518.EnhancedColorization.PNG" alt="" border="0" />][11]

 

## Peek<span style="font-size: 12px"> </span>

Editor peek is now available, enabling you to quickly see declarations at a glance complete with colorization.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/1134.Peek.PNG" alt="" border="0" />][12]

## Enhanced rename

TypeScript now supports inline rename with even more accuracy than before. Rename finds all instances the name even if it does not have an explicit type annotation –  including strings used in property access.

 [<img src="https://devblogs.microsoft.com/00/00/01/56/67/8168.rename.png" alt="" border="0" />][13]<span style="font-size: 2em"> </span>

## TODO comments support

 

TODO comments are now populated in the tasks list.

 [<img src="https://devblogs.microsoft.com/00/00/01/56/67/5314.todo_comments.jpg" alt="" border="0" />][14]

 

## Better support for functional programming style

We have made several changes to ensure that our tools work well with different JavaScript coding patterns. Specifically, we've improved support for the functional programming style by improving how the editor works with nested functions. Now "Navigate To" shows you nested functions, and the navigation bar shows where you are based on your enclosing function scopes.

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/7144.BetterSupportForFunctional.png" alt="" border="0" />][15]

 

# Looking Ahead

We're pleased to enable more coding patterns and improve the Visual Studio experience of working with TypeScript. We look forward to continuing to enhance the language and tools for working with large-scale JavaScript projects. To find out more about what we'll be working on next, check out our [roadmap][16]. We encourage you to try out this release and [give us your feedback][17].

 [1]: http://go.microsoft.com/fwlink/?LinkId=517033 "Visual Studio 2015 Preview"
 [2]: https://visualstudiogallery.msdn.microsoft.com/955e0262-0858-40c9-ab5a-1acc680e9bfd "Visual Studio 2013 via the power tool install"
 [3]: https://www.npmjs.org/package/typescript "NPM"
 [4]: https://github.com/Microsoft/TypeScript/releases/tag/v1.3 "source"
 [5]: http://typescript.codeplex.com/workitem/125
 [6]: http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts
 [7]: http://www.dotnetfoundation.org/prjnetcompilerroslyn.aspx
 [8]: https://devblogs.microsoft.com/00/00/01/56/67/5661.FasterAndMoreAccurateFind.PNG
 [9]: https://devblogs.microsoft.com/00/00/01/56/67/7673.completion_list.png
 [10]: https://devblogs.microsoft.com/00/00/01/56/67/2161.enhanced_outlining.jpg
 [11]: https://devblogs.microsoft.com/00/00/01/56/67/5518.EnhancedColorization.PNG
 [12]: https://devblogs.microsoft.com/00/00/01/56/67/1134.Peek.PNG
 [13]: https://devblogs.microsoft.com/00/00/01/56/67/8168.rename.png
 [14]: https://devblogs.microsoft.com/00/00/01/56/67/5314.todo_comments.jpg
 [15]: https://devblogs.microsoft.com/00/00/01/56/67/7144.BetterSupportForFunctional.png
 [16]: http://blogs.msdn.com/b/typescript/archive/2014/10/22/typescript-and-the-road-to-2-0.aspx
 [17]: https://github.com/microsoft/typescript/issues