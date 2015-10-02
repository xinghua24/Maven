# Maven

Maven is a project management tool base on Project Object Model(POM)

This project contains useful snippets and reference for working with Maven. 

### LifeCycle
[Intro to LifeCycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)

There are three build-in build lifecycles
* default lifecycle - build and deploy project
* clean lifecycle - clean up the directory where the the build files are located(usually target folder)
* site lifecycle - create the site

A lifecycle is make up of phrases

default lifecycle phases
* validate
* compile
* test
* package
* integration-test
* verify
* install
* deploy

clean lifecycle phase
* clean

site lifecycle phase
* site

if you call a build phase, it will execute all the phases prior to the called build phase and also the called build phase
**mvn install** will execute all phases in defaul lifecycle except deploy phase

### Plugin and Goal
A Build Phase is Made Up of Plugin Goals. A plugin goal represents a specific task. A build phase without any goals will not be executed. 

A plugin contains Goals. Plugins are make up of mojos(Maven plain Old Java Object). Each mojo is an executable goal in Maven

to execute a goal, run command **mvn** follow by pluginName:goalName.

Example: **mvn source:jar**  - bundle all source code in a jar file under Target directory, need to add maven source plugin to pom.xml for this goal to run.

Maven provides some standard plugins, when using the standard plugins, you don't need to provide the groupId *org.apache.maven.plugins* or version. Example **maven-assembly-plugin**
see [Maven Supported Plugins](http://maven.apache.org/plugins/index.html)

to configure a plugin see [Guide to Configuring Plug-ins](https://maven.apache.org/guides/mini/guide-configuring-plugins.html)

### Useful links

[Apache Maven Home](https://maven.apache.org/index.html)

[Apache Maven Start Guide](https://maven.apache.org/guides/getting-started/index.html)

[Mkyong Maven Tutorial](http://www.mkyong.com/tutorials/maven-tutorials/)

[Tutorialspoint Maven Tutorial](http://www.tutorialspoint.com/maven/)

[Maven by Example](https://books.sonatype.com/mvnex-book/reference/index.html)
