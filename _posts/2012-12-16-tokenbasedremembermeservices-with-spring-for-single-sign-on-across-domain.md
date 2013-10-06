---
layout: post
title: TokenBasedRememberMeServices with Spring for single sign on across domain
tags:
- Spring Security
- SSO
- Token
published: true
---
###motivation

The business requirements I have been working with dictate that users should be
able to work across various deployed webapp contexts in our ecosystem with
a single password-based login after registering. The databases are a convoluted
collection of write-optimized administration tables and read-only datamart
tables. The (horizontally scaled) Tomcat 7 application servers sit upstream
nginx 1.2.6 in a single [VPC](http://aws.amazon.com/vpc/).

For now I chose to use the
[TokenBasedRememberMeServices](http://static.springsource.org/spring-security/site/docs/3.1.x/apidocs/org/springframework/security/web/authentication/rememberme/TokenBasedRememberMeServices.html)
instead of the suggested
[PersistentTokenBasedRememberMeServices](http://static.springsource.org/spring-security/site/docs/3.1.x/apidocs/org/springframework/security/web/authentication/rememberme/PersistentTokenBasedRememberMeServices.html)
to make things more straightforward for a first-run through.

[Remember-Me Authentication](http://static.springsource.org/spring-security/site/docs/3.1.x/reference/remember-me.html)
may not be the best way to go about intra-domain auto-authentication,
but sharing a single cookie for
`example.com` across `app1.example.com` and
`secure.example.com/app2/` is much easier to set-up and
maintain .... for now.


###the code

I created a complete example project at
[jzerbe / spring-security-gwt-template](https://github.com/jzerbe/spring-security-gwt-template).
For this blog post, the important files to take a look at are the
[spring-security.xml](https://github.com/jzerbe/spring-security-gwt-template/blob/master/WEB-INF/spring-security.xml)
and
[RememberMeProvider.java](https://github.com/jzerbe/spring-security-gwt-template/blob/master/src/com/vraidsys/server/data/RememberMeProvider.java)
files.
