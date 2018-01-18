---
title: 初学Gradle Task
date: 2018-01-09 09:58:41
tags: [gradle,task]
categories: 笔记
---
# 脚本默认task
Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]
<!-- more -->
Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'learn'.
components - Displays the components produced by root project 'learn'. [incubating]
dependencies - Displays all dependencies declared in root project 'learn'.
dependencyInsight - Displays the insight into a specific dependency in root project 'learn'.
dependentComponents - Displays the dependent components of components in root project 'learn'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'learn'. [incubating]
projects - Displays the sub-projects of root project 'learn'.
properties - Displays the properties of root project 'learn'.
tasks - Displays the tasks runnable from root project 'learn'.


# java 插件task

Build tasks
-----------
assemble - Assembles the outputs of this project. [jar]
build - Assembles and tests this project. [assemble, check]
buildDependents - Assembles and tests this project and all projects that depend on it. [build]
buildNeeded - Assembles and tests this project and all projects it depends on. [build]
classes - Assembles main classes.
    compileJava - Compiles main Java source.
    processResources - Processes main resources.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes. [classes]
testClasses - Assembles test classes. [classes]
    compileTestJava - Compiles test Java source.
    processTestResources - Processes test resources.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code. [classes]

Verification tasks
------------------
check - Runs all checks. [test]
test - Runs the unit tests. [classes, testClasses]

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

# springboot插件task

Application tasks
-----------------
bootRun - Run the project with support for auto-detecting main class and reloading static resources [classes]
    findMainClass
run - Runs this project as a JVM application [classes]
    findMainClass

Build tasks
-----------
assemble - Assembles the outputs of this project. [bootRepackage, distTar, distZip, jar]
bootRepackage - Repackage existing JAR and WAR archives so that they can be executed from the command line using 'java -jar' [distTar, distZip, jar]

Distribution tasks
------------------
assembleDist - Assembles the main distributions [distTar, distZip]
distTar - Bundles the project as a distribution. [jar]
    findMainClass
    startScripts - Creates OS specific scripts to run the project as a JVM application.
distZip - Bundles the project as a distribution. [jar]
    findMainClass
    startScripts - Creates OS specific scripts to run the project as a JVM application.
installDist - Installs the project as a distribution as-is. [jar]
    findMainClass
    startScripts - Creates OS specific scripts to run the project as a JVM application.

	
# eclipse插件task
IDE tasks
---------
cleanEclipse - Cleans all Eclipse files.
    cleanEclipseClasspath
    cleanEclipseJdt
    cleanEclipseProject
eclipse - Generates all Eclipse files.
    eclipseClasspath - Generates the Eclipse classpath file.
    eclipseJdt - Generates the Eclipse JDT settings file.
    eclipseProject - Generates the Eclipse project file.	
	
# 生命周期之间的执行顺序
test - Runs the unit tests. [classes, testClasses]	
中括号是他的前一阶段，执行他就会先执行前面的task，依次类推，就会找到他的执行顺序。
	
	


