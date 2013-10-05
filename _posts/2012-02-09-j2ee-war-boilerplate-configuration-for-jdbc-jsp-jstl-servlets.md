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
<a href="http://en.wikipedia.org/wiki/WAR_file_format_(Sun)">.war</a>
is all about the correct archive structure. For this particular situation I
was deploying to a <a href="http://en.wikipedia.org/wiki/Apache_Tomcat">Tomcat</a>
6 instance. This walk-through assumes you already have
a working web server/Servlet container with some way to deploy your .war.<br />
<br />
To create a project that will deploy with support for
<abbr title="JavaServer Pages">
    <a href="http://en.wikipedia.org/wiki/JavaServer_Pages">JSP</a>
</abbr>,
<abbr title="JavaServer Pages Standard Tag Library">
    <a href="http://en.wikipedia.org/wiki/JavaServer_Pages_Standard_Tag_Library">JSTL</a>
</abbr>,
<a href="http://en.wikipedia.org/wiki/Java_Servlet">Servlets</a>, and
<a href="http://en.wikipedia.org/wiki/MySQL">MySQL</a> (via their custom
<a href="http://dev.mysql.com/downloads/connector/j/">JDBC driver</a>),
you will need something like the following structure in place:<br />
<br />
<code>- j2ee_war/<br />
    | -&gt; build.xml<br />
    | -&gt; META-INF/<br />
    | | -&gt; context.xml<br />
    | -&gt; src/<br />
    | | -&gt; config/<br />
    | | | -&gt; DbConn.java<br />
    | | -&gt; database/<br />
    | | | -&gt; UserManager.java<br />
    | | -&gt; servlets/<br />
    | | | -&gt; MyInfo.java<br />
    | -&gt; war-no-include/<br />
    | | -&gt; jsp-api.jar<br />
    | | -&gt; servlet-api.jar<br />
    | -&gt; web/<br />
    | | -&gt; favicon.ico<br />
    | | -&gt; index.html<br />
    | | -&gt; login.jsp<br />
    | -&gt; WEB-INF/<br />
    | | -&gt; web.xml<br />
    | | -&gt; lib/<br />
    | | | -&gt; jstl-1.2.jar<br />
    | | | -&gt; mysql-connector-java-5.1.18-bin.jar<br />
</code><br />
[<a href="https://docs.google.com/file/d/0B0yT30uCaFvvWnlHNEFFa3BTNHM/edit?pli=1">j2ee_war.zip</a>]<br />
<br />
<strong>NOTE</strong>:<br />
1) The libraries in the &quot;war-no-include&quot; directory need to be in the build
class path when compiling beans and servlets, but should not be in the deployed war
as the servlet container provides its own version of these libraries. This is
taken care of in the included <a href="http://en.wikipedia.org/wiki/Apache_Ant">ant</a>
build.xml file.<br />
2) The &quot;jstl-1.2.jar&quot; library (for JSTL support) contains both the API
and implementation. See <a href="http://stackoverflow.com/a/8045562">BalusC's
    response to javax.servlet.ServletException: java.lang.NoClassDefFoundError:
    javax/servlet/jsp/jstl/core/ConditionalTagSupport</a>.
