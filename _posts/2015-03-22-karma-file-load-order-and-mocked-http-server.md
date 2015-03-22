---
layout: post
title: Karma file load order and mocked HTTP server
tags:
- Karma
- testing
- node
- JavaScript
published: true
---
It took me far too long to figure out
[Ordering with Karma](http://karma-runner.github.io/0.12/config/files.html).

> - The order of patterns determines the order in which files are included in the browser.
> - Multiple files matching a single pattern are sorted alphabetically.
> - Each file is included exactly once. If multiple patterns match the same file, it's included as if it only matched the first pattern.

Notice the `files` block in the following Gist, this is the output of
transpilation from our typescript business logic that is then used in testing.

{% gist 083994efc0bcdb043660 %}

[GitHub Gist](https://gist.github.com/jzerbe/083994efc0bcdb043660).

---

In this particular repo, everything is written in typescript, including Jasmine
spec files:

{% gist 1a14bec710e21fb6956a %}

[GitHub Gist](https://gist.github.com/jzerbe/1a14bec710e21fb6956a).

---

The whole build process is managed using - https://github.com/Workiva/wGulp -
see the following:

{% gist ea15cb869fe6abd40bd6 %}

[GitHub Gist](https://gist.github.com/jzerbe/ea15cb869fe6abd40bd6).
