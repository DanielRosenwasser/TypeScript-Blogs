---
ID: 1067
post_title: >
  New TypeScript Quick Starts and Updates
  from Build 2017
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/new-typescript-quick-starts-and-updates-from-build-2017/
published: true
post_date: 2017-05-19 17:55:02
---
Last week Microsoft hosted its annual Build conference close to home in Seattle. Build is the single largest developer-focused conference that the company hosts, and draws a huge crowd from around the world! ![Going clockwise from the top left: Rob Wormald presenting on Angular, the TypeScript team with Sean Larkin, and two photos of Anders Hejlsberg's presentation.][1] The TypeScript team had the pleasure of meeting some friendly faces of our community, along with plenty of new people who were excited to learn about and try out TypeScript. If we got the chance to meet and chat with you, thank you for the opportunity to speak and give us your thoughts. It's humbling to meet our users personally, and we always appreciate getting feedback about the work we're doing. 
## New quick starts! One of the most common things we hear from developers is that while certain technologies might be manageable independently, it quickly becomes difficult to piece together individual components and have them work cohesively. Last year we worked on some "standalone" walkthroughs which gave more guidance, but found that they were still lacking. For instance, our React guide got users as far as getting TypeScript, Webpack, and React, but they were often lost when it came to things like testing, state management, etc. That's why, just in time for this year's Build conference, we launched a new 

[Quick Starts][2] section on our website! [ ![][3] ][2] There, we've linked to several guides, some written by our team, and others written by domain experts. For example, because Angular's documentation is so thorough and complete, we've decided to link directly to [get started with TypeScript and Angular][4]. We did the same for [Dojo 2][5] and [Glimmer][6]. When we felt that there was space to deliver best practices with TypeScript, such as with [React][7], [Node][8], [Vue][9], and [WeChat][10], we decided to dive deep and put together joint walkthroughs and samples. While you can decide to swap any linter, library, or test framework into these starters, our intent is that these technologies together already give you something greater than the sum of their parts. 
## What's new in TypeScript and this year's demos

<p class="code-line undefined undefined undefined undefined code-active-line">
  <span>Build had a </span><em>ton</em><span> of great talks this year. For example, our friends on the Angular team came and gave </span><a href="https://channel9.msdn.com/events/Build/2017/T6006-R1">a talk about how TypeScript is used in Angular and at Google</a><span>.</span>
</p>

<p class="code-line undefined">
  From our team, we had <a href="https://channel9.msdn.com/events/Build/2017/B8088">a talk from Anders this year on what's new in TypeScript</a>. His talk featured some new features we've been working on, as well as scenarios we've been working to support.
</p> Anders was able to show off a few great demos including 

[a SoundCloud client][11] in Angular built by [Richard Park][12], a [GitHub crawler][13] from us at Microsoft, and [a Vue TodoMVC sample that we ourselves built with TypeScript][14]. We also showed off [Emission][15], an open-source component library built with TypeScript and React Native for [Artsy][16]'s mobile app. Check out [our demo repository on GitHub here][17]! Thank you to everyone whose work we were able to showcase, and we'd like to offer a special thanks to [Orta Therox][18], [Eloy Durán][19], and the other great folks at Artsy who built and collaborated with us to show off Emission! 
## #iHeartTypeScript We always love hearing about how TypeScript can make you happier and more productive in your every-day coding. If you're using TypeScript and you want to let us know that you're enjoying it, we'd love to see that on Twitter with the 

[#iHeartTypeScript][20] hashtag. And while it's not as short as `iHeartTS` (sorry everyone, Taylor Swift already has dibs on that one), using this tag lets us on the team know what exactly makes TypeScript a joy to use for you! We should also mention that if there's ever something that we could improve, you should feel free to [file an issue with us on GitHub][21]. We hope those of you reading take a look at [our new quick starts][2] and get good use out of them! Keep us posted and let us know what you think [on Twitter][20] or in the comments below!

 [1]: http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/05/build-2017-yay.jpg
 [2]: http://www.typescriptlang.org/samples/index.html
 [3]: http://devblogs.microsoft.com/typescript/wp-content/uploads/sites/11/2017/05/build-2017-quick-starts-yay.png
 [4]: https://angular.io/docs/ts/latest/quickstart.html
 [5]: https://dojo.io/tutorials/002_creating_an_application/
 [6]: https://glimmerjs.com/guides/installing
 [7]: https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter
 [8]: https://github.com/Microsoft/TypeScript-Node-Starter#typescript-node-starter
 [9]: https://github.com/Microsoft/TypeScript-Vue-Starter#typescript-vue-starter
 [10]: https://github.com/Microsoft/TypeScript-WeChat-Starter
 [11]: https://github.com/r-park/soundcloud-ngrx
 [12]: https://github.com/r-park
 [13]: https://github.com/Microsoft/ghcrawler/tree/06586aca6c6f5750daab0ee18769071b909eebe3
 [14]: https://github.com/DanielRosenwasser/typescript-vue-todomvc
 [15]: https://github.com/artsy/emission/
 [16]: https://www.artsy.net/
 [17]: https://github.com/Microsoft/typescript-build2017-demos
 [18]: https://twitter.com/orta
 [19]: https://twitter.com/alloy
 [20]: https://twitter.com/intent/tweet?text=%23iHeartTypeScript
 [21]: https://github.com/Microsoft/TypeScript/issues/new