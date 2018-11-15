---
ID: 1826
post_title: TypeScript and Babel 7
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/typescript-and-babel-7/
published: true
post_date: 2018-08-27 22:56:24
---
<div style="font-size: 16px">
  Today we're excited to announce something special for Babel users. Over a year ago, we set out to find what the biggest difficulties users were running into with TypeScript, and we found that a common theme among Babel users was that trying to get TypeScript set up was just too hard. The reasons often varied, but for a lot of developers, rewiring a build that's already working can be a daunting task. Babel is a fantastic tool with a vibrant ecosystem that serves millions of developers by transforming the latest JavaScript features to older runtimes and browsers; but it doesn't do type-checking, which our team believes can bring that experience to another level. While TypeScript itself can do both, we wanted to make it easier to get that experience without forcing users to switch from Babel. That's why over the past year we've collaborated with the Babel team, and today we're happy to jointly announce that Babel 7 now ships with TypeScript support! <h2>
    How do I use it?
  </h2> If you're already using Babel and you've never tried TypeScript, now's your chance because it's easier than ever. At a minimum, you'll need to install the TypeScript plugin. 
  
  <div class="highlight highlight-source-shell">
    <pre>npm install --save-dev @babel/preset-typescript</pre>
  </div> Though you'll also probably want to get the other ECMAScript features that TypeScript supports: 
  
  <div class="highlight highlight-source-shell">
    <pre>npm install --save-dev @babel/preset-typescript @babel/preset-env @babel/plugin-proposal-class-properties @babel/plugin-proposal-object-rest-spread</pre>
  </div> Make sure your 
  
  <code style="color: #a31515">.babelrc</code> has the right presets and plugins: <div class="highlight highlight-source-js">
    <pre>{
    <span style="color: #a31515"><span class="pl-pds">"</span>presets<span class="pl-pds">"</span></span><span class="pl-k">:</span> [
        <span style="color: #a31515"><span class="pl-pds">"</span>@babel/env<span class="pl-pds">"</span></span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>@babel/preset-typescript<span class="pl-pds">"</span></span>
    ],
    <span style="color: #a31515"><span class="pl-pds">"</span>plugins<span class="pl-pds">"</span></span><span class="pl-k">:</span> [
        <span style="color: #a31515"><span class="pl-pds">"</span>@babel/proposal-class-properties<span class="pl-pds">"</span></span>,
        <span style="color: #a31515"><span class="pl-pds">"</span>@babel/proposal-object-rest-spread<span class="pl-pds">"</span></span>
    ]
}</pre>
  </div> For a simple build with 
  
  <code style="color: #a31515">@babel/cli</code>, all you need to do is run <div class="highlight highlight-source-shell">
    <pre>babel ./src --out-dir lib --extensions <span style="color: #a31515"><span class="pl-pds">"</span>.ts,.tsx<span class="pl-pds">"</span></span></pre>
  </div> Your files should now be built and generated in the 
  
  <code style="color: #a31515">lib</code> directory. To add type-checking with TypeScript, create a <code style="color: #a31515">tsconfig.json</code> file <div class="highlight highlight-source-js">
    <pre>{
  <span style="color: #a31515"><span class="pl-pds">"</span>compilerOptions<span class="pl-pds">"</span></span><span class="pl-k">:</span> {
    <span style="color: #148A14">// Target latest version of ECMAScript.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>target<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>esnext<span class="pl-pds">"</span></span>,
    <span style="color: #148A14">// Search under node_modules for non-relative imports.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>moduleResolution<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #a31515"><span class="pl-pds">"</span>node<span class="pl-pds">"</span></span>,
    <span style="color: #148A14">// Process & infer types from .js files.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>allowJs<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>,
    <span style="color: #148A14">// Don't emit; allow Babel to transform files.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>noEmit<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>,
    <span style="color: #148A14">// Enable strictest settings like strictNullChecks & noImplicitAny.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>strict<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>,
    <span style="color: #148A14">// Disallow features that require cross-file information for emit.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>isolatedModules<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>,
    <span style="color: #148A14">// Import non-ES modules as default imports.</span>
    <span style="color: #a31515"><span class="pl-pds">"</span>esModuleInterop<span class="pl-pds">"</span></span><span class="pl-k">:</span> <span style="color: #0000ff">true</span>
  },
  <span style="color: #a31515"><span class="pl-pds">"</span>include<span class="pl-pds">"</span></span><span class="pl-k">:</span> [
    <span style="color: #a31515"><span class="pl-pds">"</span>src<span class="pl-pds">"</span></span>
  ]
}</pre>
  </div> and just run 
  
  <div class="highlight highlight-source-shell">
    <pre>tsc</pre>
  </div> and that's it! 
  
  <code style="color: #a31515">tsc</code> will type-check your <code style="color: #a31515">.ts</code> and <code style="color: #a31515">.tsx</code> files. Feel free to add the <code style="color: #a31515">--watch</code> flag to either tool to get immediate feedback when anything changes. You can see how to <a href="https://github.com/Microsoft/TypeScript-Babel-Starter">set up a more complex build on this sample repository</a> which integrates with tools like Webpack. You can also just <a href="https://babeljs.io/repl/#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=EQVwzgpgBGAuBOBLAxrYBuAUJxA7WE8AZgIbLQDCi8yANtAN6ZRTwkAmi4AXFLiAFsARoSwBfbERC5UiAPa4oJeBBIAKZLyo16ASihMWK2CHiKAsiVgALAHQAFAJJQAVFGS22ncK7cAmcSA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=true&fileSize=true&timeTravel=false&sourceType=module&lineWrap=true&presets=typescript%2Cenv&prettier=false&targets=&version=7.0.0-beta.42&envVersion=7.0.0-beta.42">play around with the TypeScript preset on Babel's online REPL</a>. <h2>
    What does this mean for me?
  </h2> Using the TypeScript compiler is still the preferred way to build TypeScript. While Babel can take over compiling/transpiling - doing things like erasing your types and rewriting the newest ECMAScript features to work in older runtimes - it doesn't have type-checking built in, and still requires using TypeScript to accomplish that. So even if Babel builds successfully, you might need to check in with TypeScript to catch type errors. For that reason, we feel 
  
  <code style="color: #a31515">tsc</code> and the tools around the compiler pipeline will still give the most integrated and consistent experience for most projects. So if you're already using TypeScript, maybe this doesn't change much for you. But if you're already using Babel, or interested in the Babel ecosystem, and you want to get the benefits of TypeScript like catching typos, error checking, and the editing experiences you might've seen in the likes of Visual Studio and Visual Studio Code, this is for you! <h2>
    Caveats
  </h2> As we mentioned above, the first thing users should be aware of is that Babel won't perform type-checking on TypeScript code; it will 
  
  <em>only</em> be transforming your code, and it will compile regardless of whether type errors are present. While that means Babel is free from doing things like reading <code style="color: #a31515">.d.ts</code> files and ensuring your types are compatible, presumably you'll want <em>some</em> tool to do that, and so you'll still need TypeScript. This can be done as a separate <code style="color: #a31515">tsc --watch</code> task in the background, or it can be part of a lint/CI step in your build. Luckily, <a href="https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support">with the right editor support</a>, you'll be able to spot most errors before you even save. Second, there are certain constructs that don't currently compile in Babel 7. Specifically, <ul>
    <li>
      namespaces
    </li>
    <li>
      bracket style type-assertion/cast syntax regardless of when JSX is enabled (i.e. writing <code style="color: #a31515">&lt;Foo&gt;x</code> won't work even in <code style="color: #a31515">.ts</code> files if JSX support is turned on, but you can instead write <code style="color: #a31515">x as Foo</code>).
    </li>
    <li>
      enums that span multiple declarations (i.e. enum merging)
    </li>
    <li>
      legacy-style import/export syntax (i.e. <code style="color: #a31515">import foo = require(...)</code> and <code style="color: #a31515">export = foo</code>)
    </li>
  </ul> These omissions are largely based technical constraints in Babel's single-file emit architecture. We believe that most users will find this experience to be totally acceptable. To make sure that TypeScript can call out some of these omissions, you should ensure that TypeScript uses the 
  
  <code style="color: #a31515">--isolatedModules</code> flag. <h2>
    What next?
  </h2> You can 
  
  <a href="https://babeljs.io/blog/2018/08/27/7.0.0">read up on the details from the Babel side on their release blog post</a>. We're happy that we've had the chance to collaborate with folks on the Babel team like Henry Zhu, Andrew Levine, Logan Smyth, Daniel Tschinder, James Henry, Diogo Franco, Ivan Babak, Nicolò Ribaudo, Brian Ng, and Vladimir Kurchatkin. We even had the opportunity to <a href="https://github.com/babel/babylon/pull/459#issuecomment-296170332">speed up Babylon, Babel's parser</a>, and helped align with James Henry's work on <a href="https://github.com/eslint/typescript-eslint-parser">typescript-eslint-parser</a> which now powers Prettier's TypeScript support. If we missed you, we're sorry but we're grateful and we appreciate all the help people collectively put in! Our team will be contributing to future updates in the TypeScript plugin, and we look forward to bringing a great experience to all TypeScript users. Going forward, we'd love to hear your feedback about this new TypeScript support in Babel, and how we can make it even easier to use. Give us a shout on Twitter at <a href="https://twitter.com/typescriptlang" rel="nofollow">@typescriptlang</a> or in the comments below. Happy hacking!
</div>