---
ID: 203
post_title: Announcing 0.9 early previews
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-0-9-early-previews/
published: true
post_date: 2013-04-22 08:48:00
---
Today we’re making an [early preview available][1] which showcases some of the new features coming in 0.9.0 preview release.  It’s intended for users who wish to try out these features, begin porting existing applications, and who don’t mind a few rough edges in the compiler and language service along the way.  Users who wish to stay on stable builds may wish to skip this early preview and wait for the 0.9.0 preview release (or grab the current [0\.8.3.1 preview release][2]).
## Background

The 0.9 series marks a major milestone on the way to our 1.0 release.  This series brings major changes to the TypeScript language, compiler, and language services.  With it, the language now supports generics, the top-most requested feature, along with improvements to the type system.  In addition, the compiler and language service have been rewritten, largely from scratch, to work better with large projects in an interactive environment, which allows working with projects of more than 100k lines with little to no slowdown.

## The Early Previews

Many users have requested the ability to try 0.9 during its development.  Up to this point, the best way to try it was to use the command-line based compiler by checking out the ‘develop’ branch, but because of the incompatibilities between the 0.8 and 0.9 language services there was no way to use the techniques mentioned in a previous [blog post][3], leaving users of Visual Studio without a way to also try out 0.9. 

The early previews are snapshots of the Visual Studio plugin that are compatible with the latest language service APIs in the 0.9 work.  The intent is to unblock users who wish to stay up-to-date and continue to explore 0.9 features as they’re created.

## How to Use It

By default, the first early preview contains a snapshot of the ‘develop’ branch called ‘release-0.9.0-alpha’ and can be installed just like our previous preview releases.  This snapshot features generics and overloading-on-constants.  We’re continuing to add new features and polish the implementation, so to keep up-to-date, you will need to update your compiler and language service from the ‘develop’ branch.  We’ve written about how to do this in a [blog post][3].

## Known Caveats

Because the early previews are snapshots of our current work-in-progress, not all functionality from 0.8.3 has been ported to the new architecture.  Features known to not be complete in the first early preview, or are known missing:

*   Refactor/rename 
*   Debugging support
*   Find all reference
*   Compile-on-save

## Breaking Changes

Due to the deep changes in the language and compiler, there are some incompatibilities between the 0.9 series and the last preview release, 0.8.3.  Some of these changes come from tightening the compiler to align more closely with the specification.  Others are breaking changes made to improve the overall TypeScript experience in the long run.  For the list of breaking changes between the 0.8.x series and the 0.9 series, see the [wiki page][4].

## Path to TypeScript 0.9

We’re continuing to push toward the 0.9 release, and we hope this early preview provides an opportunity to get some additional feedback on new and changed features.  For those who aren’t afraid to dive in to a work-in-progress, we hope this gives you the chance to help us make TypeScript a better, richer, project.

 [1]: https://typescript.codeplex.com/releases/view/105503
 [2]: http://www.microsoft.com/en-us/download/details.aspx?id=34790
 [3]: http://blogs.msdn.com/b/typescript/archive/2013/01/29/trying-the-latest-typescript-build.aspx
 [4]: https://typescript.codeplex.com/wikipage?title=Known%20breaking%20changes%20between%200.8%20and%200.9