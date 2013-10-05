---
layout: post
title: implementing Java logging for JARs and WARs
tags:
- Java
- JAR
- WAR
- logging
- juli
- log4j
- properties
published: true
---
This covers implementing a build that includes configuration for the
<a href="http://docs.oracle.com/javase/6/docs/api/java/util/logging/Logger.html">standard Java logger</a>
- <code>Logger.getLogger(MyCoolClass.class.getName()).log(Level.SEVERE, null, ex)</code>
- or the <a href="http://logging.apache.org/log4j/">Apache log4j logger</a>
- <code>Logger.getLogger(MyCoolClass.class).error("error message")</code>.<br />
<br />

<strong>logging.properties</strong><br />
I have found that I much prefer the standard Java logger because of its many log
levels, tight integration with Tomcat, and out-of-the-box support. Maybe if log4j
worked out of the box wtihout additional libraries needed, I might prefer it? In
any case, on to the code.
<blockquote>
<code>&lt;copy file=&quot;${src.dir}/logging.properties&quot; tofile=&quot;${war.dir}/${web.inf.constant}/classes/logging.properties&quot; /&gt;</code>
</blockquote>
I typically have a single line <a href="http://ant.apache.org/manual/tasksoverview.html">Ant task</a>
like the above in some target that handles copying static assets. When creating
deployable WARs it is pretty easy to get the logging config into the right place,
the classpath; compare this to making sure your properties files are in the classpath
of a JAR (below).
<blockquote>
<code>
        &lt;path id=&quot;jar.classpath.ref&quot;&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;fileset dir=&quot;${lib.dir}&quot;&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;include name=&quot;*.jar&quot; /&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;/fileset&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;fileset dir=&quot;${config.dir}&quot;&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;include name=&quot;*.properties&quot; /&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;/fileset&gt;<br />
        &lt;/path&gt;<br />
        <br />
        &lt;!-- [Ant 1.7+] create the classpath for the jar --&gt;<br />
        &lt;manifestclasspath property=&quot;jar.classpath&quot; jarfile=&quot;${jar.file}&quot;&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;classpath refid=&quot;jar.classpath.ref&quot; /&gt;<br />
        &lt;/manifestclasspath&gt;<br />
        &lt;echo message=&quot;Jar Classpath = ${jar.classpath}&quot;/&gt;<br />
        <br />
        &lt;!-- put everything from the created ${jar.dir} into the ${jar.file} file --&gt;<br />
        &lt;jar jarfile=&quot;${jar.file}&quot; basedir=&quot;${jar.dir}&quot;&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;!-- create application entry point --&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;manifest&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;attribute name=&quot;Class-Path&quot; value=&quot;${jar.classpath}&quot; /&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&lt;attribute name=&quot;Main-Class&quot; value=&quot;${src.mainclass}&quot; /&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;/manifest&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;!-- integrate the lib jars to form complete classpath --&gt;<br />
        &nbsp;&nbsp;&nbsp;&nbsp;&lt;zipgroupfileset dir=&quot;${lib.dir}&quot; includes=&quot;*.jar&quot; /&gt;<br />
        &lt;/jar&gt;<br />
</code>
</blockquote>
What about an example logging.properties file?
This example was pulled from a webapp deployed to Tomcat 7.
<blockquote>
<code>
handlers = org.apache.juli.FileHandler<br />
<br />
############################################################<br />
# Handler specific properties.<br />
# Describes specific configuration info for Handlers.<br />
############################################################<br />
<br />
org.apache.juli.FileHandler.directory = ${catalina.base}/logs<br />
org.apache.juli.FileHandler.rotatable = true<br />
org.apache.juli.FileHandler.prefix = my_web_app.<br />
org.apache.juli.FileHandler.suffix = .log<br />
org.apache.juli.FileHandler.bufferSize = 0 #default to OS buf size<br />
org.apache.juli.FileHandler.level = SEVERE<br />
org.apache.juli.FileHandler.formatter = java.util.logging.SimpleFormatter<br />
<br />
com.mycompany.package.level=WARNING<br />
org.someothercompany.package.level=FINER<br />
</code>
</blockquote>
For debug builds, one could have another logging.properties file with the various
&quot;.level&quot; lines set to FINEST or something.<br />
<br />

<strong>log4j.properties</strong><br />
While I am definately no expert at logging, I have noticed that Apache&#39;s
log4j has several features that Sun&#39;s logger is missing. The big one is log
rotation based on filesize.I tested the following example against the old
<a href="http://logging.apache.org/log4j/1.2/">1.2 series</a> of log4j, but
don&#39;t forget to check out the
<a href="http://logging.apache.org/log4j/2.x/">2.x series</a>.
The procedure is the same for getting the log4j.properties file into the classpath
of your JAR or WAR as covered in the logging.properties section.
Now for an example log4j.properties file!
<a href="http://snippets.dzone.com/snippets/log4jproperties-eclipse-log4j">Inspiration source</a>.
<blockquote>
<code>
# Capture stdout<br />
log4j.appender.O=org.apache.log4j.ConsoleAppender<br />
<br />
# Set-Up file output<br />
log4j.appender.MyPackageName=org.apache.log4j.RollingFileAppender<br />
...<br />
<br />
log4j.appender.MyPackageName.File=ControllerReport.log<br />
...<br />
<br />
# Maximum log file size<br />
log4j.appender.MyPackageName.MaxFileSize=100KB<br />
...<br />
<br />
# Archival number (past original log)<br />
log4j.appender.MyPackageName.MaxBackupIndex=1<br />
...<br />
<br />
# Filename patterns<br />
log4j.appender.MyPackageName.layout=org.apache.log4j.PatternLayout<br />
...
<br />
log4j.appender.O.layout=org.apache.log4j.PatternLayout<br />
<br />
log4j.appender.MyPackageName.layout.ConversionPattern=[%d{ISO8601}]%5p%6.6r[%t]%x - %C.%M(%F:%L) - %m%n<br />
...<br />
log4j.appender.O.layout.ConversionPattern=[%d{ISO8601}]%5p%6.6r[%t]%x - %C.%M(%F:%L) - %m%n<br />
<br />
# Categories<br />
log4j.category.mypackage.controller=DEBUG, MyPackageName<br />
...<br />
log4j.rootLogger=DEBUG, O<br />
</code>
</blockquote>
