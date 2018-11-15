---
ID: 1661
post_title: TypeScript and the Road to 2.0
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescript-and-the-road-to-2-0/
published: true
post_date: 2014-10-22 09:27:00
---
When we released TypeScript 1.0 earlier this year, we focused on putting out a language that would help developers really scale their JavaScript projects.  It’s been quite a ride watching what people have done with it, including [Mozilla Shumway][1] at 170,000 lines, [Walmart stationery][2], and the rich [Microsoft Azure][3] experience, which is now over a million lines of code! 

Our goal with TypeScript is to continue supporting projects of this size and to make it the best language we can for JavaScript at scale.  With 1.1, we released a fast, lightweight compiler that was capable of compilation speeds 4x faster than before.  The new compiler is also more flexible for adding new features, which we’ll use going into 2.0.

Today, we want to talk about the roadmap to 2.0.  We encourage you to join us on the [TypeScript GitHub][4] page and help us make TypeScript the best it can be.

# JavaScript Typing Community Grows

The JavaScript typing community continues to grow, which is exciting to watch and be a part of.  [DefinitelyTyped][5] now contains over 700 .d.ts typings for a wide array of JavaScript libraries and frameworks, and is nearing its 600th contributor.

We're also seeing new projects that build on the work of the JavaScript typing community.  The first of these, Facebook's Flow language, was announced earlier this year.  It builds on modules as a fundamental building block for applications and to that adds rich type inference and .d.ts compatibility.  The second project, called AtScript, was recently announced by the Angular team at Google.  It builds on the TypeScript-style type annotations and adds metadata annotations and runtime checks.

The TypeScript team is working with both the Flow and AtScript teams to help ensure that resources that have already been created by the JavaScript typing community can be used across these tools.  There's a lot these projects can learn from each other, and we're looking forward to working together going forward and creating the best tools we can for the JavaScript community.  In the long term, we will also be working to fold the best features of these tools into ECMAScript, the standard behind JavaScript.

# Roadmap

## TypeScript 1.3

Our next release will be TypeScript 1.3 and will include a newly rewritten language service to enable a smoother, faster TypeScript development experience in editors.  This also marks the first release where we will begin standardizing the API of the compiler so that tools can depend on it.  As part of this release, we’ll also preview a new Visual Studio experience that will become part of Visual Studio.

On the language side, we are rolling out two new features: [protected][6] and [tuple types][7]. The protected access modifier has long been a [highly requested feature][8] and will help lead the way for more object-oriented patterns in TypeScript.  With tuple types, we’ll begin extending the type system to work with more of the practices that will be common in the upcoming ECMAScript 6.  Specifically, this will allow type-safe usage of destructuring arrays that are treated as tuples.

You might ask “where is TypeScript 1.2?”  We had intended to use that release as a stabilization release, but 1.1 has proven stable enough that teams are moving to it.  This lets us jump to finishing the language service work and getting new features out sooner than we had originally planned.

## TypeScript 1.4

In the 1.4 release, we’re focusing on growing the language even further.  The first features are already available in the master branch on GitHub.  These include [union types][9] and using typeof in if-blocks to refine the type.  You can see both features in this example:

<span style="font-family: courier new,courier"><span style="color: #0000ff">function</span> createCustomer(name: { firstName: <span style="color: #0000ff">string</span>; lastName: <span style="color: #0000ff">string</span> } | <span style="color: #0000ff">string</span>) {<br /></span><span style="font-family: courier new,courier"><span style="color: #0000ff">    if</span> (<span style="color: #0000ff">typeof</span> name === "string") {</span>  
<span style="color: #339966;font-family: courier new,courier">        // Because of the typeof check in the if, we know name has type string</span>  
<span style="font-family: courier new,courier">        return { fullName: name };</span>  
<span style="font-family: courier new,courier">    }</span>  
<span style="font-family: courier new,courier"><span style="color: #0000ff">    else</span> {</span>  
<span style="color: #339966;font-family: courier new,courier">        // Since it's not a string, we know name </span><span style="color: #339966;font-family: courier new,courier">has <br />        // </span><span style="color: #339966;font-family: courier new,courier">type { firstName: string; lastName: string }</span>  
<span style="font-family: courier new,courier"><span style="color: #0000ff">        return</span> { fullName: name.firstName + <span style="color: #ff0000">" "</span> + name.lastName };</span>  
<span style="font-family: courier new,courier">    }</span>  
<span style="font-family: courier new,courier">} <br /><br /><span style="color: #339966;font-family: courier new,courier"> // Both customers have type { fullName: string }</span><br /></span><span style="font-family: courier new,courier"><span style="color: #0000ff">var</span> customer = createCustomer(<span style="color: #ff0000">"John Smith"</span>); <br /></span><span style="font-family: courier new,courier"><span style="color: #0000ff">var</span> customer2 = createCustomer({ firstName: <span style="color: #ff0000">"Samuel"</span>, lastName: <span style="color: #ff0000">"Jones"</span> });</span>

These features work together to allow more natural patterns when working with JavaScript code.  Union types also help alleviate some of the pain points in earlier versions of TypeScript when working with heterogeneous arrays by giving a more precise type when there are multiple options. 

## From TypeScript 1.5 to TypeScript 2.0

As we look to the 2.0 release, we 're focusing on two goals in addition to our main goal of bringing good tooling to JavaScript development.  The first is to align with ES6.  Aligning with ES6 allows TypeScript to become a superset of the next JavaScript, opening the way for working with new code patterns like destructuring, string templates, promises, iterators and more in addition to features TypeScript already supports, such as classes and lambda functions.  We're also working with the Flow and Angular teams to make sure that TypeScript is the best language for working with a broader range of libraries, including declarative frameworks like the upcoming Angular 2.0 release. 

# Looking Forward

There is a lot of work ahead, but we’re already making good progress along this roadmap.  You can already try some of the features mentioned above by grabbing the latest source at the [TypeScript GitHub][4] repo.  We’ve adopted the GitHub style of putting out specifications along with a pull request.  This lets you follow features as they are developed, and we’d love to hear your feedback.

 

 [1]: https://github.com/mozilla/shumway
 [2]: http://www.walmartstationery.com/estore/
 [3]: http://azure.microsoft.com/
 [4]: https://github.com/Microsoft/TypeScript/
 [5]: https://github.com/borisyankov/DefinitelyTyped
 [6]: https://github.com/Microsoft/TypeScript/pull/688
 [7]: https://github.com/Microsoft/TypeScript/pull/428
 [8]: http://typescript.codeplex.com/workitem/125
 [9]: https://github.com/Microsoft/TypeScript/pull/824