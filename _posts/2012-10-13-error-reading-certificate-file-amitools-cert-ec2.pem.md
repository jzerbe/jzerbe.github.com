---
layout: post
title: error reading certificate file amitools/cert-ec2.pem
tags:
- AWS
- AMI
- certificate
- bundle
published: true
---
I was working through creating a bunch of Amazon Machine Instances for internal use.
And here is the kicker: they all descend from a single Debian 6 image with
some custom Java and build tools support.

I proceeded to get the _error reading certificate file amitools/cert-ec2.pem_
error, as [posted by traxonius](https://forums.aws.amazon.com/thread.jspa?threadID=84085),
when trying to create new descendant images.

I also had the pleasure of getting keyring errors because all of the
Debian apt-get keyring files get dumped by the AMI bundle rules.


### my original API/AMI setup

I set up my tools in `/opt/ec2` based on the following commands from the
[_CreateEC2Image_ article on the Debian project wiki](http://wiki.debian.org/Cloud/CreateEC2Image).

    wget http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip
    wget http://s3.amazonaws.com/ec2-downloads/ec2-ami-tools.zip
    
    unzip ec2-api-tools.zip
    unzip ec2-ami-tools.zip
    
    mkdir /opt/ec2
    
    rsync -ar ec2-api-tools-*/* /opt/ec2
    rsync -ar ec2-ami-tools-*/* /opt/ec2
    
    rm -rf ec2-api-tools*
    rm -rf ec2-ami-tools*

Export `EC2_HOME` by adding `export EC2_HOME=/opt/ec2` to `/etc/profile`.
Add `EC2_HOME/bin` to your `PATH` by updating `export PATH` to look like `export PATH=$PATH:$EC2_HOME/bin`.
`chmod -R 755 /opt` for good measure.


### the hackish fix

If you are only using these images for intra-company use, then you might be able
to take advantage of the slightly hackish solution I used. I removed
the excerpted lines from the `/opt/ec2/lib/ec2/platform/base/constants.rb` file.

    '"*.pem"',
    '"*.priv"',
    '"*.gpg"',
    '"*.jks"',
