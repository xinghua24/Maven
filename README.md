<!-- TOC -->

- [Maven Core Concept](#maven-core-concept)
    - [LifeCycle](#lifecycle)
    - [Plugin and Goal](#plugin-and-goal)
    - [Properties](#properties)
        - [Maven Project Properties](#maven-project-properties)
        - [Maven Settings Properties](#maven-settings-properties)
        - [Environment Variable Properties](#environment-variable-properties)
- [Useful Commands](#useful-commands)
- [Core Maven Plugins](#core-maven-plugins)
    - [Clean Plugin](#clean-plugin)
    - [Resources Plugin](#resources-plugin)
    - [Compiler Plugin](#compiler-plugin)
    - [Surefire Plugin](#surefire-plugin)
    - [Deploy Plugin](#deploy-plugin)
    - [Maven Dependency Plugin](#maven-dependency-plugin)
    - [Maven Jar Plugin](#maven-jar-plugin)
    - [Assembly Plugin](#assembly-plugin)
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

**default lifecycle phase's bindings**<br>
* generate-resources         -->    plugin:descriptor                            
* process-resources          -->    resources:resources                          
* compile                    -->    compiler:compile                             
* process-test-resources     -->    resources:testResources                      
* test-compile               -->    compiler:testCompile                         
* test                       -->    surefire:test                                
* package                    -->    jar:jar and plugin:addPluginArtifactMetadata 
* install                    -->    install:install                              
* deploy                     -->    deploy:deploy                                



## Plugin and Goal
A Build Phase is Made Up of Plugin Goals. A plugin goal represents a specific task. A build phase without any goals will not be executed. 

A plugin contains Goals. Plugins are make up of mojos(Maven plain Old Java Object). Each mojo is an executable goal in Maven

to execute a goal, run command **mvn** follow by pluginName:goalName.

Example: **mvn source:jar**  - bundle all source code in a jar file under Target directory, need to add maven source plugin to pom.xml for this goal to run.

Maven provides some standard plugins, when using the standard plugins, you don't need to provide the groupId *org.apache.maven.plugins* or version. Example **maven-assembly-plugin**
see [Maven Supported Plugins](http://maven.apache.org/plugins/index.html)

to configure a plugin see [Guide to Configuring Plug-ins](https://maven.apache.org/guides/mini/guide-configuring-plugins.html)


## Properties
For more info on Properties see [Maven: The Complete Reference - Maven Properties](https://books.sonatype.com/mvnref-book/reference/resource-filtering-sect-properties.html)'
You can use Maven properties in a pom.xml file or in any resource that is being processed by the Maven Resource plugin’s filtering features. A property is always surrounded by ${ and }. e.g ${project.groupId}

Maven project has implicit properties, you can also have user-defined properties.


### Maven Project Properties
use project.* to reference values in a Maven Pom. e.g ${project.groupId}, ${project.version}, ${project.build.sourceDirectory}

* ${project.basedir} - The directory that the current project resides in
* ${project.build.sourceDirectory} - ${project.basedir}/src/main/java
* ${project.build.directory} - ${project.basedir}/target

see suport POM for the list of pre-defined variable. [pom](https://github.com/apache/maven/blob/trunk/maven-model-builder/src/main/resources/org/apache/maven/model/pom-4.0.0.xml#L53)


### Maven Settings Properties
You can also reference any properties in the Maven Local Settings file which is usually stored in ~/.m2/settings.xml. prefix is settings.*


### Environment Variable Properties
Environment variables can be referenced with the env.* prefix.


# Useful Commands

to generate a maven quick start project
```sh
mvn archetype:generate -DgroupId={project-packaging} -DartifactId={project-name} -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

# example
mvn archetype:generate -DgroupId=com.xinghua24 -DartifactId=HelloProject -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

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


## Clean Plugin
[Clean plugin Homepage](https://maven.apache.org/plugins/maven-clean-plugin/)

The Maven Clean Plugin will delete the target directory by default. 

you can also delete additional directories
```xml
<plugin>
	<artifactId>maven-clean-plugin</artifactId>
	<version>3.0.0</version>
	<configuration>
		<filesets>
			<fileset>
				<directory>${basedir}/output-resources</directory>
			</fileset>
		</filesets>
	</configuration>
</plugin>
```


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


## Compiler Plugin
[Maven Compiler plugin Homepage](https://maven.apache.org/plugins/maven-compiler-plugin/)

two goals
* compiler:compile is bound to the compile phase and is used to compile the main source files.
* compiler:testCompile is bound to the test-compile phase and is used to compile the test source files.

if you want the compiler to use java 8 feature and compile to JVM 1.8, set the source and target parameters.
```xml
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.0</version>
				<configuration>
					<source>${java.version}</source>
					<target>${java.version}</target>
				</configuration>
			</plugin>
		</plugins>
```



## Surefire Plugin
[Maven Surefire Plugin Homepage](https://maven.apache.org/surefire/maven-surefire-plugin/)

Test phrase binds to surefire plugin's `test` goal. It generates reports in two different file formats:

Plain text files (*.txt)
* XML files (*.xml)
* By default, these files are generated in ${basedir}/target/surefire-reports/TEST-*.xml.

By default, these files are generated in ${basedir}/target/surefire-reports/TEST-*.xml.



## Deploy Plugin
[Deploy Plugin Homepage](https://maven.apache.org/plugins/maven-deploy-plugin/)

The deploy plugin is primarily used during the deploy phase, to add your artifact(s) to a remote repository for sharing with other developers and projects.


Two goals
* deploy:deploy is used to automatically install the artifact, its pom and the attached artifacts produced by a particular project. Most if not all of the information related to the deployment is stored in the project's pom.
* deploy:deploy-file is used to install a single artifact along with its pom. In that case the artifact information can be taken from an optionally specified pomFile, but can be completed/overriden using the command line.

## Maven Dependency Plugin
[Maven Dependency Plugin Homepage](https://maven.apache.org/plugins/maven-dependency-plugin/)

The dependency plugin provides the capability to manipulate artifacts. It can copy and/or unpack artifacts from local or remote repositories to a specified location.


## Maven Jar Plugin
[Maven Jar Plugin Homepage](https://maven.apache.org/plugins/maven-jar-plugin/)

This plugin provides the capability to build jars. If you like to sign jars please use the Maven Jarsigner Plugin.

goals
* jar:jar create a jar file for your project classes inclusive resources.
* jar:test-jar create a jar file for your project test classes .


## Assembly Plugin
[Maven Assembly Plugin Homepage](https://maven.apache.org/plugins/maven-assembly-plugin/)

it can be used to create distributions. it supports a lot of formats such as zip, tar.gz, jar etc.

**What is Assembly?**<br>
An "assembly" is a group of files, directories, and dependencies that are assembled into an archive format and distributed.

The main goal in the assembly plugin is the single goal. It is used to create all assemblies.

see [example](https://stackoverflow.com/questions/2514429/creating-a-zip-archive-of-the-maven-target-directory)
on how to create zip


# Useful links
* [Apache Maven Home](https://maven.apache.org/index.html)
* [Book Maven: The Complete Reference](https://books.sonatype.com/mvnref-book/reference/index.html)
* [Apache Maven Guide](https://maven.apache.org/guides/getting-started/index.html)
    * [Intro to Build Lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
    * [Config plugin](https://maven.apache.org/guides/mini/guide-configuring-plugins.html)
    * [Built-in Lifecycle Bindings](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Built-in_Lifecycle_Bindings)
* [Mkyong Maven Tutorial](http://www.mkyong.com/tutorials/maven-tutorials/)
* [Tutorialspoint Maven Tutorial](http://www.tutorialspoint.com/maven/)
* [Maven by Example](https://books.sonatype.com/mvnex-book/reference/index.html)
* [Baeldung maven plugins](https://www.baeldung.com/core-maven-plugins)


