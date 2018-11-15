---
ID: 1005
post_title: 'TypeScript&#8217;s New Release Cadence'
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescripts-new-release-cadence/
published: true
post_date: 2017-03-27 18:05:44
---
One of the things we love about the TypeScript community is the enthusiasm around new features and rapid adoption of new TypeScript releases. Because of this, we have been focusing on increasing the velocity and consistency of TypeScript releases so that you can get your hands on the latest features even more quickly and predictably. This new release cadence has been mostly great, but there has been some confusion on when and how to get the latest TypeScript version. There have also been questions regarding availability of TypeScript in Visual Studio 2017. This blog post aims to clarify our intentions and our general plan for shipping TypeScript in the future, including shipping as a part of Visual Studio and Visual Studio Code. 
## New release cadence With TypeScript 2.0 and earlier releases, we didn't bother keeping a predictable release schedule. New versions were ready when they were ready, and we kept working on a release until it had all the features we wanted to be merged in. This was nice in that each TypeScript release felt large and impactful, but came with the downside of small fixes getting gated behind significant changes, and seemingly random amounts of time between releases. Furthermore, the scope of impact wasn't just limited to TypeScripr users. VS Code takes advantage of the TypeScript language service to power its JavaScript editing experience as well. This meant that any bug fixes for VS Code, for both TypeScript and JavaScript, would have to wait for a full release to be completed before shipping. Because VS Code ships every month, it sometimes took several VS Code releases before even minor bugs could be fixed. To both address the needs of VS Code and to get features out to the TypeScript community faster, we've moved to a monthly release cadence for TypeScript that mirrors 

[the cadence adapted by Visual Studio Code][1]. Going forward, TypeScript releases will adhere to the following principles: 
*   Release a **full feature release every two months** (e.g. 2.1, 2.2, etc.)
*   Publish at least **one TypeScript update to npm every month** (either patch or feature releases)
*   Features that aren't complete by release time are deferred to the next release
*   TypeScript's monthly release should come ~1 week before, and be included in, a VS Code release
*   Other editors can adopt the new TypeScript version during their next available update The following diagram may help visualize the new cadence. 

![][2] We believe that following these principles will benefit everyone in the TypeScript community. 
## TypeScript updates to Visual Studio Supporting the latest Typescript release in Visual Studio has been one of the goals that we've been working on as a team. However, when 

[Visual Studio 2017 RTM][3] released a few weeks back, we were not able to include the latest TS version (v2.2) with it. Instead, Visual Studio 2017 currently comes with built-in support for Typescript v2.1.5. With the changing cadence of TypeScript releases, we could not fully align the TypeScript v2.2 release with Visual Studio 2017. Given some key changes that have gone into the new setup authoring process for Visual Studio 2017, we need to do additional work to ensure that TypeScript releases can be applied to Visual Studio 2017 at a faster cadence as users might be used to in Visual Studio 2015 and VS Code. We've heard feedback from several users about their need to move to TypeScript v2.2 in Visual Studio 2017 and understand the confusion and pain this has caused. As a team, we're actively working on the problem and hope to have a fix available soon. We'll keep the community updated as we make progress. Once implemented, developers will have full flexibility to update as soon as a new version of TypeScript is available. We apologize for the confusion and want to assure you that fixing this issue is a top priority. 
## Drop us a line The great part about being an open source project is that we can make changes, get immediate feedback from the community, and quickly improve in the future. As such, we greatly appreciate your enthusiasm and would love to hear your feedback. As always, you can find us on 

[GitHub][4], [Twitter][5], and [Gitter][6].

 [1]: https://github.com/Microsoft/vscode/wiki/Development-Process
 [2]: http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/03/release-cadence-plan-timeline.png
 [3]: https://blogs.msdn.microsoft.com/visualstudio/2017/03/07/announcing-visual-studio-2017-general-availability-and-more/
 [4]: https://github.com/microsoft/typescript
 [5]: https://twitter.com/typescriptlang
 [6]: https://gitter.im/Microsoft/TypeScript