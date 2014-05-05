---
layout: post
title: access MySQL of scaled OpenShift application
tags:
- MySQL
- OpenShift
- scale
published: true
---
One of the great things about running code on OpenShift is the
[auto-scaling properties of cartridges](https://www.openshift.com/developers/scaling).
One just needs to specify during the setup (via the command line or the web interface)
the level of scaling.

Getting _root-ish_ access to the MySQL cartridge schema becomes a tad
troublesome in these instances. PHPMyAdmin is not supported for scaled apps,
and to keep developers from committing a security faux pas, the MySQL instance
(or db of choosing) runs in a virtual network of sorts inaccessible to the
at-large Internet.

###The Goods
1. SSH into your application with the private key you added to your account.
See [Remote Access to Your Application](https://www.openshift.com/developers/remote-access)
for more.
2. `env | grep "mysql"`. Should return back an environment variable
- `OPENSHIFT_MYSQL_DB_URL` - in the listing. Should have a value like:
`mysql://user:password@host-domain.rhcloud.com:42424/`.
3. Setup your SSH client to tunnel all traffic directed at port `3307` to
(in this example) `42424`; aka _local port forwarding_.
For a great explanation on tunneling see:
[SSH Tunneling Explained](http://chamibuddhika.wordpress.com/2012/03/21/ssh-tunnelling-explained/).
4. Start tunnel, and use the database management UI of choosing to connect to
`127.0.0.1:3307`, with the _user_ and _password_ from the URI string. I like
[HeidiSQL](http://www.heidisql.com/) on Windows.

![](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvdEl3eFJVYzl1YzA)
