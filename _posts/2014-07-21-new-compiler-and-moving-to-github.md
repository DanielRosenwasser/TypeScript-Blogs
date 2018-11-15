---
ID: 1663
post_title: New Compiler and Moving to GitHub
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/new-compiler-and-moving-to-github/
published: true
post_date: 2014-07-21 09:52:00
---
Today we’re announcing two changes to the TypeScript project.
# Introducing the New Compiler

The first change we’re making to TypeScript is that we’ve been taking a good, hard look at the compiler performance we had with 1.0.  We knew we could leverage the experience from building the original compiler over the last two years.  This led to experimenting with a new, lighter-weight compiler core.  The early results with this new compiler core were so positive that we’re now focused on growing this core into the new TypeScript compiler and language service.

The first stage of this effort is to build a complete standalone compiler based on this new core architecture.  The new compiler follows the same TypeScript 1.0 language spec, has the same compiler flags and compiler functionality, and outputs nearly identical code to the currently shipping TypeScript compiler.  Once complete, the new compiler should be drop-in compatible with the existing one.  The goal here isn't new compiler features, but rather a cleaner compiler architecture that has better performance and allows us to add new features with greater ease in releases to come.

Our work to date on the new compiler has been very promising.  At its current level of completeness, the new compiler is able to compile existing real-world TypeScript code 5x faster than the currently shipping compiler.  These results are still early. Once the compiler has reached parity, we'll be able to report out a more complete picture of the performance improvements.

We have a good start, but there’s a lot of work to do. For the experiment to mature into a full-featured compiler, in addition to making sure the code is robust and correct, we still need to flesh out the following features: 

*   **Parser** – Strict mode, incremental parsing for the language service (if necessary).
*   **Type checking **– Get type checker to parity with 1.0 in terms of error coverage.  This is nearing completion.
*   **Compiler options** – Support for --propagateEnumConstants, --watch, --locale, --logFile and --version. 
*   **Language service **– Adapt the language service to work with new compiler.
*   **Emitter** – .d.ts output and comment preservation.

As we reach parity with our existing compiler, we’re looking forward to moving on to ECMAScript 6 features and exploring the top user requests.

If you’d like to help us out, you can find the new compiler at our new [TypeScript repository on GitHub][1], which leads us to change #2.

# Moving to GitHub

We’ve [heard your feedback][2], and we’re making the move to GitHub.  We’re continually looking for ways to improve how TypeScript works with the community, and we’re excited about what the move to GitHub could help us do in the coming months.

The new site will work similarly to the current one. 

*   Source code moves to the [TypeScript git repository on GitHub][1]
*   Issues move to the [TypeScript issues on GitHub][3]

*   ***Please note: ****since we’re moving to a new issue tracker and a new codebase, we are not copying all issues over.  If you have issues in the current CodePlex issue tracker, please try them against the new codebase and file them in the GitHub issue tracker if they still repro.*

*   Wiki documentation moves to the [TypeScript wiki on GitHub][4]
*   Forums will now be more organized. 
*   For questions about how to use TypeScript and how to work with various JavaScript libraries and patterns, please use the [TypeScript StackOverflow site][5].  This helps users have a one-stop place to find the best answers and get started quickly. 
*   For feature requests and design discussions, please post to the [TypeScript issues on GitHub][3] 

The blog will continue to be hosted on MSDN and will not change at this time.

 [1]: https://github.com/Microsoft/TypeScript
 [2]: https://typescript.codeplex.com/workitem/2420
 [3]: https://github.com/Microsoft/TypeScript/issues
 [4]: https://github.com/Microsoft/TypeScript/wiki
 [5]: http://stackoverflow.com/questions/tagged/typescript