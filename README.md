# servlet-helloworld-maven
Just a servlet example in 2024!

## Prerequisites: Setup your Environment
### Install SDKMAN
See [SDKMAN installation instructions](https://sdkman.io/install). Also check out [baeldung's tutorial on SDKMAN](https://www.baeldung.com/java-sdkman-intro).

### Install Java
Here, we installed Java 17 from Amazon Coretto
```
sdk install java 17.0.10-amzn
```

### Install Maven using SDKMAN
```bash
# install maven
sdk install maven

# check maven version
mvn --version
```

### Install Tomcat using SDKMAN
```bash
# install tomcal
sdk install tomcat

# check out the tomcat installation directory under ~/.sdkman
~ tree -L 2 ~/.sdkman/candidates/tomcat/
~/.sdkman/candidates/tomcat/
├── 10.1.14
│   ├── BUILDING.txt
│   ├── CONTRIBUTING.md
│   ├── LICENSE
│   ├── NOTICE
│   ├── README.md
│   ├── RELEASE-NOTES
│   ├── RUNNING.txt
│   ├── bin
│   ├── conf
│   ├── lib
│   ├── logs
│   ├── temp
│   ├── webapps
│   └── work
└── current -> 10.1.14
```

## Setup the project
### Use maven to generate the project
```bash
mvn archetype:generate -DgroupId=com.mycompany.hello -DartifactId=servlet-helloworld-maven -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```
### Add war packaging in Maven
```xml
  <groupId>com.mycompany.hello</groupId>
  <artifactId>servlet-helloworld-maven</artifactId>
  <version>1.0-SNAPSHOT</version>
  <!-- add the line below to your pom.xml -->
  <packaging>war</packaging>
```
### Add Jakarta Servlet API as a Maven dependency
```xml
<dependency>
  <groupId>jakarta.servlet</groupId>
  <artifactId>jakarta.servlet-api</artifactId>
  <version>6.0.0</version>
  <scope>provided</scope>
</dependency>
```

## Setup Tomcat
### Make the scripts executable
```
chmod +x ~/.sdkman/candidates/tomcat/10.1.14/bin/*.sh
```
### Setup users in Tomcat
Add the following roles and users to `~/.sdkman/candidates/tomcat/10.1.14/conf/tomcat-users.xml`
```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <user username="manager" password="password" roles="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="admin" password="password" roles="admin-gui"/>
</tomcat-users>
```
### Allow access to the Manager web app
1. Update the `Context` section and comment following lines in `~/.sdkman/candidates/tomcat/10.1.14/webapps/host-manager/META-INF/context.xml`
```xml
<Context antiResourceLocking="false" privileged="true" >
<!--
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```
2. Update the `Context` section and comment following lines in `~/.sdkman/candidates/tomcat/10.1.14/webapps/manager/META-INF/context.xml`
```xml
<Context antiResourceLocking="false" privileged="true" >
<!--
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
  -->
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```
Use the following to start/stop Tomcat
```bash
# start tomcat
~/.sdkman/candidates/tomcat/10.1.14/bin/startup.sh
# stop tomcat
~/.sdkman/candidates/tomcat/10.1.14/bin/shutdown.sh
```
## Deploy the servlet
### Start tomcat
```
~/.sdkman/candidates/tomcat/10.1.14/bin/startup.sh
```
Browse to http://localhost:8080

### Package the war file
```
mvn war:war
```
The following war file will be generated
```
~/.sdkman/candidates/tomcat/10.1.14/webapps/servlet-helloworld-maven-1.0-SNAPSHOT.war
```
### Deploy the war file

Use Tomcat's Manager web interface to deploy the war file.

TODO: PUT SCREENSHOTS HERE

### Visit the servlet
Browse the following to check our servlet
```
http://localhost:8080/servlet-helloworld-maven-1.0-SNAPSHOT/hello
```
TODO: PUT SCREENSHOT HERE

## References
### Tutorials by Baeldung
* https://www.baeldung.com/java-sdkman-intro
* https://www.baeldung.com/intro-to-servlets
* https://www.baeldung.com/java-servlets-containers-intro
* https://www.baeldung.com/maven-generate-war-file

### Maven Tutorial
* https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html
