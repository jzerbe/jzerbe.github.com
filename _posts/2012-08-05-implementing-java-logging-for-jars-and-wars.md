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
[standard Java logger](http://docs.oracle.com/javase/6/docs/api/java/util/logging/Logger.html) -
`Logger.getLogger(MyCoolClass.class.getName()).log(Level.SEVERE, null, ex)` -
or the [Apache log4j logger](http://logging.apache.org/log4j/) -
`Logger.getLogger(MyCoolClass.class).error("error message")`.


###logging.properties

I have found that I much prefer the standard Java logger because of its many log
levels, tight integration with Tomcat, and out-of-the-box support. Maybe if log4j
worked out of the box without additional libraries needed, I might prefer it? In
any case, on to the code.

    <copy file="${src.dir}/logging.properties" tofile="${war.dir}/${web.inf.constant}/classes/logging.properties" />

I typically have a single line [Ant task](http://ant.apache.org/manual/tasksoverview.html)
like the above in some target that handles copying static assets. When creating
deployable WARs it is pretty easy to get the logging config into the right place,
the classpath; compare this to making sure your properties files are in the classpath
of a JAR (below).

    <path id="jar.classpath.ref">
        <fileset dir="${lib.dir}">
            <include name="*.jar" />
        </fileset>
        
        <fileset dir="${config.dir}">
            <include name="*.properties" />
        </fileset>
    </path>
    
    <!-- [Ant 1.7+] create the classpath for the jar -->
    <manifestclasspath property="jar.classpath" jarfile="${jar.file}">
        <classpath refid="jar.classpath.ref" />
    </manifestclasspath>
    <echo message="Jar Classpath = ${jar.classpath}"/>
    
    <!-- put everything from the created ${jar.dir} into the ${jar.file} file -->
    <jar jarfile="${jar.file}" basedir="${jar.dir}">
        <!-- create application entry point -->
        <manifest>
            <attribute name="Class-Path" value="${jar.classpath}" />
            <attribute name="Main-Class" value="${src.mainclass}" />
        </manifest>
        
        <!-- integrate the lib jars to form complete classpath -->
        <zipgroupfileset dir="${lib.dir}" includes="*.jar" />
    </jar>

What about an example logging.properties file?
This example was pulled from a webapp deployed to Tomcat 7.

    handlers = org.apache.juli.FileHandler
    ############################################################
    # Handler specific properties.
    # Describes specific configuration info for Handlers.
    ############################################################
    org.apache.juli.FileHandler.directory = ${catalina.base}/logs
    org.apache.juli.FileHandler.rotatable = true
    org.apache.juli.FileHandler.prefix = my_web_app.
    org.apache.juli.FileHandler.suffix = .log
    org.apache.juli.FileHandler.bufferSize = 0 #default to OS buf size
    org.apache.juli.FileHandler.level = SEVERE
    org.apache.juli.FileHandler.formatter = java.util.logging.SimpleFormatter
    com.mycompany.package.level=WARNING
    org.someothercompany.package.level=FINER

For debug builds, one could have another logging.properties file with the various
".level" lines set to FINEST or something.


###log4j.properties

While I am definitely no expert at logging, I have noticed that Apache\'s
log4j has several features that Sun\'s logger is missing. The big one is log
rotation based on filesize.I tested the following example against the old
[1.2 series](http://logging.apache.org/log4j/1.2/) of log4j, but
don\'t forget to check out the
[2.x series](http://logging.apache.org/log4j/2.x/).
The procedure is the same for getting the log4j.properties file into the classpath
of your JAR or WAR as covered in the logging.properties section.
Now for an example log4j.properties file!
[Inspiration source](http://snippets.dzone.com/snippets/log4jproperties-eclipse-log4j).

    # Capture stdout
    log4j.appender.O=org.apache.log4j.ConsoleAppender
    # Set-Up file output
    log4j.appender.MyPackageName=org.apache.log4j.RollingFileAppender
    ...
    log4j.appender.MyPackageName.File=ControllerReport.log
    ...
    # Maximum log file size
    log4j.appender.MyPackageName.MaxFileSize=100KB
    ...
    # Archival number (past original log)
    log4j.appender.MyPackageName.MaxBackupIndex=1
    ...
    # Filename patterns
    log4j.appender.MyPackageName.layout=org.apache.log4j.PatternLayout
    ...
    log4j.appender.O.layout=org.apache.log4j.PatternLayout
    log4j.appender.MyPackageName.layout.ConversionPattern=[%d{ISO8601}]%5p%6.6r[%t]%x - %C.%M(%F:%L) - %m%n
    ...
    log4j.appender.O.layout.ConversionPattern=[%d{ISO8601}]%5p%6.6r[%t]%x - %C.%M(%F:%L) - %m%n
    # Categories
    log4j.category.mypackage.controller=DEBUG, MyPackageName
    ...
    log4j.rootLogger=DEBUG, O
