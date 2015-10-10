---
layout: post
title: my software infrastructure story
tags:
- hosting
- software
- infrastructure
published: true
---
I get paid to make software systems work together. Sometimes this means I get paid to make judgement calls about
how and where these systems should run. Some developers I know do not appreciate this part of the job, but I
love it. Probably stems from my tight history with operations (ops). This is intended to be my personal history
to enrich the person with minor software development exposure.

### before my time
Mainframes. Time sharing terminals. Whoa! I have heard stories from my dad about punch-card stacks,
seen inoperable main frames when I did [data center](https://en.wikipedia.org/wiki/Data_center)
management software for a summer, and heard legends from crusty EEs.

I will sum up 40 years of history in: arithmetic devices, single-purpose logic devices,
non-portable programs, single-user portable programs, and finally multi-user systems.
Following all of this there was a massive decentralization (late 80s through the 90s)
as software started being run on desktop-ish looking off-white towers, usually running some
[BSD](https://en.wikipedia.org/wiki/Berkeley_Software_Distribution) derivative.

### shared hosting
An Internet-connected (assumed) physical computer (_box_ in hosting parlance) has a
[web server](https://en.wikipedia.org/wiki/Web_server) (program that responds to HTTP requests in this context)
running (aka server process). This box also has a means of updating files on it, usually some FTP variant.
For a discussion on the main two - <http://www.wise-ftp.com/know-how/ftp_and_sftp.htm> - and
the principles behind FTP - <https://wiki.filezilla-project.org/Network_Configuration#Technical_background>.

On this single box, there are many users with a variety of websites. There is usually some software configured to
keep users from getting into the files of one another, and some attempt at keeping one user/website from hogging
all the resources of this box. This gets especially difficult to enforce when websites are allowed to execute (server-side)
instructions to fetch your comment from a database, display the right profile picture from a collection of millions,
or keep your credit card information between your latest purchases. For more information on one of the primary methods
of keeping websites from touching the files of the other back when I was in this space:
<http://unix.stackexchange.com/questions/105/chroot-jail-what-is-it-and-how-do-i-use-it>.
To my knowledge there still is no one tried and true way to keep one website from hogging all the processing resources.

I started out on the ghetto-ridiculous version: free web hosting. The box owner will inject ads into the pages
the server process returns to any page viewers (including your website). And you thought margins were already as low
as they could go! Most of the providers I started out with no longer exist, although looks like there is still the
handy-dandy search: <http://www.free-webhosts.com/>.
Mind you this was before [WordPress.com](https://wordpress.com/) or [weebly](http://www.weebly.com/) hit the scene;
bargain basement no-frills (or if there were, massive upsells).

One of the best shared providers I have ever used is <https://www.nearlyfreespeech.net/>. Highly recommended
for a developer starting out.

### dedicated hosting
Okay, so remember how we had that box owner who had a bunch of tenants? You too can be a box owner without too
much trouble. The idea is to lease a box from a company with many boxes; the monthly fee usually includes the
electricity and a certain speed limit and amount of data transfer to the box.

By doing this, I get full control over what is running on this system. Obviously, I still have to adhere to the
laws of the jurisdiction this system is running in (more on that later). With great freedom, comes great responsibility.
As such, I would have to take care of all system updates/security. If I lock myself out, then the hosting company I am
leasing from, might be nice enough to do a completely fresh installation of my operating system of choice
(Windows costs extra) and send me the new administrator login.

Notice that on the [_dedicated servers_ page](https://www.fdcservers.net/dedicated-servers.php), FDC Servers lists
where the physical server is located. If you ask, they will probably also tell you what
[network carriers](http://whatis.techtarget.com/definition/carrier-network) provide
service to the climate controlled, secure-access building where all these servers are housed.

### virtual private server (VPS)
I did not get into the dedicate server market, but I did for a time lease a few VPSs. Remember when I mentioned
adhering to laws of the jurisdiction? Certain countries are more _laissez faire_ than others in terms of their
computer laws. For a time I leased from <http://shinjiru.com/>. Notice on their Acceptable Use Policy -
<http://shinjiru.com/legal/acceptable-user-policy.php> - there is no explicit mention of copyrighted content
violations, while on a Panamanian provider there is - <http://www.ccihosting.com/aup.php>.

I will link out to the
wikipedia entry on this one - <https://en.wikipedia.org/wiki/Virtual_private_server> - the _Virtualization_ section
is a must read to understand any later developments. With this kind of technology, the hosting provider can lease
portions of a physical machine without any of the risks associated with shared hosting. Suddenly, I do not need to
keep as much inventory overhead.

On the consumer side, the management burden is lower, as I can use a template to fill my portion of the physical machine.
I say template as this is a set of instructions of exactly what software I want running, in addition to the base
operating system. The implications are enormous! I can suddenly control everything that my software needs to run!
Why test a particular release version? I can test a particular
[virtual machine](https://en.wikipedia.org/wiki/Virtual_machine) template and know exactly how my
service will function; zero surprises.
This is why companies like [Docker](https://www.docker.com/) are blowing up.

### colocation
Very similar agreements to dedicated hosting, except I provide the hardware in exchange for a lower rate. Makes sense?
I am just paying for electricity and a network connection at this point.

Back in July 2009 I purchased a used [1U](https://en.wikipedia.org/wiki/Rack_unit) Dell Poweredge on eBay and had
it shipped to a spot I had reserved in a
data center (Chicago) run by [FDC Servers](https://www.fdcservers.net/colocation.php). I ran 3 virtual private
servers on top of [VMware Server](https://en.wikipedia.org/wiki/VMware_Server)
[on CentOS 5.8](http://vraidsys.com/2012/08/vmware-server-1.0.10-on-centos-5.8/):
[TeamSpeak](https://www.teamspeak.com/downloads) on [Debian](http://www.debian.org/),
[Counter Strike on Debian](http://vraidsys.com/2009/01/counter-strike-debian-linux-server-setup/),
and a shared hosting environment for contract clients. That lasted until late 2011 when I was able to offload
most of my contract clients.

Hosting providers will sometimes flaunt the list of carriers,
or advertise as carrier-neutral (meaning they have multiple options).
Premo data centers usually have at least two [Tier 1 providers](https://en.wikipedia.org/wiki/Tier_1_network).

Why would I care what carriers service a particular data center? Why would I want to put my hardware somewhere else?
If I can provide faster access to my services by getting as close/direct line to my users,
my users like me more, and I make more money.
This gets especially crazy when milliseconds can make millions of dollars of difference in software
[arbitrage](https://en.wikipedia.org/wiki/Arbitrage). If I can
[colocate my servers next to the NASDAQ exchange servers](http://www.nasdaqtrader.com/Trader.aspx?id=colo), I win.

### the cloud
In my head this is confluence of technologies and business practices as apposed to a defining moment.

<https://en.wikipedia.org/wiki/Cloud_computing>:
> Cloud computing is a model for enabling ubiquitous, convenient,
> on-demand access to a shared pool of configurable computing resources.

Like colocation, I can choose the region where my services are running. Like VPSs, I can template the complete
installation process to make it fast to add more resources in a repeatable fashion.

My databases professor, who gave me my first W-2 job out of college at his startup, spins a yarn something like this:
1. Amazon grows like gangbusters
2. Amazon needs more capacity to handle more customer requests (and support software - fraud, line of business)
3. Amazon has excess capacity that they realize they can recoup costs on
4. Amazon leases out excess capacity (in particular regions) behind slick provisioning software
5. Amazon Web Services division makes fat stack of cash in its own right: <http://techcrunch.com/2015/07/23/amazons-aws-unit-reports-q2-revenue-of-1-8b-391m-profit/>

### something for the road
In having this small personal historical perspective, it makes making judgement calls that much easier.
Hopefully some of this history will provide better context for making your own decisions. If you want more:

- [The evolution of cloud: the intergalactic infrastructure of the future](http://www.information-age.com/technology/cloud-and-virtualisation/123458755/evolution-cloud-%E2%80%98intergalactic-infrastructure-future)
