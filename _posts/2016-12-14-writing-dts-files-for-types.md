---
ID: 915
post_title: Writing Declaration Files for @types
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/writing-dts-files-for-types/
published: true
post_date: 2016-12-14 18:11:33
---
<div style="font-size: 16px">
  <a href="https://blogs.msdn.microsoft.com/typescript/2016/06/15/the-future-of-declaration-files/">A while back</a> we talked about how TypeScript 2.0 made it easier to grab declaration files for your favorite library. Declaration files, if you're not familiar, are just files that describe the shape of an existing JavaScript codebase to TypeScript. By using declaration files (also called <code>.d.ts</code> files), you can avoid misusing libraries and get things like completions in your editor. As a recap of that previous blog post, if you're using an npm package named <code>foo-bar</code> and it doesn't ship any <code>.d.ts</code> files, you can just run <div class="highlight highlight-source-shell">
    <pre>npm install -S @types/foo-bar</pre>
  </div> and things will just work from there. But you might have asked yourself things like "where do these 'at-types' packages come from?" or "how do I update the 
  
  <code>.d.ts</code> files I get from it?". We're going to try to answer those very questions. <h2>
    DefinitelyTyped
  </h2> The simple answer to where our 
  
  <code>@types</code> packages come from is <a href="https://github.com/DefinitelyTyped/DefinitelyTyped">DefinitelyTyped</a>. DefinitelyTyped is just a simple repository on GitHub that hosts TypeScript declaration files for all your favorite packages. The project is community-driven, but supported by the TypeScript team as well. That means that anyone can help out or contribute new declarations at any time. <h2>
    Authoring New Declarations
  </h2> Let's say that we want to create declaration files for our favorite library. First, we'll need to fork 
  
  <a href="https://github.com/DefinitelyTyped/DefinitelyTyped">DefinitelyTyped</a>, clone your fork, and create a new branch. <div class="highlight highlight-source-shell">
    <pre>git clone https://github.com/YOUR_USERNAME_HERE/DefinitelyTyped
cd DefinitelyTyped
git checkout -b my-favorite-library</pre>
  </div> Next, we can run an 
  
  <code>npm install</code> and create a new package using the <code>new-package</code> npm script. <div class="highlight highlight-source-shell">
    <pre>npm install
npm run new-package my-favorite-library</pre>
  </div> For whatever library you use, 
  
  <code>my-favorite-library</code> should be replaced with the <strong>verbatim name</strong> that it was published with on npm. If for some reason the package doesn't exist in npm, mention this in the pull request you send later on. The <code>new-package</code> script should create a new folder named <code>my-favorite-library</code> with the following files: <ul>
    <li>
      <code>index.d.ts</code>
    </li>
    <li>
      <code>my-favorite-library-tests.ts</code>
    </li>
    <li>
      <code>tsconfig.json</code>
    </li>
    <li>
      <code>tslint.json</code>
    </li>
  </ul> Finally we can get started writing our declaration files. First fix up the comments for 
  
  <code>index.d.ts</code> by adding the library's MAJOR.MINOR version, the project URL, and your username. Then, start describing your library. Here's what <code>my-favorite-library/index.d.ts</code> might look like: <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Type definitions for my-favorite-library x.x</span>
<span style="color: #148A14">// Project: https://github.com/my-favorite-library-author/my-favorite-library</span>
<span style="color: #148A14">// Definitions by: Your Name Here &lt;https://github.com/YOUR_GITHUB_NAME_HERE&gt;</span>
<span style="color: #148A14">// Definitions: https://github.com/DefinitelyTyped/DefinitelyTyped</span>

<span style="color: #00f">export</span> <span style="color: #00f">function</span> getPerpetualEnergy()<span class="pl-k">:</span> <span style="color: #00f">any</span>[];

<span style="color: #00f">export</span> <span style="color: #00f">function</span> endWorldHunger(<span class="pl-v">n</span><span class="pl-k">:</span> <span style="color: #00f">boolean</span>)<span class="pl-k">:</span> <span style="color: #00f">void</span>;</pre>
  </div> Notice we wrote this as a 
  
  <em>module</em> - a file that contains explicit imports and exports. We're intending to import this library through a module loader of some sort, using Node's <code>require()</code> function, AMD's <code>define</code> function, etc. Now, this library might have been written using <a href="https://github.com/umdjs/umd">the UMD pattern</a>, meaning that it could either be imported or used as a global. This is rare in libraries for Node, but common in front-end code where you might use your library by including a <code>&lt;script&gt;</code> tag. So in this example, if <code>my-favorite-library</code> is accessible as the global <code>MyFavoriteLibrary</code>, we can tell TypeScript that with this one-liner: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">export</span> <span style="color: #00f">as</span> <span style="color: #00f">namespace</span> <span class="pl-en">MyFavoriteLibrary</span>;</pre>
  </div> So the body of our declaration file should end up looking like this: 
  
  <div class="highlight highlight-source-ts">
    <pre><span style="color: #148A14">// Our exports:</span>
<span style="color: #00f">export</span> <span style="color: #00f">function</span> getPerpetualEnergy()<span class="pl-k">:</span> <span style="color: #00f">any</span>[];

<span style="color: #00f">export</span> <span style="color: #00f">function</span> endWorldHunger(<span class="pl-v">n</span><span class="pl-k">:</span> <span style="color: #00f">boolean</span>)<span class="pl-k">:</span> <span style="color: #00f">void</span>;

<span style="color: #148A14">// Make this available as a global for non-module code.</span>
<span style="color: #00f">export</span> <span style="color: #00f">as</span> <span style="color: #00f">namespace</span> <span class="pl-en">MyFavoriteLibrary</span>;</pre>
  </div> Finally, we can add tests for this package in 
  
  <code>my-favorite-library/my-favorite-library-tests.ts</code>: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">import</span> <span class="pl-c1">*</span> <span style="color: #00f">as</span> <span class="pl-smi">lib</span> <span style="color: #00f">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>my-favorite-library<span class="pl-pds">"</span></span>;

<span style="color: #00f">const</span> energy <span class="pl-k">=</span> <span class="pl-smi">lib</span>.<span class="pl-en">getPerpetualEnergy</span>()[<span style="color: #09885a">14</span>];

<span class="pl-smi">lib</span>.<span class="pl-en">endWorldHunger</span>(<span style="color: #00f">true</span>);</pre>
  </div> And that's it. We can then commit, push our changes to GitHub... 
  
  <div class="highlight highlight-source-shell">
    <pre>git add ./my-favorite-library
git commit -m <span style="color: #a31515"><span class="pl-pds">"</span>Added declarations for 'my-favorite-library'.<span class="pl-pds">"</span></span>
git push -u origin my-favorite-library</pre>
  </div> ...and send a pull request to the 
  
  <code>master</code> branch on DefinitelyTyped. Once our change is pulled in by a maintainer, it should be automatically published to npm and available. The published version number will depend on the major/minor version numbers you specified in the header comments of <code>index.d.ts</code>. <h2>
    Sending Fixes
  </h2> Sometimes we might find ourselves wanting to update a declaration file as well. For instance, let's say we want to fix up 
  
  <code>getPerpetualEnergy</code> to return an array of <code>boolean</code>s. In that case, the process is pretty similar. We can simply fork & clone DefinitelyTyped as described above, check out the <code>master</code> branch, and create a branch from there. <div class="highlight highlight-source-shell">
    <pre>
git clone https://github.com/YOUR_USERNAME_HERE/DefinitelyTyped
git checkout -b fix-fav-library-return-type</pre>
  </div> Then we can fix up our library's declaration. 
  
  <div class="highlight highlight-source-diff">
    <pre><span style="color: #BD2C00">- export function getPerpetualEnergy(): any[];</span>
<span style="color: #55A532">+ export function getPerpetualEnergy(): boolean[];</span></pre>
  </div> And fix up 
  
  <code>my-favorite-library</code>'s test file to make sure our change can be verified: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">import</span> <span class="pl-c1">*</span> <span style="color: #00f">as</span> <span class="pl-smi">lib</span> <span style="color: #00f">from</span> <span style="color: #a31515"><span class="pl-pds">"</span>my-favorite-library<span class="pl-pds">"</span></span>;

<span style="color: #148A14">// Notice we added a type annotation to 'energy' so TypeScript could check it for us.</span>
<span style="color: #00f">const</span> energy<span class="pl-k">:</span> <span style="color: #00f">boolean</span> <span class="pl-k">=</span> <span class="pl-smi">lib</span>.<span class="pl-en">getPerpetualEnergy</span>()[<span style="color: #09885A">14</span>];

<span class="pl-smi">lib</span>.<span class="pl-en">endWorldHunger</span>(<span style="color: #00f">true</span>);</pre>
  </div>
  
  <h2>
    Dependency Management
  </h2> Many packages in the 
  
  <code>@types</code> repo will end up depending on other type declaration packages. For instance, the declarations for <code>react-dom</code> will import <code>react</code>. By default, writing a declaration file that imports any library in DefinitelyTyped will automatically create a dependency for the <em>latest</em> version of that library. If you want to snap to some version, you can make an explicit <code>package.json</code> for the package you're working in, and fill in the list of dependencies explicitly. For instance, the declarations for leaflet-draw <a href="https://github.com/DefinitelyTyped/DefinitelyTyped/blob/1a5a11e8f1de7c25c3fb254f0d2677ccf2550bf8/leaflet-draw/package.json">depend on the the <code>@types/leaflet</code> package</a>. Similarly, the Twix declarations package <a href="https://github.com/DefinitelyTyped/DefinitelyTyped/blob/1a5a11e8f1de7c25c3fb254f0d2677ccf2550bf8/twix/package.json">has a dependency on <code>moment</code> itself</a> (since Moment 2.14.0 now ships with declaration files). As a note, only the <code>dependencies</code> field <code>package.json</code> is necessary, as the DefinitelyTyped infrastructure will provide the rest. <h2>
    Quicker Scaffolding with dts-gen
  </h2> We realize that for some packages writing out every function in the API an be a pain. Thats why we wrote 
  
  <a href="https://github.com/Microsoft/dts-gen">dts-gen</a>, a neat tool that can quickly scaffold out declaration files fairly quickly. For APIs that are fairly straightforward, dts-gen can get the job done. For instance, if we wanted to create declaration files for the <a href="https://www.npmjs.com/package/array-uniq">array-uniq</a> package, we could use dts-gen intsead of DefinitelyTyped's <code>new-package</code> script. We can try this our by installing dts-gen: <div class="highlight highlight-source-shell">
    <pre>
npm install -g dts-gen</pre>
  </div> and then creating the package in our DefinitelyTyped clone: 
  
  <div class="highlight highlight-source-shell">
    <pre>cd ./DefinitelyTyped
npm install array-uniq

dts-gen -d -m array-uniq</pre>
  </div> The 
  
  <code>-d</code> flag will create a folder structure like DefinitelyTyped's <code>new-package</code> script. You can peek in and see that dts-gen figured out the basic structure on its own: <div class="highlight highlight-source-ts">
    <pre><span style="color: #00f">export</span> <span class="pl-k">=</span> <span class="pl-smi">array_uniq</span>;
<span style="color: #00f">declare</span> <span style="color: #00f">function</span> array_uniq(<span class="pl-v">arr</span><span class="pl-k">:</span> <span style="color: #00f">any</span>)<span class="pl-k">:</span> <span style="color: #00f">any</span>;</pre>
  </div> You can even try this out with something like TypeScript itself! Keep in mind dts-gen doesn't figure out 
  
  <em>everything</em> - for example, it typically substitutes parameter and return values as <code>any</code>, and can't figure out which parameters are optional. It's up to you to make a quality declaration file, but we're hoping dts-gen can help bootstrap that process a little better. dts-gen is still in early experimental stages, but <a href="https://github.com/Microsoft/dts-gen">is on GitHub</a> and we're looking feedback and contributions! <h2>
    A Note About Typings, tsd, and DefinitelyTyped Branches
  </h2> If you're not using tools like 
  
  <a href="https://www.npmjs.com/package/tsd">tsd</a> or <a href="https://www.npmjs.com/package/typings">Typings</a>, you can probably skip this section. If you've sent pull requests to DefinitelyTyped recently, you might have heard about a branch on DefinitelyTyped called <code>types-2.0</code>. The <code>types-2.0</code> branch existed so that infrastructure for <code>@types</code> packages wouldn't interfere with other tools. However, this was a source of confusion for new contributors and so we've merged <code>types-2.0</code> with <code>master</code>. The short story is that all new packages should be sent to the <code>master</code> branch, which now must be structured for for TypeScript 2.0+ libraries. Tools like tsd and Typings will continue to install existing packages that are locked on specific revisions. <h2>
    Next Steps
  </h2> Our team wants to make it easier for our community to use TypeScript and help out on DefinitelyTyped. Currently we have 
  
  <a href="http://www.typescriptlang.org/docs/handbook/declaration-files/publishing.html">our guide on Publishing</a>, but going forward we'd like to cover more of this information on our website proper. We'd also like to hear about resources you'd like to see improved, and information that isn't obvious to you, so feel free to leave your feedback below. Hope to see you on DefinitelyTyped. Happy hacking!
</div>