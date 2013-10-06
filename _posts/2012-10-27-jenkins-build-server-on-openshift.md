---
layout: post
title: Jenkins build server on OpenShift
tags:
- Jenkins
- OpenShift
published: true
---
__NOTE__: This information has been superseded by
[Jenkins build server on OpenShift (2)](http://vraidsys.com/2013/06/jenkins-build-server-on-openshift-2/).

You should sign-up for an OpenShift account, just to make use of being able
to spin-up your own [Jenkins-CI](http://jenkins-ci.org/) build
server for FREE.

Granted, connecting to external SSH backed GIT repositories is not possible,
due to the inability to add to the `known_hosts` file,
but there are ways around this.

I have made use of bitbucket's "get more users" deal when you
invite some people and they sign-up: I created a second account with a dummy
password for use with connecting over HTTPS, as I do not want to divulge
my actual account password in the Jenkins config.

###auto-install Jenkins

The OpenShift application maintenance panel provides an option to create a
Jenkins build server _gear_ when digging around in your existing
application settings. The __Enable Jenkins builds__ button is your
friend.


###with the OpenShift app's internal GIT repo

When you log in for the first time to your new Jenkins gear, you should see a
`_somethingsomething_-build` job already created. If you are using the
GIT repo associated with your application and do not touch any configuration
files, then everything should work out of the box. However, I wanted to be
be able to build any type of project.


###break the sandbox - build tools

1. Manage Jenkins -> Configuration
    - # of executors = 1
    - Usage = Utilize this slave as much as possible
2. The Git plugin and binaries are already installed, but you will need
        to correctly configure them. I named mine `git 1.7.1` and
        had to set the path `/usr/bin/git`. May want to confirm
        with a `git --version` and `which git`.
3. The Maven plugin and binaries are also already installed, one may need
        to set up the naming => `Maven 3.0.3`, with
        `MAVEN_HOME` = `/usr/bin`. On OpenShift, the only option for
        Maven repository information will be to use the
        _Local to workspace_ option; nothing else is writable.
4. Download and extract Apache Ant from one of the
        [binary archives](http://ant.apache.org/bindownload.cgi).
        Install the Ant plugin. Configure with a name and the full path
        to the root directory Ant was extracted into. On my gear, that was
        `/var/lib/stickshift/[...]/app-root/data/apache-ant-1.8.4`.


###break the sandbox - config with 3rd party repo

1. Create a __New Job__ copied from the existing
        `_somethingsomething_-build` job. I called mine
        `_somethingsomething_-custom`.
2. May want to do something about: _Discard Old Builds_
3. Source Code Management -> Git
    - URL of repository = `https://[username]:[password]@bitbucket.org/you/cool_repo.git`
    - Branch Specifier = `origin/master`
4. Build Triggers -> Trigger builds remotely -> Authentication Token.
        Input a long-ish string that is secret. Then in the _Services_
        section of `cool_repo`: `Token` would be set to
        said long-ish string, `Project name` =
        `_somethingsomething_-custom`, `Endpoint` =
        `https://[user]:[API token]@jenkins-[namespace].rhcloud.com`.
        
        __NOTE__:
        [Launching a build with parameters](https://wiki.jenkins-ci.org/display/JENKINS/Parameterized+Build#ParameterizedBuild-Launchingabuildwithparameters)
        requires additional configuration.


###split up the Build steps

Summary: all WARs built by Ant get put into `deployments` directory
and pushed onto OpenShift gear for production use. You will need to add the
public key version of the `jenkins_id_rsa` SSH key to your OpenShift
account's list of trusted keys.

__ssh-wrapper script__:

    #!/bin/bash
    
    ID_RSA="$OPENSHIFT_DATA_DIR/.ssh/jenkins_id_rsa"
    
    ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i $ID_RSA $1 $2

1. _This build is parameterized_, __String Parameter__
> Name = `WAR_FILE`
> Default Value = `ROOT.war`
> Description = `filename of WAR to be deployed to server`
2. __Execute shell__
>
>    # Build setup and run user pre_build
>    . ci_build.sh
>    
>    if [ -e ${OPENSHIFT_REPO_DIR}.openshift/markers/java7 ];
>    then
>        export JAVA_HOME=/etc/alternatives/java_sdk_1.7.0
>    else
>        export JAVA_HOME=/etc/alternatives/java_sdk_1.6.0
>    fi
3. __Invoke Ant__: Ant 1.X.X, your-build-target
4. __Execute shell__

    source /usr/libexec/stickshift/cartridges/abstract/info/lib/jenkins_util
    
    # set deploy user and endpoint
    DEPLOY_ENDPOINT=**[username]@somethingsomething-[namespace].rhcloud.com**
    
    # set $GIT_SSH wrapper script and backup for later
    GIT_SSH_BAK=$GIT_SSH
    GIT_SSH=$OPENSHIFT_DATA_DIR/ssh-wrapper
    
    # Stop app and all deps including MySQL
    $GIT_SSH $DEPLOY_ENDPOINT 'ctl_all stop'
    
    # Delete old APP version
    $GIT_SSH $DEPLOY_ENDPOINT "rm -fr ~/app-root/repo/deployments/$WAR_FILE"
    
    # Force retry of failed APPs
    $GIT_SSH $DEPLOY_ENDPOINT "rm -fr ~/app-root/repo/deployments/*.failed"
    
    # Push content to application
    rsync -az -e 'ssh -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no -i /var/lib/stickshift/[...]/app-root/data/.ssh/jenkins_id_rsa' --exclude='*.deployed' --exclude='*.deploying' --exclude='*.isundeploying' $WORKSPACE/deployments/. $DEPLOY_ENDPOINT:~/app-root/repo/deployments/
    
    # Configure / start app
    $GIT_SSH $DEPLOY_ENDPOINT deploy.sh # run user-level deploy scripts
    $GIT_SSH $DEPLOY_ENDPOINT 'ctl_all start'
    $GIT_SSH $DEPLOY_ENDPOINT post_deploy.sh # run user-level post deploy scripts
