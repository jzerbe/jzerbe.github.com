---
layout: post
title: Make JavaScript minimization part of continuous integration
tags:
- JavaScript
- minimization
- continuous integration
published: true
---
Google has put their Closure JavaScript compiler up on App Engine at
<http://closure-compiler.appspot.com>.

I think it links in real nice with a
[Jenkins continuous integration](http://jenkins-ci.org/) setup
via a simple __Execute shell__ block:

    wget --post-data "compilation_level=SIMPLE_OPTIMIZATIONS&output_format=text&output_info=compiled_code&code_url=https://bitbucket.org/ninja_metrics/kapi-javascript-lib/raw/`git rev-parse HEAD`/KApi.js" --output-document "KApi-min.js" http://closure-compiler.appspot.com/compile

Translation: output just the minimized JavaScript to "KApi-min.js" in
the current Jenkins workspace based on the latest version of the repository.
The workspace currently contains code pulled from `origin/master`
of [https://bitbucket.org/ninja_metrics/kapi-javascript-lib](https://bitbucket.org/ninja_metrics/kapi-javascript-lib).

For more on tweaking for your particular setup, see the
[_Closure Compiler Service API Reference_](https://developers.google.com/closure/compiler/docs/api-ref).
