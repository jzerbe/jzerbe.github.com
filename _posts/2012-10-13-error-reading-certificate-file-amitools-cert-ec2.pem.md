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
I was working through creating a bunch of
<abbr title="Amazon Machine Instance">AMI</abbr>s for internal work use.
And here is the kicker: they all descend from a single Debian 6 image with
some custom Java and build tools support.<br />
<br />
I proceeded to get the "error reading certificate file amitools/cert-ec2.pem"
error, as
<a href="https://forums.aws.amazon.com/thread.jspa?threadID=84085">posted by traxonius</a>,
when trying to create new descendant images.<br />
I also had the pleasure of getting keyring errors because all of the
Debian apt-get keyring files get dumped by the AMI bundle rules.<br />
<br />
<strong>my original API/AMI setup</strong><br />
I set up my tools in <code>/opt/ec2</code> based on the following commands from the
<a href="http://wiki.debian.org/Cloud/CreateEC2Image"><i>CreateEC2Image</i> article on the Debian project wiki</a>.
<blockquote>
    <code>
        wget http://s3.amazonaws.com/ec2-downloads/ec2-api-tools.zip<br />
        wget http://s3.amazonaws.com/ec2-downloads/ec2-ami-tools.zip<br />
        unzip ec2-api-tools.zip<br />
        unzip ec2-ami-tools.zip<br />
        mkdir /opt/ec2<br />
        rsync -ar ec2-api-tools-*/* /opt/ec2<br />
        rsync -ar ec2-ami-tools-*/* /opt/ec2<br />
        rm -rf ec2-api-tools*<br />
        rm -rf ec2-ami-tools*<br />
    </code><br />
    <br />
    Export EC2_HOME by adding <code>export EC2_HOME=/opt/ec2</code>
    to <code>/etc/profile</code>.<br />
    <br />
    Add <code>EC2_HOME/bin</code> to your <code>PATH</code> by updating
    <code>export PATH</code> to look like <code>export PATH=$PATH:$EC2_HOME/bin</code>.<br />
    <code>chmod -R 755 /opt</code> for good measure.
</blockquote>
<br />
<strong>the hackish fix</strong><br />
If you are only using these images for intra-company use, then you might be able
to take advantage of the slightly hackish solution I used. I removed
the excerpted lines from the following file.<br />
<code>/opt/ec2/lib/ec2/platform/base/constants.rb</code><br />
<blockquote>
    <code>
        '"*.pem"',<br />
        '"*.priv"',<br />
        '"*.gpg"',<br />
        '"*.jks"',<br />
    </code>
</blockquote>
<br />
