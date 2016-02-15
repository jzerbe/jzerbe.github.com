---
layout: post
title: use Node package version information in Ant script
tags:
- Node.js
- Node Package Manager
- Apache Ant
published: true
---
Ant supports inline JavaScript via [Mozilla\'s Rhino engine](https://developer.mozilla.org/en-US/docs/Rhino),
which comes embedded in J2SE 6. Combined with [Ant-Contrib](http://ant-contrib.sourceforge.net/), I was able to
pull in version information from [NPM](https://npmjs.org/) and from package manifests.


### Ant Script setup

1. Link in the Ant-Contrib tasks: `<taskdef resource="net/sf/antcontrib/antlib.xml" classpath="no-include-war/ant-contrib.jar" />`
2. Map the NPM and Shell executables to Ant properties for easier reuse:
    `<property name="npm.executable" value="npm" />`, `<property name="shell.executable" value="sh" />`
3. Store off some `sed` pipes for later use:
    `<property name="sed.filter.version" value="| sed -n '2p' | sed 's/[^0-9.]*//g'" />`. What this does is filter out
    everything but the second returned line, and then filter out everything but numeric values and the `.` character.


### Get package manifest version information

Do note that the defined JavaScript methods can only be used within the same enclosing `script` element.

    <target name="node-modules.version.pack">
        <loadfile property="package.raw" srcFile="package.json" />
        <script language="javascript"> <![CDATA[
            function contains(theHaystack, theNeedle) {
                return (theHaystack.indexOf(theNeedle) > -1);
            }
            
            function getPackageVersion(theStr) {
                var aSplitStr = " ";
                if (contains(theStr, "~")) {
                    aSplitStr = "~";
                } else if (contains(theStr, "#")) {
                    aSplitStr = "#";
                }
                
                var aStrArray = theStr.split(aSplitStr);
                var rawVersionStr = aStrArray[(aStrArray.length - 1)];
                
                var majorVersionStr = rstrip(rawVersionStr, "-");
                
                return majorVersionStr.replace(/[^0-9.]*/g, '');
            }
            
            function rstrip(theStr, theSplitStr) {
                var aStrArray = theStr.split(theSplitStr);
                var secondToLastElement = aStrArray.length - 2;
                if (secondToLastElement < 0)
                    secondToLastElement = 0;
                return aStrArray[secondToLastElement];
            }
            
            var package_raw = project.getProperty("package.raw");
            var package_obj = eval("(" + package_raw + ")");
            
            for (var prop in package_obj.dependencies) {
                if (package_obj.dependencies.hasOwnProperty(prop)){
                    var antPropName = "package.dependencies." + prop + ".version";
                    var antPropValue = getPackageVersion(package_obj.dependencies[prop]);
                    project.setProperty(antPropName, antPropValue);
                }
            }
        ]]> </script>
    </target>

Each version string from the `dependencies` section of the `package.json` manifest file is now available
for use as an Ant property by the name of `package.dependencies.depItem.version`. You may need to tweak the
`getPackageVersion` function to suit your exact needs, whether this be internal URLs or mainline Node modules.


### Get installed package versions and compare

Now that the dependency information has been pulled in from the `package.json` manifest, loop over
all the defined dependencies with `propertyselector` (Ant-Contrib) and check the locally installed version.
Normally Ant properties are immutable, but with
[`var` from Ant-Contrib](http://ant-contrib.sourceforge.net/tasks/tasks/variable_task.html),
it is possible to skirt around this restriction.

    <target name="node-modules.version.local" depends="node-modules.version.pack">
        <property name="hasRequiredPackageVersions" value="true" />
        
        <propertyselector property="package.list"
                          delimiter=","
                          match="package\.dependencies\.([^\.]*)\.version"
                          select="\1" casesensitive="false" />
        
        <for list="${package.list}" param="package.name.invariant" delimiter=",">
            <sequential>
                <var name="package.name" unset="true" />
                <property name="package.name" value="@{package.name.invariant}" />
                
                <var name="versionLocalVal" unset="true" />
                <exec executable="${shell.executable}" failonerror="true" outputproperty="versionLocalVal">
                    <arg value="-c" />
                    <arg value="${npm.executable} list ${package.name} version ${sed.filter.version}" />
                </exec>
                
                <var name="hasRequiredVersion" unset="true" />
                <script language="javascript"> <![CDATA[
                    var moduleName = project.getProperty("package.name");
                    var versionLocal = "local.dependencies." + moduleName + ".version";
                    var versionPackage = "dependencies." + moduleName + ".version";
                    var versionPackageVal = project.getProperty(versionPackage);
                    
                    println(versionLocal + " = " + versionLocalVal);
                    println(versionPackage + " = " + versionPackageVal);
                    println("");
                    
                    var hasRequiredVersion = (versionLocalVal != "") && (versionLocalVal != "null")
                        && (versionPackageVal != ".")
                        && (versionLocalVal >= versionPackageVal);
                    project.setProperty("hasRequiredVersion", hasRequiredVersion);
                ]]> </script>
                <if>
                    <isfalse value="${hasRequiredVersion}" />
                    <then>
                        <var name="hasRequiredPackageVersions" value="false" />
                    </then>
                </if>
            </sequential>
        </for>
    </target>

After this finishes running, `hasRequiredPackageVersions` will only remain `true` if all
dependencies are already installed.
