---
layout: post
title: Ease of OpenShift
tags:
- OpenShift
- Red Hat
- J2EE
published: true
---
I originally heard of [Red Hat's OpenShift](https://openshift.redhat.com/app/)
Platform as a Service from
[_CHRIS WONG'S DEVELOPMENT BLOG_](http://chriswongdevblog.blogspot.com/2011/12/reconsidering-cloud.html).
He did not go into great detail about OpenShift, and I first played around with [Jelastic](http://jelastic.com/)
back when they were in beta. Unfortunately I would have to pay ~$30/month for running my JSP/Servlets/JDBC
web app on Jelastic's PaaS at close to minimum traffic levels. If you want to control all your scaling via
a sexy in-browser UI then look no further to Jelastic. But if I was going to pay $30/month, why not pay
$40/month and get my own [colocated 1U](http://www.fdcservers.net/server_colocation.php)?

To set up the OpenShift PaaS for running a standard MySQL powered Java web app:

1. Sign up for an account. Duh. The automated system should email you in a few minutes.
2. Log in and head over to the [_Create a New Application_](https://openshift.redhat.com/app/console/application_types) tab.
3. I opted to go for the _JBoss Application Server 7.1_ app type, did not want to spend the time on the _Do-It-Yourself_ option.
4. Name the app and hit _Create Application_.
5. Add the _mysql-5.1_ cartridge. I also added the _phpmyadmin-3.4_ cartridge cause I am a sucker for that sort of thang.
6. Emails should be sent to you for the mysql instance and phpmyadmin auth.
7. Clone your app's git repo so you can add your app code in. I chose to delete everything except
        the _.openshift_ and _deployments_ directories because I would much prefer building
        and sticking a ROOT.war in the _deployments_ directory than having a second source tree to
        keep up to date. There are ways to automate this, but I only use OpenShift to host production code.
8. Push your new J2EE app live by pushing to the remote end.
It is a work-flow that I think is reasonably slick at this point.

The only thing that I could not accomplish from the web panel or Git was adding an alias;
so I could use somehost.cooldomain.ext.

The official command is
`rhc app add-alias -l [email@alias.com] -a [app name] --alias mycool.alias.com`.
But on the
[Mac OS X machine I was using, I ran the setup](http://docs.redhat.com/docs/en-US/OpenShift/2.0/html/Getting_Started_Guide/sect-Getting_Started_Guide-Installing_on_Mac_OS_X-Installing_Client_Tools.html)
of the gems locally. So it looked more like
`cd ~/.gem/ruby/1.8/bin; ./rhc-app add-alias -l [email@alias.com] -a [app name] --alias mycool.alias.com`.
Don't forget to update your domain's authoritative DNS servers.

According to a Red Hat representative I emailed:
> I know it's hard to believe but we plan to keep today's 3 free gears under the
> free tier when we introduce a paid plan later this year. Right now it's a wip and I've
> attached our current plan to this email
> [[PDF link](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvbjJMMFlCTUJzbmM)],
> appreciate your feedback.
> 
> Regarding scaling & costs, we have a feature in our roadmap that will allow users to
> set thresholds for their applications both on min/max gears in the application and cost.
> That should help you to scale without blowing your budget.

__TL;DR check out OpenShift for great JBoss hosting__ (among other things)
