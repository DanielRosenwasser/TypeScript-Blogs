---
ID: 1674
post_title: Introducing TypeScript npm nightlies
author: 'Jonathan Turner [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/introducing-typescript-npm-nightlies-2/
published: true
post_date: 2015-07-27 09:10:00
---
To make it easier to try out the very latest in TypeScript, we're now publishing the TypeScript nightly npm package.  You can try this out using:

<p style="padding-left: 30px">
  <span style="font-family: 'courier new', courier">npm install -g typescript@next</span>
</p>

We've set this to update each night, and we timestamp each release.  You can see what version you're on with the --version commandline switch:

<p style="padding-left: 30px">
  <span style="font-family: 'courier new', courier">C:\Users>tsc --version</span><br /><span style="font-family: 'courier new', courier">message TS6029: Version 1.6.0-dev.20150727</span>
</p>

**Please note:** since this is the coming straight from the bleeding edge source, it may have bugs or incompatibilities.  If you find any, please let us know [the usual way][1].

 [1]: https://github.com/microsoft/typescript/issues