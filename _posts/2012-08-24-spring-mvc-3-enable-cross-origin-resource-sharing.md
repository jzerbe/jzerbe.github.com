---
layout: post
title: Spring MVC 3 enable Cross Origin Resource Sharing
tags:
- Spring
- MVC
- CORS
published: true
---
I am using a Spring MVC 3 annontation driven setup, and I needed to enable
Cross-Origin Resource Sharing. Relevant reading:

- [W3C spec](http://www.w3.org/TR/cors/) is informative but not immediately helpful
- the [enable CORS cheat-sheet](http://enable-cors.org/) has a fast-action listing of relevant headers
- build [CORS in vanilla JavaScript](http://www.nczonline.net/blog/2010/05/25/cross-domain-ajax-with-cross-origin-resource-sharing/)
- build [AJAX in vanilla JavaScript](http://www.xul.fr/en-xml-ajax.html)


###update web.xml

Update the DispatcherServlet section of your web.xml file (typically in WEB-INF)
so that it dispatches HTTP OPTIONS requests to controllers instead of pushing back the
default response that does not include the relevant _Access-Control-Allow_
headers.

    <servlet>
        <servlet-name>spring-servlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>dispatchOptionsRequest</param-name>
            <param-value>true</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>spring-servlet</servlet-name>
        <url-pattern>/*</url-pattern>
    </servlet-mapping>


###create an OPTIONS RequestMapping

In my case I want CORS to always work for any HTTP end-points, so I created a
__CorsCommon__ class that I extend all my other controllers from.
NOTE: In my experience, one cannot just use `"*"` for `Access-Control-Allow-Headers`,
so if your CORS request is going to be using any additional headers, be sure to list them
where `my-cool-header` is. jQuery likes to use the `x-requested-with` header.

    public class CorsCommon {
        
        @RequestMapping(method = RequestMethod.OPTIONS)
        public void commonOptions(HttpServletResponse theHttpServletResponse) throws IOException {
            theHttpServletResponse.addHeader("Access-Control-Allow-Headers", "origin, content-type, accept, x-requested-with, my-cool-header");
            theHttpServletResponse.addHeader("Access-Control-Max-Age", "60"); // seconds to cache preflight request --> less OPTIONS traffic
            theHttpServletResponse.addHeader("Access-Control-Allow-Methods", "GET, POST, OPTIONS");
            theHttpServletResponse.addHeader("Access-Control-Allow-Origin", "*");
        }
    
    }


###add Access-Control-Allow-Origin to your existing RequestMapping

For every single RequestMapping that you want CORS to work with, one will have to
add something like `theHttpServletResponse.addHeader("Access-Control-Allow-Origin", "*");`
to said RequestMapping.


###example files

For more contextually complete examples, take a look at the following files from
the
[_spring-security-gwt-template_](https://github.com/jzerbe/spring-security-gwt-template)
example repo I put up on GitHub:

- [web.xml](https://github.com/jzerbe/spring-security-gwt-template/blob/master/WEB-INF/web.xml)
- [CorsController.java](https://github.com/jzerbe/spring-security-gwt-template/blob/master/src/com/vraidsys/server/CorsController.java)
- [FooController.java](https://github.com/jzerbe/spring-security-gwt-template/blob/master/src/com/vraidsys/server/FooController.java)
