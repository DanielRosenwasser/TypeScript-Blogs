---
ID: 333
post_title: 'Angular 2: Built on TypeScript'
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/angular-2-built-on-typescript/
published: true
post_date: 2015-03-05 08:00:00
---
<p dir="ltr">
  <span>We're excited to unveil the result of a months-long partnership with the Angular team.</span>
</p>

<p dir="ltr">
  <span>This partnership has been very productive and rewarding experience for us, and as part of this collaboration, we're happy to announce that Angular 2 will now be built with TypeScript.  We're looking forward to seeing what people will be able to do with these new tools and continuing to work with the Angular team to improve the experience for Angular developers. </span>
</p>

<p dir="ltr">
  <span>The first fruits of this collaboration will be in the upcoming TypeScript 1.5 release. </span>
</p>

<p dir="ltr">
  <span>We have worked with the Angular team to design a set of new features that will help you develop cleaner code when working with dynamic libraries like Angular 2, including a new way to annotate class declarations with metadata.  Library and application developers can use these metadata annotations to cleanly separate code from information about the code, such as configuration information or conditional compilation checks.  </span>
</p>

<span id="docs-internal-guid-19bc4ce9-e0e6-ee24-129e-3671104fad48"><span>We've also added a way to retrieve type information at runtime.  When enabled, this will enable developers to do a simple type introspection.  To verify code correctness with additional runtime checks.  It also enables libraries like Angular to use type information to set up dependency injection based on the types themselves.</span></span>

## TodoMVC for Angular 2 in TypeScript

<p dir="ltr">
  <span>At ng-conf, we are previewing this work by showing a TodoMVC example, based on </span><a href="https://github.com/davideast/ng2do"><span>David East’s Angular 2 TodoMVC</span></a><span>.  You can </span><a href="https://github.com/Microsoft/ngconfdemo"><span>try this example</span></a><span> out for yourself. If you’re new to TypeScript, you can also learn TypeScript through our </span><a href="http://www.typescriptlang.org/Playground"><span>interactive playground</span></a><span>.</span>
</p>

<span id="docs-internal-guid-19bc4ce9-e0e7-2b93-9951-d64b1ec067f9"><span>We’d love to hear your feedback. </span></span>

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/0820.Sublime_Intellisense.png" alt="" border="0" />][1]

<p dir="ltr">
  <em>TypeScript autocomplete in Sublime 3 for Angular 2</em>
</p>

<span id="docs-internal-guid-19bc4ce9-e0e7-8fd5-1136-5e23ba09c62b"><span>We’re looking forward to releasing a beta of TypeScript 1.5 in the coming weeks, and along with it, growing TypeScript’s tooling support to include more development styles and environments.  We'd also like to give a huge thanks to Brad, Igor, Miško on the Angular team for being great partners.  Special shout out to Yehuda Katz, who helped us design the annotation+decorator proposal which helped make this work possible. </span></span>

 [1]: https://devblogs.microsoft.com/00/00/01/56/67/0820.Sublime_Intellisense.png