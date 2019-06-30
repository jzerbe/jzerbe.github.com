---
layout: post
title: continuous deployment with PM2
tags:
- PM2
- continuous deployment
published: true
---
I have a series of `.js` configuration files for my Node application.
Each describe a collection of data sources and runtime parameters.
None of this is containerized yet.

The following is an excerpt from my [`ecosystem.config.js`](https://pm2.io/doc/en/runtime/reference/ecosystem-file/) in my project root based on <https://pm2.io/doc/en/runtime/guide/easy-deploy-with-ssh/>:

    module.exports = {
      apps: [{
        env: {
          NODE_ENV: 'production',
        },
        name: 'myApplication',
        script: 'dist/server.js',
      }],
      deploy: {
        cs: {
          env: {
            NODE_ENV: 'production',
          },
          host: ['host1.application.com'],
          key: 'user1_at_host1.pem',
          path: '/home/myApplication/instance1',
          ref: 'origin/master',
          repo: 'git@github.com:Company/myApplication.git',
          ssh_options: 'StrictHostKeyChecking=no',
          user: 'user1',
          'post-deploy': 'app/config.sh app/configs/instance1.js host1.application.com 3001 && cd app && npm install && npm run build && ./restart.sh cs && git checkout .',
        },
        ...
    }

The `config.sh` file wraps `sed --expression=s/baseSettings:/host:\'$2\',port:$3,baseSettings:/ $1 > app/config.js`
to configure the listen host and port on a particular running instance.

`restart.sh` wraps a hard reset of the single-instance Node server (no hot deploys)

    appList=`./node_modules/pm2/bin/pm2 ls`
    if [[ ${appList} == *$1* ]]; then
      ./node_modules/pm2/bin/pm2 delete $1
    fi
    ./node_modules/pm2/bin/pm2 start dist/server.js -i 1 --name $1

At this point it is possible to hook up Jenkins to run `./node_modules/pm2/bin/pm2 deploy [environment name] origin/master`
