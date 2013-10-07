---
layout: post
title: GWT devmode on Windows requires localhost permissions
tags:
- GWT
- Windows
- Java
- permissions
published: true
---
While GWT\'s devmode plugin should work out of the box when one is connecting
from localhost to localhost. This is not the case on Google\'s own Chrome.

I have noticed this sort of behavior on Windows
before; it is very particular about specific address matching.
`127.0.0.1 != localhost` on Windows.

But then why does this not happen on Firefox or IE?


###the fix

![GWT devmode Chrome plugin](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvZm41VzhwNFJBRGc)
