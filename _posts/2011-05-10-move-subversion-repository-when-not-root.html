---
layout: post
title: move subversion repository when not root
tags:
- rsvndump
published: true
---
Over the course of the past semester, I built this large code-base for
<a href="http://www-users.cselabs.umn.edu/classes/Spring-2011/csci3081/">CS3081W</a>.
The course is supposed to expose many of the kids who have never built long-term software to the process.
Seeing as how I have contract work that spans several months during the summers and personal projects that
have been ongoing for the past couple of years, this class was in the bag. The problem in all of this was that
all of our code was tied up on the CS subversion severs; not like I would have any chance at convincing the
operators to let me get in and use svndump. So here is how I pulled my code off our repository without direct
file system access to the repository in question.
<ol>
    <li>Download and install <a title="see the main project page for more information" href="http://rsvndump.sourceforge.net/">rsvndump</a>
        <ul>
            <li>install libapr-dev and libsvn-dev (<code>apt-get install libapr1-dev libsvn-dev</code>)</li>
            <li>the latest at time of writing: <code>wget http://prdownloads.sourceforge.net/rsvndump/rsvndump-0.5.5.tar.gz</code></li>
            <li>do the usual <code>./configure</code> after getting into the untar'd rsvndump archive (didn't work on cygwin)</li>
            <li><code>make</code>, then <code>make install</code> (as root)</li>
        </ul>
    </li>
    <li><code><a title="see the manpage for more" href="http://rsvndump.sourceforge.net/manpage.html">rsvndump</a> -v -u [svn user] -p [svn pass] [url to svn repo] &gt; [reponame].dump</code></li>
    <li>Create your new subversion repository on the new host machine and import the dump
        <ul>
            <li><code>svnadmin create [reponame]</code></li>
            <li><code>svnadmin load [reponame] &lt; [reponame].dump</code></li>
        </ul>
    </li>
</ol>
