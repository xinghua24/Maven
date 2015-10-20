## create a project
```text
mvn archetype:generate 
-DgroupId=com.mycompany.app
-DartifactId=my-app
-DarchetypeArtifactId=maven-archetype-quickstart
-DinteractiveMode=false
```

## clean and install a project in Maven local repository
clean phrase and isntall phrase are run
```text
mvn clean install
```

## run unit test on project
```text
mvn test
```

## generate javadoc
javadoc will be under target/site

```text
mvn javadoc:javadoc
