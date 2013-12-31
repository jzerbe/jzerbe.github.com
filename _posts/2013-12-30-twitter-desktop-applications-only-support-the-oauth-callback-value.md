---
layout: post
title: Twitter desktop applications only support the oauth callback value
tags:
- Twitter
- Meteor
- oauth
published: true
---
First use of the `accounts-twitter` Meteor package ended in getting the
following dump from the Twitter API:

    Desktop applications only support the oauth_callback
    value 'oob'/oauth/request_token [401]

I know my `accounts-ui` configuration in `server/configure_login_service.js`
is correct.

    Accounts.loginServiceConfiguration.remove({
        service: "twitter"
    });
    Accounts.loginServiceConfiguration.insert({
        service: "twitter",
        consumerKey: "...",
        secret: "..."
    });

According to the discussion on -
[Desktop applications only support the oauth_callback value 'oob'/oauth/request_token](https://dev.twitter.com/discussions/392)
- it seems as though the fix it to put in placeholder data in the __Callback URL__
field in the app settings. I also ticked the __Sign in with Twitter__ check box
and things are working great.

![](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvTFI4TmdoUDR1RDg)
