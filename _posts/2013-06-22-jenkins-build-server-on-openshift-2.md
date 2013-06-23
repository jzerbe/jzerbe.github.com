---
layout: post
title: Jenkins build server on OpenShift (2)
tags:
- Jenkins
- build
- OpenShift
status: publish
type: post
published: true
---
I originally wrote on spinning up a Jenkins server for intra-OpenShift
deployments with an external repository in
[Jenkins build server on OpenShift](http://vraidsys.com/2012/10/jenkins-build-server-on-openshift/).
This post will detail the basic process and major changes that have been
required as the OpenShift platform has evolved to the
[General Availability release](https://www.openshift.com/blogs/announcing-the-general-availability-of-openshift-online).

This assumes you already have an
[OpenShift account](https://openshift.redhat.com/app/account/new),
an application server cartridge, a
[Jenkins cartridge](https://openshift.redhat.com/app/console/application_types?search=jenkins),
and a new _diy_ project created in Jenkins. I would advise against using
OpenShift&#39;s Jenkins-app-server-integration, and just spin them up separately.
At time of writing the OpenShift maintained Jenkins cartridge still reads
_Jenkins Server 1.4_, but it is actually _1.509.1_.

###create SSH key-pair###
Create an SSH key on the fresh Jenkins cartridge in `~/app-root/data/.ssh/id_rsa`
with `ssh-keygen -C jenkins@openshift`, and add it to the
[Account Settings](https://openshift.redhat.com/app/console/settings). This will
ensure that authentications with the key are accepted on any of your
account&#39;s cartridges.

###deployment to other cartridge from Jenkins###
    # set deploy user and endpoint
    DEPLOY_ENDPOINT=[alpha-num username]@[cart name]-[namespace].rhcloud.com

    # Push content to server
    scp -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ~/app-root/data/.ssh/id_rsa $WORKSPACE/*.war $DEPLOY_ENDPOINT:~/jbossews/webapps/

    # Reload server with new version
    ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i ~/app-root/data/.ssh/id_rsa $DEPLOY_ENDPOINT "~/jbossews/bin/control restart"

As one can see, I am deploying to a
[JBoss EWS](https://openshift.redhat.com/app/console/application_types?search=jboss+ews)
cartridge.

###service linking###
![Bitbucket Jenkins service](/post-assets/bitbucket_jenkins_service.png)

You can emulate what Bitbucket will do with the following command:
`wget --auth-no-challenge --post-data="" http://[user]:[API token]@[cart]-[ns].rhcloud.com/job/[project name]/build?token=[token]`

`http://[user]:[API token]@[cart]-[ns].rhcloud.com/` should be equivalent to what
you have in the _Endpoint_ field.


Be sure to configure Jenkins&#39; __Global Security__ properly so that `[user]`
is able to start builds:

1. Security Realm: Jenkins's own user database
2. Authorization: Matrix-based security
    1. Overall: Read
    2. Job: Read, Discover, Build
    3. View: Read
