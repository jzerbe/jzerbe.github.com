---
layout: post
title: J2EE WAR boilerplate configuration for JDBC, JSP, JSTL, Servlets
tags:
- J2EE
- JDBC
- JSP
- JSTL
published: true
---
I found out the long way that building a properly working
[.war](http://en.wikipedia.org/wiki/WAR_file_format_(Sun))
is all about the correct archive structure. For this particular situation I
was deploying to a [Tomcat](http://en.wikipedia.org/wiki/Apache_Tomcat)
6 instance. This walk-through assumes you already have
a working web server/Servlet container with some way to deploy your .war.

To create a project that will deploy with support for
[JavaServer Pages](http://en.wikipedia.org/wiki/JavaServer_Pages),
[JavaServer Pages Standard Tag Library](http://en.wikipedia.org/wiki/JavaServer_Pages_Standard_Tag_Library),
[Servlets](http://en.wikipedia.org/wiki/Java_Servlet), and
[MySQL](http://en.wikipedia.org/wiki/MySQL) (via their custom
[JDBC driver](http://dev.mysql.com/downloads/connector/j/)),
you will need something like the following structure in place:

    j2ee_war/
        | -> build.xml
        | -> META-INF/
        | | -> context.xml
        | -> src/
        | | -> config/
        | | | -> DbConn.java
        | | -> database/
        | | | -> UserManager.java
        | | -> servlets/
        | | | -> MyInfo.java
        | -> war-no-include/
        | | -> jsp-api.jar
        | | -> servlet-api.jar
        | -> web/
        | | -> favicon.ico
        | | -> index.html
        | | -> login.jsp
        | -> WEB-INF/
        | | -> web.xml
        | | -> lib/
        | | | -> jstl-1.2.jar
        | | | -> mysql-connector-java-5.1.18-bin.jar

[j2ee_war.zip](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvWnlHNEFFa3BTNHM)


###NOTE
1) The libraries in the _war-no-include_ directory need to be in the build
class path when compiling beans and servlets, but should not be in the deployed war
as the servlet container provides its own version of these libraries. This is
taken care of in the included [ant](http://en.wikipedia.org/wiki/Apache_Ant)
`build.xml` file.

2) The _jstl-1.2.jar_ library (for JSTL support) contains both the API
and implementation. See
[BalusC\'s response to javax.servlet.ServletException: java.lang.NoClassDefFoundError: javax/servlet/jsp/jstl/core/ConditionalTagSupport](http://stackoverflow.com/a/8045562).
