---
ID: 401
post_title: >
  Developing in TypeScript on a Mac with
  Sublime
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/developing-in-typescript-on-a-mac-with-sublime/
published: true
post_date: 2015-06-05 10:00:00
---
<span>Within the TypeScript team, many of us have Macs that we use for development. We’ve also heard from some of you that you’d like to use your OS X machines to build TypeScript projects. So recently, we’ve been focusing on building a natural and rich developer toolset for Mac and Linux fans.</span> Starting with TypeScript 1.5, TypeScript projects have complete tooling support in OS X using [Sublime][1], [Atom][2], and [Visual Studio Code][3]. <span>In this article, we’ll be talking about how to set up your Mac for developing TypeScript using Sublime.</span> 
# <span>Getting started</span>

<p dir="ltr">
  <span>First, install the TypeScript compiler, which you can get via </span><a href="https://www.npmjs.com/package/typescript"><span>npm</span></a><span>.  You can install this once you have node by installing the 'typescript' package.  Since this installs globally, we run the command using 'sudo':</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier">> sudo npm install -g typescript</span>
</p>

<p dir="ltr">
  <span>Next, we'll add the TypeScript support for Sublime.  The TypeScript package is available via </span><a href="https://packagecontrol.io"><span>Package Control</span></a><span>.  Once you have Package Control installed, you can search for the TypeScript package.  First, bring up the command palette with Cmd+Shift+P.</span>
</p>

<p dir="ltr">
  <span><img src="https://lh4.googleusercontent.com/qlnD2zHfVxWhqY9fJj44UPqHPmznPGB8X9wqLqt0N4IxvDvtyoi1ezdsSxrBooRjZjQWnI_w_wgEmSjo-2aNLBCPLlMQ4m0yQdfqj8gbNaT1VCCIn4AYAffC5iAwJmizC_uwJPo" alt="Screen Shot 2015-06-02 at 2.00.28 PM.png" width="624px;" height="480px;" /></span> Next, select Package Control: Install Package and search for ‘typescript’.  You’ll see there are a few packages that match ‘typescript’, but the one we want is simply ‘TypeScript’. <img src="https://lh5.googleusercontent.com/wbpPWIeUfQOCCzyclqq1qHDAjq6ePBpMJ6RMTTGqA9aG8D5UA10MSZs3vLu0ZLrsM3xwVQ3Se-WB4fqZeQZPM5IEc6Tly_Sfspb4IBNQbFmH6G4JDn_rQSx-kBfsdIoXJwnWvEs" alt="Screen Shot 2015-06-02 at 2.00.37 PM.png" width="624px;" height="389px;" /> After you’ve installed the ‘TypeScript’ package, you’ll be able to begin working with TypeScript projects.  You can do a quick test to make sure everything is working by starting up a new TypeScript (*.ts) file.  Let’s make a hello.ts: <span style="font-family: 'courier new', courier;color: #339966">// hello.ts</span><br /><span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> greet(msg: <span style="color: #0000ff">string</span>) {</span><br /><span style="font-family: 'courier new', courier">  console.log(msg);</span><br /><span style="font-family: 'courier new', courier">}<br /><br />greet(42);</span> Notice that Sublime will help you as you type in the example, giving auto-complete help as you dot into console and argument help as you begin the call into log. <img src="https://lh5.googleusercontent.com/6D7KOUwlk15PCBICi-FYfmfdkuf2pIHfZoum8CoWohXOvGtEF6-Pt7LiHON8gSJvBN9dDPk6tYdlvjiZUlAtddy0yl9_qdHkp-TpY9pxS72nUO_E3ltCEQHvXj_7IynUKRZDGH0" alt="Screen Shot 2015-06-03 at 9.10.18 AM.png" width="624px;" height="157px;" /> TypeScript also warns us against passing a number when we intended to pass a string by squiggling under the number and changing the status at the bottom of the editor when we click on the squiggled text.  This is pretty helpful for finding simple bugs before we deploy our project. <img src="https://lh6.googleusercontent.com/y80gGEM-XHv2LbDOFuKZ_cfn8M2hKuIMmn_p0Y3eCY6Xrv8mETJXosMjMx2CiP9RLMZxPPubUs6TA1rHU_inKsauu9f7I262FgThwSmMfLu1JjSEI33RhoZeuH2_yJpxhXCZo88" alt="Screen Shot 2015-06-03 at 9.10.43 AM.png" width="624px;" height="171px;" /> Now that we’re set up Sublime to work with TypeScript, the next thing we want to do is to create a TypeScript project.  Starting with 1.5, TypeScript supports a lightweight project file called ‘tsconfig.json’.  We can write a tsconfig.json for our hello app: <img src="https://lh4.googleusercontent.com/D-l4OKALXpKa-NV7R7ic749PpkA7rMsg259LD2O-yPqknrO3il9NmyxSuNkNZziXmUom5BR7P3aQhsx8dnH323frTKtpfnG46aTe41AJ8lc_6UJeHmCDn2FtzFigebGEQmyAVgI" alt="Screen Shot 2015-06-03 at 9.18.03 AM.png" width="624px;" height="255px;" />
</p>

<p dir="ltr">
  <span>The ‘tsconfig.json’ file has two main parts: the options passed to the compiler and the list of files.  In the example, I switch on source maps, so the TypeScript compiler will generate a .map file we can use to directly debug the TypeScript rather than the generated JavaScript.  I also list the files explicitly to show how you can control which files are included in the build.  For this example, this step is optional.  If you leave off the "files" section, ‘tsconfig.json’ will compile all *.ts files in the same directory.</span> We have all we need to build the app now.  We can open a terminal, cd to our project directory, and run the compiler with the ‘tsc’ command. <img src="https://lh4.googleusercontent.com/n2s2wbyWS6JjYVZTF7W5-98t9FsQzon0DP8Q573Frm6XucPdzVAxjsKPqPnwN57W_7pi9lHBR63awr-yZnOS6uCiUOPgoGZVadIf7RFAbkehr-aYen-CRL0VEvRZV_lN8LLupLQ" alt="Screen Shot 2015-06-03 at 9.30.40 AM.png" width="624px;" height="69px;" /> Oh right!  We forgot to fix the issue in ‘hello.ts’.  Let’s update hello.ts to pass in a string instead: <span style="font-family: 'courier new', courier;color: #339966">// revised hello.ts</span>
</p>

<p dir="ltr">
  <span style="font-family: 'courier new', courier"><span style="color: #0000ff">function</span> greet(msg: <span style="color: #0000ff">string</span>) {<br /></span><span style="font-family: 'courier new', courier"> console.log(msg);<br /></span><span style="font-family: 'courier new', courier">}</span> <span style="font-family: 'courier new', courier">greet(<span style="color: #ff0000">"Greetings, OS X"</span>);</span> Now, when we re-run the build, the error goes away.  We can also run our file using node. <img src="https://lh6.googleusercontent.com/HTkta5hugBC-U2T8c3zla8Q7QvWiR24VMEQgY7zP_1QKXrJTOPtobe1JkbRDaU4ZGSOh4b_TmXlh5fGsr3i9zFqs9K6m6QqXG-TLFhaDXhaZjgO5WNim_i3dtqebVv1J-RqOd9k" alt="Screen Shot 2015-06-03 at 9.33.38 AM.png" width="624px;" height="113px;" />
</p>

# <span>Debugging our app</span> If we look at our project directory, we’ll see the output JavaScript as well as the source map file we enabled earlier: 

<img src="https://lh6.googleusercontent.com/hjn_3NjGDS9tfo9IaxllPXttU5-qORFBulWMvF0u7XiU3TpTlatqh2yLwEwaELT41jIteHPIiYEurMgfZj2W1ujN20yIleceHgMSQMJpiplNP7w2XOxmx8HPhwSHdUwtymNbYAA" alt="Screen Shot 2015-06-03 at 9.37.20 AM.png" width="624px;" height="64px;" /> Let’s use the source map file so we can debug our TypeScript source.  To do so, we’ll create a small html page that will call our hello.js: <img src="https://lh5.googleusercontent.com/1JUyTTW0rYZGqsrI7O34Jt08sUkb1GfvMBcSi3AhLKJsXg-d7DuKeDD6nNG6gS4ObyF6uyOmT5XqWh_zsy8IoVqGQeDH3A3tkCb6NDQJuPmFOR654nKK-D4kpq-s-ZMIrzBq5Iw" alt="Screen Shot 2015-06-03 at 9.37.01 AM.png" width="624px;" height="248px;" /> Next, we’ll open up Chrome and our index.html.  Opening the Developer Tools in Chrome via Option+Cmd+I or through the "More tools" menu, we can see the same output we saw running the app in the terminal earlier: <img src="https://lh5.googleusercontent.com/q-RBGGEGhJGX-qkvTsNCkC5lGaopsGuYe3BrNq0RolbSAI7Xm3I0agKLMZN7ezKncwhEvJTxPYLje7iynuz_sQ3PXdTjkFeFGmPdH28DsajqALQvZsO1yuPno88h7m1dNYcJEKU" alt="Screen Shot 2015-06-03 at 9.40.53 AM.png" width="624px;" height="103px;" /> We can also work with the TypeScript we created by clicking on the "hello.ts:2" link to the right or switching to the Sources tab.  Notice how Chrome uses the source map file to map from the hello.js file back to the original hello.ts. <img src="https://lh5.googleusercontent.com/_mJrdjfQ-2ccVOmVkpQF1vHxSMW7gIm8rRC1Z_mQLQI4BG8OrMRA_kXv8BReVbQ0Aszs2eaqZiLjdySx9RAcBCFRGRCBQYcXdRsUB0WB_4-AkqdCviKBjUsolTvsp2fm_GtHJqw" alt="Screen Shot 2015-06-03 at 9.42.03 AM.png" width="624px;" height="165px;" /> 
# What's next

<span style="font-size: 12px">We’re just getting warmed up.  There are a lot of resources to help you get going.  You can browse the </span><a style="font-size: 12px" href="http://www.typescriptlang.org/Handbook">TypeScript handbook</a><span style="font-size: 12px"> to learn TypeScript, ask the team and the TypeScript community questions via </span><a style="font-size: 12px" href="http://stackoverflow.com/questions/tagged/typescript">StackOverflow</a><span style="font-size: 12px"> and the </span><a style="font-size: 12px" href="https://github.com/Microsoft/TypeScript">TypeScript GitHub</a><span style="font-size: 12px"> site, and start working with a variety of JavaScript libraries by adding their </span><a style="font-size: 12px" href="https://github.com/borisyankov/DefinitelyTyped">type information</a><span style="font-size: 12px"> to your project.  If you’re using TypeScript on a Mac and have some suggestions for how we can make the experience better, we’d </span><a style="font-size: 12px" href="https://github.com/Microsoft/TypeScript/issues">love to hear them</a><span style="font-size: 12px">!</span> 
<span><br /></span>

 [1]: https://github.com/Microsoft/TypeScript-Sublime-Plugin
 [2]: https://atom.io/packages/atom-typescript
 [3]: https://code.visualstudio.com/