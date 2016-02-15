---
layout: post
title: Spring Security URL parameter based auth
tags:
- Spring
- Security
- parameter
published: true
---
For some webservices, authentication needs to happen based on URL
parameters. That JSON client you built into your iOS app is not going to like
rando form login HTML coming in. Let us make Spring bend to our will. This is
from a pruned down version of code I wrote based on
[_Implementing REST Authentication_](http://www.objectpartners.com/2011/06/16/implementing-rest-authentication/)
I have been using in production.

Just the good parts!


### extend OncePerRequestFilter

This guy comes from org.springframework.web.filter.OncePerRequestFilter, which
is in the
[__spring-web__ class library](http://mvnrepository.com/artifact/org.springframework/spring-web).

    public class ParameterAuthFilter extends OncePerRequestFilter {
        
        @Override
        protected void doFilterInternal(final HttpServletRequest request, final HttpServletResponse response, final FilterChain theFilterChain) throws ServletException, IOException {
            if (isValidAuthBasedOnParams(request)) {
                theFilterChain.doFilter(request, response);
            } else {
                response.sendError(HttpServletResponse.SC_UNAUTHORIZED, "invalid parameter authentication");
            }
        }
        
    }


### update web.xml

    <filter>
        <filter-name>signatureFilter</filter-name>
        <filter-class>com.vraidsys.auth.ParameterAuthFilter</filter-class>
    </filter>
    
    <filter-mapping>
        <filter-name>signatureFilter</filter-name>
        <url-pattern>/api/*</url-pattern>
    </filter-mapping>
