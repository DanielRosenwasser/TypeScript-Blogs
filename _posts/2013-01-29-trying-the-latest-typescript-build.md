---
ID: 243
post_title: Trying the Latest TypeScript Build
author: Ryan Cavanaugh
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/trying-the-latest-typescript-build/
published: true
post_date: 2013-01-29 08:52:00
---
Starting with release 0.8.2.0, you can now update the TypeScript language services file, the compiled JavaScript that powers most of the TypeScript editor features in Visual Studio. This includes tasks like error reporting, compile-on-save, renaming, go to definition, completion lists, signature help, and others.

By updating your TypeScript language services file, you can try out the latest features we're working on before they appear in an official release. Naturally, you might encounter bugs here as the code built in the development branches isn't as stable or well-tested as the released version.

The majority of the TypeScript editor features are found in a file called <tt><span class="code">typescriptSerivces.js</span></tt>. To update your file, you'll need to get a new copy and overwrite the existing copy.

<a name="Topic_5394"></a>

## Obtaining a new <span class="italic">'typescriptServices.js'</span> file

<a name="Section_5395"></a>

### Option 1: Use the LKG

The LKG ("Last Known Good") version of the compiler is updated somewhat frequently. You can get a copy from CodePlex using the [source browser][1] and downloading the <tt><span class="code">typescriptServices.js</span></tt> file from the <tt><span class="code">bin</span></tt> directory.

<a name="Section_5396"></a>

### Option 2: Build Locally

<a name="Subsection_5397"></a>

#### Install git and Node.js

You'll need some basic tools first

*   [Install git][2]. Some recommendations for the install options:

*   Don't install the Windows Explorer integration unless you plan to use it
*   Select the <span class="italic">Run Git from the Windows Command Prompt</span> option on the following page
*   Select <span class="italic">Checkout as-is, commit as-is</span> option on the following page to minimize any line-ending weirdness

*   [Install Node.js][3]
<a name="Subsection_5398"></a>

#### Setup a Local Repository

<pre><span class="code"> &gt; git clone https://git01.codeplex.com/typescript </span></pre>

This will set up a TypeScript repository in a subfolder of the current folder called 'typescript'.

<a name="Subsection_5399"></a>

#### Select a Branch

<pre><span class="code"> &gt; cd typescript &gt; git checkout develop </span></pre>

If you're interested in the latest, the <tt><span class="code">develop</span></tt> branch is probably your best bet. There may be other branches with active development; if you know of a branch you'd like to try out instead, replace <tt><span class="code">develop</span></tt> with that branch name.

<a name="Subsection_5400"></a>

#### Get the Development Dependencies

<pre><span class="code"> &gt; npm install </span></pre>

This will install a local copy of [<tt><span class="code">jake</span></tt>][4], the build system we use for TypeScript.

<a name="Subsection_5401"></a>

#### Build TypeScript

<pre><span class="code"> &gt; .\node_modules\.bin\jake local </span></pre>

This will produce <tt><span class="code">built\local\typescriptServices.js</span></tt>. Copy that file as indicated below.

<a name="Subsection_5402"></a>

#### Iterate

If you like, you can simply delete the TypeScript folder created in the first step and be done. However, if you come back at a later time and want a new fresh copy (with the latest changes), you'll need to run <tt><span class="code">git pull</span></tt> to get the newest sources. After that, just run <tt><span class="code">jake local</span></tt> and copy over the <tt><span class="code">typeScriptServices.js</span></tt> file again.

<a name="Topic_5403"></a>

## Updating TypescriptServices.js

You'll need to locate the folder where the TypeScript extension was installed. Look for a file called <tt><span class="code">typescriptServices.js</span></tt> in a folder like:

<tt><span class="code">C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\Extensions\<span class="highlight">enn4wcm5.z2b</span>\</span></tt>

The highlighted portion at the end there will be different on every machine, but there shouldn't be too many of them in the <tt><span class="code">Extensions</span></tt> folder. In some rare cases, you might have more than one subfolder that has a TypeScript installation in it; if this is the case, you'll want to use the newest one (check the Created date).

When you find the one with <tt><span class="code">typescriptServices.js</span></tt>, backup that file and copy in the new version. After you restart Visual Studio, you'll be working with the updated services file.

 [1]: http://typescript.codeplex.com/SourceControl/BrowseLatest?branch=develop
 [2]: http://git-scm.com/downloads
 [3]: http://nodejs.org/
 [4]: https://github.com/mde/jake