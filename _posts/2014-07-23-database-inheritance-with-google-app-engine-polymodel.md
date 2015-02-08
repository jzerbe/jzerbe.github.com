---
layout: post
title: database modeling with Google App Engine PolyModel
tags:
- database
- model
- Google App Engine
- PolyModel
- python
published: true
---
The following are two approaches to the problem of handling data at
arbitrary levels of specificity. Both handle the problem of
[Quotas and limits](https://developers.google.com/appengine/docs/python/datastore/#Python_Quotas_and_limits),
but only one is easy to read through. Obviously, I chose the one with
lower developer maintenance costs: PolyModel.
View [GitHub Gist](https://gist.github.com/jzerbe/466ddec827cd766313ed).

{% gist 466ddec827cd766313ed %}

> instead of dropping all None columns from existence on database put
> via a hack, one can be smart about instantiating entities at the
> appropriate specificity and reap the rewards of inheritance

[source](https://gist.github.com/jzerbe/466ddec827cd766313ed#comment-1279155)

In [_Effective PolyModel_](https://developers.google.com/appengine/articles/polymodel),
Rafe Kaplan uses the tried and true ecommerce catalog example
for illustrating the benefits of using
[PolyModel](https://developers.google.com/appengine/docs/python/ndb/polymodelclass).

> Instead of placing all products in to a single class, it is possible to define
> each category as its own class that fits naturally in to a product category hierarchy.

Do your team a favor and check out `google.appengine.ext.ndb.polymodel.PolyModel`:

1. Querying at arbitrary inheritence levels.
2. Built in caching.
3. One of the closest things you get to an SQL `JOIN` in App Engine.
4. Easy to read data modeling.
