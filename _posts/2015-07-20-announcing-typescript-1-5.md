---
ID: 411
post_title: Announcing TypeScript 1.5
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-5/
published: true
post_date: 2015-07-20 07:55:00
---
Today we’re happy to announce the release of TypeScript 1.5.  This release took an [alpha][1], a [beta][2], and your help to get here.  It’s a big one, so let’s get started!

TypeScript 1.5 is part of the newly released [Visual Studio 2015][3].  You can also get a separate [download for Visual Studio 2013][4], [npm][5], and straight from [GitHub][6].

# ES6 support

[<img src="https://devblogs.microsoft.com/00/00/01/56/67/1307.Kangax.PNG" alt="" width="470" height="200" />  
][7]*TypeScript 1.5 - closing the gap on [Kangax ES6 support][8]*

 

TypeScript 1.5 adds a number of new ES6 features including [modules][9], [destructuring][10], [spread][11], [for..of][12], [symbols][13], [computed properties][14], [let/const][15], and [tagged string templates][16].  There is quite a bit of information available in the links above, including samples of how to use these features.  With these features, TypeScript takes a big step in completing its goal of becoming a superset of ES6 and offering type-checking for all of ES6’s major features

<div>
  <p>
    In the above table, you can see our progress on the Kangax ES6 support table.  This table, originally for JS engines, also shows coverage of the features transpilers and polyfills support for ES5 output.  With TypeScript 1.5, we doubled the number of passing tests and will continue to improve over the next few releases.
  </p>
</div>

# Modules

There has been quite a bit of work on how modules work in the 1.5 release.  With this release, we’ve begun supporting the official ES6 modules, we’re simplifying how modules work, and we’re adding support for more kinds of modules as output.

## ES6 modules

TypeScript 1.5 supports the new module syntax from ES6.  The ES6 module syntax offers a rich way of working with modules.  Similar to external modules in TypeScript, ES6 modules can import modules and exports each piece of your public API.  Additionally, ES6 modules allow you to selectively import the parts of that public API you want to use.

<div>
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> * as Math <span style="color: #0000ff">from</span> <span style="color: #ff0000">"my/math"</span>;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff"> import</span> { add, subtract } <span style="color: #0000ff">from</span> <span style="color: #ff0000">"my/math"</span>;</span>
  </p>
  
  <span>You can also work with the module itself using a ‘default’ export.  The default allows you a handle on what the module main content is.  This gives you even more precise control over the API you make available.</span>
</div>

<div>
  <p>
    <span style="font-family: 'courier new', courier;color: #008080">// math.ts</span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">export</span> <span style="color: #0000ff">function</span> add(x, y) { <span style="color: #0000ff">return</span> x + y }</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff"> export</span> <span style="color: #0000ff">function</span> subtract(x, y) { <span style="color: #0000ff">return</span> x – y }</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff"> export</span> <span style="color: #0000ff">default</span> <span style="color: #0000ff">function</span> multiply(x, y) { <span style="color: #0000ff">return</span> x * y }</span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier;color: #008080">// myFile.ts</span>
  </p>
  
  <p>
    <span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> {add, subtract} <span style="color: #0000ff">from</span> <span style="color: #ff0000">"math"</span>;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff"> import</span> times <span style="color: #0000ff">from</span> <span style="color: #ff0000">"math"</span>;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff"> var</span> result = times(add(2, 3), subtract(5, 3));</span>
  </p>
  
  <div>
    <div>
      <div>
        <p>
          If you look closely, you can see the 'export default' used as the last line of <span style="font-family: 'courier new', courier">math.ts</span>.  This line allows us to control a 'default' export, which is what is exported when you don't import specific exports with curly braces ({ }) but instead use a name, like the second line of <span style="font-family: 'courier new', courier">myFiles.ts</span>.
        </p>
        
        <h2>
          Simplifying modules
        </h2>
      </div>
    </div>
  </div>
  
  <p>
    One of the common points of feedback we’ve heard as new users pick up TypeScript for the first time is that the modules are a bit confusing.  Before ES6, there were internal and external modules.  Now with support for ES6 modules, there is now another module to learn about.  We’re simplifying this with the 1.5 release.
  </p>
  
  <p>
    Going forward, internal modules will be called ‘namespace’.  We chose to use this term because of the closeness between how this form works and namespaces in other languages, and how the pattern in JS, sometimes called IIFE, is used in practice. We’ve already updated the handbook to reflect this change.  We’re encouraging teams to use the new terminology and corresponding syntax.
  </p>
  
  <p>
    Likewise, external modules just become ‘modules’, with a strong emphasis on the standard ES6 module syntax.  With these changes, there is now just one ‘module’, and it works like the corresponding concept in JavaScript.  
  </p>
  
  <h2>
    New module output
  </h2>
  
  <p>
    TypeScript has supported multiple module loaders since the early days.  Because JavaScript is used in both the browser and on the server, TypeScript has supported compiling modules for either AMD or CommonJS.  
  </p>
  
  <p>
    We’re adding two new module output formats to help continue support more JavaScript practices: SystemJS and UMD.  SystemJS will allow you to use ES6 modules closer to their native semantics without requiring an ES6-compatible browser engine.  UMD gives you a way to output a single module that works in both AMD and CommonJS.
  </p>
  
  <div>
    <h1>
      Lightweight, portable projects<strong></strong>
    </h1>
    
    <p>
      One of the tricky things with TypeScript projects is that it’s not often easy to move from a single file to working with a growing project of files.  You generally have two options: adding<span style="font-family: 'courier new', courier"> ///<reference></span> statements to tie your project together, or manually handling everything on the commandline.  Neither approach is particularly clean, and easily become a mess as the project grows.  Additionally, only the <span style="font-family: 'courier new', courier">///<reference></span> approach works well with editors, so you inevitably have a number of them in addition to your build.
    </p>
    
    <p>
      TypeScript 1.5 introduces a new feature to make getting started withTypeScript easier.  The compiler now supports ‘<a href="https://github.com/Microsoft/TypeScript/pull/1692">tsconfig.json</a>’, a new file which allows you to specify the files in your project and the compiler settings to use.  This lets you create a lightweight project that can be used both on the command-line and within the editor.  In fact, VS Code, Sublime, Atom, and others already support using tsconfig.json files.
    </p>
  </div>
  
  <h1>
    Decorators
  </h1>
  
  <div>
    <p>
      The 1.5 release also adds support for the proposed Decorator feature of ES7, which is currently being developed in collaboration with the <a href="https://angular.io/">Angular</a>, <a href="http://emberjs.com/">Ember</a>, and <a href="http://aurelia.io/">Aurelia</a> teams.  Since Decorators are being defined in ES7, which hasn’t stabilized yet, the feature is considered ‘experimental’, but it is already showing how powerful it is when working with rich libraries and applications. 
    </p>
    
    <p>
      <span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> {Component, View, NgFor, bootstrap} <span style="color: #0000ff">from</span> <span style="color: #ff0000">"angular2/angular2"</span>;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> {loadFile} <span style="color: #0000ff">from</span> <span style="color: #ff0000">"audioFile"</span>;</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">import</span> {displayAudioFile} from <span style="color: #ff0000">"displayAudio"</span>;</span>
    </p>
    
    <p>
      <span style="font-family: 'courier new', courier">@Component({selector: <span style="color: #ff0000">'file-list'</span>})</span><br /><span style="font-family: 'courier new', courier">@View({template: <span style="color: #ff0000">`</span></span><br /><span style="font-family: 'courier new', courier;color: #ff0000">  <select id="fileSelect" size="5"></span><br /><span style="font-family: 'courier new', courier;color: #ff0000">    <option *ng-for="#item of items; #i = index"</span><br /><span style="font-family: 'courier new', courier;color: #ff0000">      [selected]="selected === item"(click)="updateSelection()">{{ item }}</option></span><br /><span style="font-family: 'courier new', courier"><span style="color: #ff0000">  </select>`</span>,</span><br /><span style="font-family: 'courier new', courier">  directives: [NgFor]</span><br /><span style="font-family: 'courier new', courier">})</span>
    </p>
    
    <p>
      <span style="font-family: 'courier new', courier"><span style="color: #0000ff">class</span> MyDisplay {</span><br /><span style="font-family: 'courier new', courier">  items: <span style="color: #0000ff">string</span>[];</span><br /><span style="font-family: 'courier new', courier">  constructor() {</span><br /><span style="font-family: 'courier new', courier">    this.items = [<span style="color: #ff0000">"item1"</span>, <span style="color: #ff0000">"item2"</span>];</span><br /><span style="font-family: 'courier new', courier">  }</span><br /><br /><span style="font-family: 'courier new', courier">  updateSelection() { … }</span><br /><span style="font-family: 'courier new', courier">}</span><br /><em>Using decorators in Angular 2</em>
    </p>
    
    <p>
      Decorators allow you to attach metadata to classes and members, as well as update the functionality of what is being decorated.  As you can see above, Angular 2 uses Decorators to define the HTML selector and template on a class directly. We’re excited to see what else developers do with this feature.
    </p>
    
    <p>
      Note: to use decorators in your projects, you'll need to pass the <strong>--experimentalDecorators</strong> flag to the compiler.
    </p>
    
    <h1>
      What’s next
    </h1>
    
    <p>
      The 1.5 release has significant new language features to use in your projects.  It’s been a fairly long release, and we’re excited to hear your feedback on TypeScript 1.5 as well as jump in and <a href="https://github.com/Microsoft/TypeScript/wiki/Roadmap">finish TypeScript 1.6</a>.
    </p>
    
    <p>
      We’ve also heard your feedback that you’d like smaller releases, more often.  We’re currently working on ways to make that happen, so you get great features, high quality, and don’t have to wait long to get it.  Stay tuned…
    </p>
  </div>
  
  <h1>
    Thanks!
  </h1>
  
  <div>
    This release was possible because of the all the contributors that helped make it happen.  Special thanks to everyone in this list, who all contributed code to the 1.5 release:
  </div>
</div>

<div>
  <ul>
    <li>
      Ahmad Farid 
    </li>
    <li>
      Anders Hejlsberg
    </li>
    <li>
      Arnav Singh
    </li>
    <li>
      Basarat Ali Syed 
    </li>
    <li>
      Bill Ticehurst 
    </li>
    <li>
      Bryan Forbes 
    </li>
    <li>
      Caitlin Potter 
    </li>
    <li>
      Chris Bubernak
    </li>
    <li>
      Colin Snover
    </li>
    <li>
      Cyrus Najmabadi
    </li>
    <li>
      Dan Quirk 
    </li>
    <li>
      Daniel Rosenwasser
    </li>
    <li>
      Dick van den Brink 
    </li>
    <li>
      Dirk Bäumer 
    </li>
    <li>
      Frank Wallis 
    </li>
    <li>
      Guillaume Salles 
    </li>
    <li>
      Ivo Gabe de Wolff 
    </li>
    <li>
      James Whitney 
    </li>
    <li>
      Jason Freeman
    </li>
    <li>
      Jason Ramsay 
    </li>
    <li>
      Johannes Rieken 
    </li>
    <li>
      Jonathan Bond-Caron
    </li>
    <li>
      Kagami Sascha Rosylight
    </li>
    <li>
      Keith Mashinter
    </li>
    <li>
      Lorant Pinter 
    </li>
    <li>
      Masahiro Wakame
    </li>
    <li>
      Mohamed Hegazy 
    </li>
    <li>
      Oleg Mihailik
    </li>
    <li>
      Paul van Brenk 
    </li>
    <li>
      Pedro Maltez 
    </li>
    <li>
      Ron Buckton 
    </li>
    <li>
      Ryan Cavanaugh 
    </li>
    <li>
      Sheetal Nandi
    </li>
    <li>
      Shengping Zhong
    </li>
    <li>
      Stan Thomas
    </li>
    <li>
      Steve Lucco
    </li>
    <li>
      Tingan Ho
    </li>
    <li>
      Tomas Grubliauskas
    </li>
    <li>
      TruongSinh Tran-Nguyen 
    </li>
    <li>
      Vladimir Matveev
    </li>
    <li>
      Yui Tanglertsampan
    </li>
    <li>
      Zev Spitz 
    </li>
    <li>
      Zhengbo Li
    </li>
  </ul>
</div>

 [1]: http://blogs.msdn.com/b/typescript/archive/2015/03/27/announcing-typescript-1-5-alpha.aspx
 [2]: http://blogs.msdn.com/b/typescript/archive/2015/04/30/announcing-typescript-1-5-beta.aspx
 [3]: http://go.microsoft.com/fwlink/?LinkId=517106
 [4]: https://visualstudiogallery.msdn.microsoft.com/b1fff87e-d68b-4266-8bba-46fad76bbf22
 [5]: https://www.npmjs.com/package/typescript
 [6]: https://github.com/Microsoft/TypeScript
 [7]: https://devblogs.microsoft.com/00/00/01/56/67/1307.Kangax.PNG
 [8]: https://kangax.github.io/compat-table/es6/
 [9]: https://github.com/Microsoft/TypeScript/issues/2242
 [10]: https://github.com/Microsoft/TypeScript/pull/1346
 [11]: https://github.com/Microsoft/TypeScript/pull/1931
 [12]: https://github.com/Microsoft/TypeScript/pull/2207
 [13]: https://github.com/Microsoft/TypeScript/pull/1978
 [14]: https://github.com/Microsoft/TypeScript/issues/1082
 [15]: https://github.com/Microsoft/TypeScript/pull/2161
 [16]: https://github.com/Microsoft/TypeScript/pull/1589