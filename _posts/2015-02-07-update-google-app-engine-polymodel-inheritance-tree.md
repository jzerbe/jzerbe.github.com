---
layout: post
title: update Google App Engine PolyModel inheritance tree
tags:
- database
- model
- Google App Engine
- PolyModel
- inheritance
- migrate
- python
published: true
---
It is perfectly possible to alter a
[PolyModel](https://cloud.google.com/appengine/docs/python/datastore/polymodelclass)
inheritance tree and then migrate the existing entities to the new tree.
Without running a migration, the data will get deserialized from bigtable with
just the fields of the class which is the parent of the changes.

In the following example code, I will alter the tree from
`GrandParent -> Child` to `GrandParent -> Parent -> Child`,
and add the intermediary `Parent` class to the `class_` field where
the inheritance path is tracked. If I did not run the migration, then we
would just see those fields that exist on `GrandParent`.

{% gist 2615b891e35159d00a67 %}

View [GitHub Gist](https://gist.github.com/jzerbe/2615b891e35159d00a67).
