---
ID: 374
post_title: Using TypeScript in Visual Studio Code
author: 'Luke Hoban [MS]'
post_excerpt: ""
layout: post
permalink: >
  https://devblogs.microsoft.com/typescript/using-typescript-in-visual-studio-code/
published: true
post_date: 2015-04-30 14:43:00
---
<p dir="ltr">
  With the recent announcement of <a href="https://code.visualstudio.com/"><span>Visual Studio Code</span></a>, there have been a lot of questions about how to get started writing TypeScript.  In this quickstart, we’ll create a simple TypeScript project.  Out of the box, Visual Studio Code supports <a href="http://blogs.msdn.com/b/typescript/archive/2015/04/30/announcing-typescript-1-5-beta.aspx">TypeScript 1.5 beta</a> and using either the node or Visual Studio command-line compilers.  For this quickstart, because of its use of the new tsconfig.json feature, we assume you already have TypeScript 1.5 beta installed.  
</p>

<p dir="ltr">
  <span>Let’s start by making a new empty folder, MyProject, and opening it in Visual Studio Code. In Window, click “Open Folder”; on OS X, click “Open” and select a folder:</span>
</p>

<p dir="ltr">
  <span><span id="docs-internal-guid-1a74ed6c-0c4a-f15c-2ebf-4098525e2c84"><br /><span><img src="https://lh6.googleusercontent.com/iUpfW-u7JsJXdnpctba8Hrw3kzGUgnJIoFcvVtlcBENYI_7iVlsfMc8UTpplkZN8KyfAi0wDHnQanlgvGh6V2-5PSAKQRZJNYRPbVLJ6JA54fL17T21qEVJuJ7vLuL0c2bo37UQ" alt="openfolder.png" width="250px;" height="400px;" /></span></span></span>
</p>

<p dir="ltr">
  <span>Create a new file by clicking “New FIle” in the MyProject row of the Explore Sidebar:</span>
</p>

<p dir="ltr">
  <span><img src="https://lh3.googleusercontent.com/_sIK8rcqB2xKOl1K2ItBBOzCL0dwtmZR7534LTDLVGt7X-SEm7Tf7wQnqurK168MTSzc717dMAtMwPqy9smhJJgtogtJbCwlEOcrQlSwb5-c1-Lyys-pff5JMpcVRwzQ1KJLsqA" alt="newfile.png" width="256px;" height="256px;" /></span>
</p>

<p dir="ltr">
  <span>Enter “tsconfig.json” as the filename</span>
</p>

<p dir="ltr">
  <span>Type { } in the file and hit Ctrl-Space between the braces to trigger completion:</span>
</p>

<p dir="ltr">
  <span><img src="https://lh4.googleusercontent.com/9QmuScAZrA38qGQygW0gxR2pPmCUZUzC_TFMuf71ki2jEWQDM_CH0iihfqjYSk2oZQmtZDIEbMNI_rAa8c3l0Xu4zl9trnE60oy43iiIcWzfKOajZcW6evmy-DKcU__i9MRb6u4" alt="tsconfigautocomplete.png" width="624px;" height="219px;" /></span>
</p>

<p dir="ltr">
  <span>We can configure the project for ES5, AMD modules, and source maps:</span>
</p>

<p dir="ltr">
  <span><img src="https://lh3.googleusercontent.com/pIJ-CLKXI6TW5CAfyNl53b0vhRc1-7bOF7YAWy45taSigBMhaac9hg1l-HIkeV7Y4D-1a5u78UDE-d0wRoKBzENbI8kS8CGZfu0iKDAiGLDzlJwUHr_QM1m2Ie6DTFUQG8el-Ws" alt="tsconfigcomplete.png" width="624px;" height="237px;" /></span>
</p>

<p dir="ltr">
  <span>Using “New File” again, add a TypeScript file and try writing some code in it:</span>
</p>

<p dir="ltr">
  <span id="docs-internal-guid-1a74ed6c-0c58-24b6-73db-8ebe94698372"><span><img src="https://lh4.googleusercontent.com/n-b45n_pD0NRr1oZ_m82kR9kNfOm2clIwF6nX-C3a_pZd6U4WpzGMsn-ELz57eliDSQDWvTMkkH28tdwYT0er3uROVM0fKvY2om7TPNtjeXp1-VY0zPTzW5nC3IFQY2Jc8QVlKw" alt="withletfile.png" width="624px;" height="204px;" /></span></span>
</p>

<p dir="ltr">
  <span>Before anything else, let’s try compiling with the Ctrl-Shift-B shortcut key. A message appears telling us that we don’t have a task runner enabled for this project yet. Click “Configure Task Runner” to set up a config file:</span>
</p>

<p dir="ltr">
  <span><img src="https://lh4.googleusercontent.com/rWQ05LsYciUN1wO0ZE-fvgqFsZMIxDtlW5K_QzcHYx52Cwz_OJ7JnwrvXxp52wo9kjHNHR_ppSohCY-NyylGAuv8HE23Umdqv1lOve3yHy9spsKucEwByOsDGkVNY0heql8FYH0" alt="notaskrunner2.png" width="624px;" height="76px;" /></span>
</p>

<p dir="ltr">
  <span>Remove “HelloWorld.ts” from the “args” array, and add a property called “isShellCommand” with the value ‘true’ in the config file:</span>
</p>

<p dir="ltr">
  <span id="docs-internal-guid-1a74ed6c-0c4f-282d-f272-3ac96c7b1193"><span><img src="https://lh4.googleusercontent.com/qnnXPqXtyGBLdEHGcAngq6sHLV2JH0ZBHa2V8MsKFhpY1S-xcvKBraxhsXvkhGoDM2O5CZhRPzxgdE6imJvOhiyUH-zh7hQz0LTsxvWifOKhEcDC__f6ivA8wwyf67E6Wi5qN2E" alt="tasksjsonconfig.png" width="624px;" height="373px;" /></span></span>
</p>

<p dir="ltr">
  <span>While your application is building, you’ll notice an indicator at the bottom left of the screen</span>
</p>

<p dir="ltr">
  <span><img src="https://lh5.googleusercontent.com/LQfYjEPwUAFJZciR-NpauyxTF0w_62Kkv6Z8Dv25vSuTPR9ocIb0Dwbw6f4epiPp4PE-WcesIQuhI7oaVLt0Hqco0Y7G4_p9GYuVFhyF_5c6IcBiYRecjZiNcX-GvxVStrRgqOw" alt="buildinprogress.png" width="124px;" height="39px;" /></span>
</p>

<p dir="ltr">
  <span>The icons show source control status, build indicator (spinning), number of errors, and number of warnings.  You can click on the source control, error, and warning indicators for more information.</span>
</p>

<p dir="ltr">
  <span>In the lower right corner, you’ll see the rest of the status bar, showing the cursor position, line endings, language, and smiley. Click the smiley to send us feedback!</span>
</p>

<p dir="ltr">
  <span><img src="https://lh6.googleusercontent.com/RKoUfCqfZSTyTsFDrs0vQGqGK79pwIfttYCcKRlBlLloIFXSFjAqPt_trIF1PHYDOWRzA9AhqhMEEjA2T5elGllCDpbgPIt7bNhfL-SKPx8mZxuKXE-Nzq9H1VNSQJWoxYQSIgE" alt="" width="305px;" height="33px;" /></span>
</p>

<div>
  Visual Studio Code is still a preview release.  If you notice issues or have ideas for feature requests, be sure to use the smiley to let us know.  We look forward to hearing from you.
</div>