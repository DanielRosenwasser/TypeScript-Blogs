---
ID: 521
post_title: Announcing TypeScript 1.8 Beta
author: Bowden Kelly
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/announcing-typescript-1-8-beta/
published: true
post_date: 2016-01-28 11:53:00
---
<span>Today we are releasing a beta of TypeScript 1.8. There are quite a lot of changes coming in the 1.8 release, so please check them out and<span class="Apple-converted-space"> </span></span><a href="http://github.com/microsoft/typescript/issues" target="_blank">send us your feedback</a><span>. TypeScript 1.8 beta is available for<span class="Apple-converted-space"> </span></span><a href="http://download.microsoft.com/download/6/D/8/6D8381B0-03C1-4BD2-AE65-30FF0A4C62DA/TS1.8.1-Beta-TS-master.20160126.1/TypeScript_Dev14Full.exe" target="_blank">Visual Studio 2015</a><span>, NuGet (</span><a href="http://blogs.msdn.com/controlpanel/blogs/(https://www.nuget.org/packages/Microsoft.TypeScript.Compiler/" target="_blank">Compiler</a><span><span class="Apple-converted-space"> </span>and<span class="Apple-converted-space"> </span></span><a href="http://blogs.msdn.com/controlpanel/blogs/(https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/" target="_blank">MSBuild task</a><span>),<span class="Apple-converted-space"> </span></span><a href="https://www.npmjs.com/package/typescript" target="_blank">npm</a><span>, and straight from the<span class="Apple-converted-space"> </span></span><a href="https://github.com/Microsoft/TypeScript/releases" target="_blank">source</a><span>.</span>

For npm users, make sure to use the following command:

<span><span style="font-family: courier new,courier">> npm install -g typescript@beta</span><br /></span>

You can also get TypeScript 1.8 beta in VS Code by downloading the above npm package and updating your <span style="font-family: courier new,courier"><code>.vscode/settings.json</code></span> with the following:

<div class="highlight highlight-source-json">
  <pre><span style="font-family: courier new,courier"><span class="pl-s" style="color: #333399"><span class="pl-pds">"</span>typescript.tsdk<span class="pl-pds">"</span></span>: <span class="pl-s" style="color: #993300"><span class="pl-pds">"</span>/some/path/to/node_modules/typescript/lib<span class="pl-pds">"</span></span></span></pre>
</div>

## JavaScript in TypeScript Compilation

In TypeScript 1.8, the TypeScript compiler can now consume JavaScript files alongside TypeScript files. To enable this feature, just add the compiler flag `--allowJs` or add `"allowJs": true` to `compilerOptions` in `tsconfig.json`. This is awesome for several reasons:

### Converting JavaScript projects to TypeScript

At times, the conversion process from JavaScript to TypeScript could be menacing for big projects. While it’s project specific, simply converting all `.js` files to `.ts` files may yield a fairly large number of compilation errors at times, making the conversion a bit unfriendly. However, by allowing JavaScript files into the TypeScript compiler, it is now possible to convert one file at a time while still compiling your entire project all together. This allows developers to be in full control of where and how they want to carry out the conversion making converting sizable projects much more manageable.

### Using 3rd party JavaScript libraries is easier

In order to utilize JavaScript libraries in your TypeScript projects, developers have to include a type definition (.d.ts) file so that the TypeScript compiler knows what you’re doing at compile time and the JavaScript engine knows what to use at runtime. Typically, this is fine unless you want to bundle your external JavaScript libraries with the JavaScript emitted by your TypeScript compilation. Until now, to address this problem, developers needed a third party bundler such as webpack or browserify. With TypeScript 1.8, you can simply include these 3rd party JavaScript libraries in your compilation and they will follow your emitted JavaScript wherever it goes. Source maps included!

### Trivial JavaScript manipulation

With JavaScript in TypeScript compilation, the TypeScript compiler can now emit JavaScript files in all the same ways that TypeScript files can. This includes ES3/ES5, different module types, and bundling. For example, for a quick ES6 to ES5 downlevel conversion using the TypeScript compiler, you just need the following command:

<div class="highlight highlight-source-shell">
  <pre><span class="pl-k">&gt;</span> tsc --allowJs --outDir out target_file.js</pre>
</div>

## Expanded JSX Support

After much [community discussion][1], TypeScript 1.8 brings a few key improvements to enhance the state of art TSX/JSX support.

### Custom JSX Factories

While React has paved the way, JSX syntax is not only tied to React. As such, a JSX factory should be able to be overridden from the default `React` factory. Using the new compiler flag `--reactNamespace <factory_name>` in combination with `--jsx react`, TSX authors can now control the emit of the factory name.

### Stateless functional components

Stateless functional components are a new concept with React 0.14, however they are already seeing very healthy adoption. Starting with TypeScript 1.8, we support stateless functional components in TSX files.

<span style="font-family: courier new,courier"><span class="pl-k" style="color: #993366">interface</span> <span style="color: #333399">GreeterProps</span> {</span>  
<span style="font-family: courier new,courier">    name<span class="pl-k">:</span> string;</span>  
<span style="font-family: courier new,courier">}</span>  
  
<span style="font-family: courier new,courier"><span class="pl-k" style="color: #993366">class </span><span class="pl-en" style="color: #333399">Greeter </span><span class="pl-k" style="color: #993366">extends </span><span class="pl-en" style="color: #333399">React</span>.<span class="pl-smi" style="color: #333399">Component</span><span class="pl-k"><</span><span style="color: #333399">GreeterProps</span>, {}<span class="pl-k">></span> {</span>  
<span style="font-family: courier new,courier"><span class="pl-en" style="color: #333399">    render</span>() {</span>  
<span style="font-family: courier new,courier"><span class="pl-k" style="color: #993366">        return </span><span class="pl-k"><</span>div<span class="pl-k">></span>Hello, {<span class="pl-v" style="color: #ff6600">this</span>.<span class="pl-smi">props</span>.<span class="pl-c1" style="color: #000000">name</span>}<span class="pl-k"><</span><span class="pl-k">/</span>div<span class="pl-k">></span>;</span>  
<span style="font-family: courier new,courier">    }   </span>  
<span style="font-family: courier new,courier">}</span>  
  
<span class="pl-c" style="color: #339966;font-family: courier new,courier">//stateless functional component SimpleGreeter</span>  
<span style="font-family: courier new,courier"><span class="pl-k" style="color: #993366">let </span><span class="pl-en" style="color: #333399"><span style="color: #000000">SimpleGreeter <span class="pl-k">=</span> ({<span class="pl-smi">name</span> <span class="pl-k">=</span> <span class="pl-s" style="color: #000080"><span class="pl-pds">'</span>world<span class="pl-pds">'</span></span>}) <span class="pl-k">=></span> <span class="pl-k"><</span><span style="color: #800080">div</span><span class="pl-k">></span>Hello, {name}<span class="pl-k"><</span><span class="pl-k">/</span><span style="color: #800080">div</span><span class="pl-k">></span>;</span></span></span>

### Syntax highlighting in VS 2015

With the TypeScript 1.8 update in Visual Studio, JSX tags will now get classified and colorized improving the readability of the code.

*[<img src="https://devblogs.microsoft.com/00/00/01/56/67/8546.jsxSample.png" alt="" border="0" />][2]*

## TypeScript NuGet Packges

A long-standing request from some of our users has been to provide an official TypeScript NuGet package to improve the ease of acquisition. Starting with TypeScript 1.8 beta, we will be distributing the TypeScript compiler and the MSBuild task as a NuGet package with every release! Links to the respective packages are below:

[TypeScript compiler][3]

[TypeScript MSBuild task][4]

In addition to these official releases, we will also be hosting the nightly build of TypeScript on MyGet. These are not supported builds, but feel free to check them out and let us know what you think. You can find the builds [here][5].

## Chakra Core + TypeScript

We are always looking for ways to improve the performance of the TypeScript compiler, so when [Chakra Core][6] was released a few weeks ago, we jumped at the opportunity to test it out. Below is a chart of the compilation times for each engine over different size repositories. We found ChakraCore achieved significantly reduced compilation times, outperforming the previous version of Chakra that we were using by 5-20% and Node by 10-50%! Based on these improvements, we have switched to using ChakraCore when possible.

*[<img src="https://devblogs.microsoft.com/00/00/01/56/67/1055.chakraCorePerf.png" alt="" border="0" />][7]*

## And More!

TypeScript 1.8 includes several more features like F-Bounded polymorphism, string literal types, etc. If you want to deep dive on these and other features coming in 1.8, check out our [what’s new page][8]. As always, we encourage you to weigh in on the future of TypeScript by browsing our [existing issues][9], throwing us a [pull request][10], or just hanging out with the team on [gitter][11].

 [1]: https://github.com/Microsoft/TypeScript/issues/3788
 [2]: https://devblogs.microsoft.com/00/00/01/56/67/8546.jsxSample.png
 [3]: https://www.nuget.org/packages/Microsoft.TypeScript.Compiler/
 [4]: https://www.nuget.org/packages/Microsoft.TypeScript.MSBuild/
 [5]: https://www.myget.org/users/typescriptteam
 [6]: https://blogs.windows.com/msedgedev/2016/01/13/chakracore-now-open/
 [7]: https://devblogs.microsoft.com/00/00/01/56/67/1055.chakraCorePerf.png
 [8]: https://github.com/Microsoft/TypeScript/wiki/What%27s-new-in-TypeScript#typescript-18-upcoming
 [9]: https://github.com/Microsoft/TypeScript/issues
 [10]: https://github.com/Microsoft/TypeScript/pulls
 [11]: https://gitter.im/microsoft/typescript