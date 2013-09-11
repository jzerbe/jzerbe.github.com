---
layout: post
title: split django unit tests into files
tags:
- python
- django
- tests
- split
status: publish
type: post
published: true
---
###motivation###
I like having unit tests grouped into separate files as any self-respecting
unit test framework typically allows. Kind of a shock that django didn't get
things right out of the box. The start of my research landed me on
[split django app tests in several files](http://stackoverflow.com/questions/6163108/split-django-app-tests-in-several-files/6163375#6163375).

###create/update files###
- I
[forgot to add](https://github.com/jzerbe/taskifier/commit/e60af1652fd3e762752f52370c273a5579d04f9f)
my application to django's __INSTALLED_APPS__ constants in `settings.py`;
don't make that mistake.

- django assumes that `<root dir>/<app name>/tests.py` exists, instead add
`<root dir>/<app name>/tests/__init__.py` to masquerade a package. Examples:
[taskifier/tests/__init__.py](https://github.com/jzerbe/taskifier/blob/master/taskifier/tests/__init__.py) and
[polls/tests/__init__.py](https://github.com/toastdriven/guide-to-testing-in-django/blob/master/polls/tests/__init__.py).

- The previously mentioned import would then import all tests in
[taskifier/tests/helpers.py](https://github.com/jzerbe/taskifier/blob/master/taskifier/tests/helpers.py)
or
[polls/tests/forms.py](https://github.com/toastdriven/guide-to-testing-in-django/blob/master/polls/tests/forms.py)
respectively.
