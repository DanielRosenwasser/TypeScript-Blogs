---
ID: 1676
post_title: The Future of Declaration Files
author: Daniel Rosenwasser
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/the-future-of-declaration-files-2/
published: true
post_date: 2016-06-15 16:03:28
---
Declaration files (`.d.ts` files) are a fundamental part of using existing JavaScript libraries in TypeScript, but getting them has always been a place where we've known there was room for improvement. As we get closer to TypeScript 2.0, we're very excited to show off a sneak peak of our plan to simplify things. Getting type declarations in TypeScript 2.0 will require **no tools apart from npm**. As an example, getting the declarations for a library like lodash will be just an npm command away: <div>
  <pre>npm install --save @types/lodash
</pre>
</div> From there you'll be able to use lodash in your TypeScript code with no fuss. This works for both modules and global code. For example, once you've 

`npm install`-ed your type declarations, then you can use imports and write <div>
  <pre><span style="color: #0000ff">import</span> * <span style="color: #0000ff">as</span> _ <span style="color: #0000ff">from</span> <span style="color: #a31515">"lodash"</span>;
_.padStart(<span style="color: #a31515">"Hello TypeScript!"</span>, <span style="color: #09885a">20</span>, <span style="color: #a31515">" "</span>);
</pre>
</div> or if you're not using modules, you can just use the global variable 

`_` if you have a `tsconfig.json` around. <div>
  <pre>_.padStart(<span style="color: #a31515">"Hello TypeScript!"</span>, <span style="color: #09885a">20</span>, <span style="color: #a31515">" "</span>);
</pre>
</div> Looking for more than just lodash? Well, we're also making it extremely easy to figure out which packages have the type declarations you need. We now have a type search at 

<https://aka.ms/types> where you can find the package for your favorite library. Best of all, this all works today with our nightly builds. Just run `npm install -g typescript@next`, and give it a shot. This is all you need to know to start experiencing the future of type acquisition. We'd love to hear your thoughts, so if you'd like to give us your feedback or just understand the specifics, [head on over to GitHub][1] to read more or leave a comment. 
### Notes and Acknowledgements For those wondering, DefinitelyTyped will still be the place to author new declaration files. We'll soon be providing a greatly expanded set of documentation aimed at 

`.d.ts` authors in the near future. Tools like Typings and tsd will continue to work, and we'll be working alongside those communities to ensure a smooth transition. We also owe a great thanks to those who helped guide us in this direction. Specifically, [Blake Embrey][2], the maintainer and creator of Typings, has worked closely with us during this entire process and given us valuable feedback. In addition to this, [Boris Yankov][3], founder of DefinitelyTyped, and [Diullei Gomes][4] and [Bart van der Schoor][5], maintainers of tsd, have helped lay the foundation of these efforts. Lastly, we'll mention that this is still a work in progress. Editor support is currently limited to Visual Studio Code and our Sublime plugins, with Visual Studio support on the way. We absolutely value all the feedback we can get, so try out our nightly builds and let us know what the new experience is like!

 [1]: https://github.com/Microsoft/TypeScript/issues/9184
 [2]: https://github.com/blakeembrey
 [3]: https://github.com/borisyankov
 [4]: https://github.com/Diullei
 [5]: https://github.com/Bartvds