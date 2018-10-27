<!-- TOC -->

- [Maven Core Concept](#maven-core-concept)
    - [LifeCycle](#lifecycle)
    - [Plugin and Goal](#plugin-and-goal)
- [Useful Commands](#useful-commands)
- [Core Maven Plugins](#core-maven-plugins)
    - [Clean Plugin](#clean-plugin)
    - [Resources Plugin](#resources-plugin)
    - [Surefire Plugin](#surefire-plugin)
- [Useful links](#useful-links)

<!-- /TOC -->

# Maven Core Concept

Maven is a project management tool base on Project Object Model(POM)

This project contains useful snippets and reference for working with Maven. 

* A **lifecycle** is make up of **phrases**.


## LifeCycle
[Intro to LifeCycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

There are three build-in build lifecycles
* default lifecycle - build and deploy project
* clean lifecycle - clean up the directory where the the build files are located(usually target folder)
* site lifecycle - create the site

A **lifecycle** is make up of **phrases**.
A Build **Phase** is Made Up of Plugin **Goals**.

**default lifecycle's phases**<br>
* validate - validate the project is correct and all necessary information is available
* process-sources - process the source code
* compile - compile the source code of the project
* process-test-resources - process the test source code
* test-compile - compile the test source code into the test destination directory
* test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
* package - take the compiled code and package it in its distributable format, such as a JAR.
* verify - run any checks on results of integration tests to ensure quality criteria are met
* install - install the package into the local repository, for use as a dependency in other projects locally
* deploy - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

complete list see [Default Lifecycle Reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)

**clean lifecycle's phase**<br>
* pre-clean
* clean
* post-clean

**site lifecycle's phase**<br>
* pre-site
* site
* post-site
* site-deploy

**default lifecycle's bindings**<br>
| Default Lifecycle Phrase | Plugin Binding                               |
|--------------------------|----------------------------------------------|
| generate-resources       | plugin:descriptor                            |
| process-resources        | resources:resources                          |
| compile                  | compiler:compile                             |
| process-test-resources   | resources:testResources                      |
| test-compile             | compiler:testCompile                         |
| test                     | surefire:test                                |
| package                  | jar:jar and plugin:addPluginArtifactMetadata |
| install                  | install:install                              |
| deploy                   | deploy:deploy                                |



## Plugin and Goal
A Build Phase is Made Up of Plugin Goals. A plugin goal represents a specific task. A build phase without any goals will not be executed. 

A plugin contains Goals. Plugins are make up of mojos(Maven plain Old Java Object). Each mojo is an executable goal in Maven

to execute a goal, run command **mvn** follow by pluginName:goalName.

Example: **mvn source:jar**  - bundle all source code in a jar file under Target directory, need to add maven source plugin to pom.xml for this goal to run.

Maven provides some standard plugins, when using the standard plugins, you don't need to provide the groupId *org.apache.maven.plugins* or version. Example **maven-assembly-plugin**
see [Maven Supported Plugins](http://maven.apache.org/plugins/index.html)

to configure a plugin see [Guide to Configuring Plug-ins](https://maven.apache.org/guides/mini/guide-configuring-plugins.html)

# Useful Commands

if you call a build phase, it will execute all the phases prior to the called build phase and also the called build phase. 
For Example **mvn install** will execute all phases in defaul lifecycle except deploy phase.

You can not execute a lifecycle directly, you can only execute a phase or a plugin. So when you execute command **mvn clean**, 
the pre-client and clean phase are actually called, not the lifecycle.

**compile, test, package or install project**<br>
```sh
mvn clean compile

mvn clean test

mvn package

# install project in local repo
mvn clean install

# to print debug info, add -X flag
mvn clean test -X
```


**generate javadoc**<br>
```text
mvn javadoc:javadoc
```

**get source**<br>
```sh
# download source
mvn dependency:sources

# download javadoc
mvn dependency:resolve -Dclassifier=javadoc
```

# Core Maven Plugins
* [Baeldung maven plugins](https://www.baeldung.com/core-maven-plugins)
    * [Resource plugin](https://www.baeldung.com/maven-resources-plugin)
    * [Surefire plugin](https://www.baeldung.com/maven-surefire-plugin)

## Clean Plugin
[Clean plugin Homepage](https://maven.apache.org/plugins/maven-clean-plugin/)

The Maven Clean Plugin will delete the target directory by default. 


## Resources Plugin
[Resources plugin Homepage](https://maven.apache.org/plugins/maven-resources-plugin/)

The Resources Plugin handles the copying of project resources to the output directory. There are two different kinds of resources:
 main resources and test resources. The difference is that the main resources are the resources associated to the main source code 
 while the test resources are associated to the test source code.



There are 3 goals for this plugin
* resources – copy resources that are part of the main source code to the main output directory
* testResources – copy resources that are part of the test source code to the test output directory
* copy-resources – copy arbitrary resource files to an output directory, requiring us to specify the input files and the output directory


By default only resource in src/main/resources directory is copied to target/classes. additional resources can be added by specifying resource directories.
Same for target/test-classes.
```xml
	<build>
		<resources>
			<resource>
				<directory>src/test/resources</directory>
			</resource>
			<resource>
				<directory>my-resources</directory>
			</resource>
		</resources>
		<testResources>
			<testResource>
				<directory>my-resources</directory>
				<excludes>
					<exclude>**/*.bmp</exclude>
					<exclude>**/*.jpg</exclude>
					<exclude>**/*.jpeg</exclude>
					<exclude>**/*.gif</exclude>
				</excludes>
			</testResource>
		</testResources>
        <!-- other configs -->
	</build>
```

You can use resources plugin's copy-resources goal to move resource to an arbitrary directory
```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-resources-plugin</artifactId>
	<version>3.1.0</version>
	<executions>
		<execution>
			<id>copy-resources</id>
			<phase>validate</phase>
			<goals>
				<goal>copy-resources</goal>
			</goals>
			<configuration>
				<outputDirectory>${basedir}/target/special-resources</outputDirectory>
				<resources>
					<resource>
						<directory>my-resources</directory>
						<filtering>true</filtering>
					</resource>
				</resources>
			</configuration>
		</execution>
	</executions>
</plugin>
```

## Surefire Plugin
[Maven Surefire Plugin Homepage](https://maven.apache.org/surefire/maven-surefire-plugin/)

Test phrase binds to surefire plugin's `test` goal. It generates reports in two different file formats:

Plain text files (*.txt)
* XML files (*.xml)
* By default, these files are generated in ${basedir}/target/surefire-reports/TEST-*.xml.

By default, these files are generated in ${basedir}/target/surefire-reports/TEST-*.xml.


# Useful links
* [Apache Maven Home](https://maven.apache.org/index.html)
* [Apache Maven Guide](https://maven.apache.org/guides/getting-started/index.html)
    * [Intro to Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
    * [Config plugin](https://maven.apache.org/guides/mini/guide-configuring-plugins.html)
    * [Built-in Lifecycle Bindings](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings)
* [Mkyong Maven Tutorial](http://www.mkyong.com/tutorials/maven-tutorials/)
* [Tutorialspoint Maven Tutorial](http://www.tutorialspoint.com/maven/)
* [Maven by Example](https://books.sonatype.com/mvnex-book/reference/index.html)
