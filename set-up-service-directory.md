# How to set up a Service-Directory

This guide covers how to set up your own Service-Directory which is the central component that makes your Middleware work.

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](./middleware-concept-introduction.md) to the EFS-middleware first.
- You have experience with building micro services using the Spring Boot framework.

**Table of Contents:**
TODO




## Create a new maven project

Set up a new maven project with your favorite tool or IDE.

## pom.xml

Open the pom.xml and insert the following parent-tag and dependencies:

```xml
<parent>
    <groupId>com.github.keim-hs-esslingen.efs</groupId>
    <artifactId>efs-service-parent</artifactId>
    <version>1.0.1</version>
    <relativePath/> <!-- skip parent lookup in parent directory. -->
</parent>

<dependencies>
    <dependency>
        <groupId>com.github.keim-hs-esslingen.efs</groupId>
        <artifactId>service-directory-plugin</artifactId>
    </dependency>
</dependencies>
```
The parent POM contains information about which versions the required dependencies need to have, so you only need to worry about setting the version number of the parent project.

The service-directory-plugin is built around the Spring Boot framework. The framework is already included in the service-directory-plugin as a transitive dependency. However you still need to provide your own configuration using an application.yml file.

## application.yml

Create and/or open your application.yml file and add the following properties to it:

```yml
spring:
  application:
    name: service-directory # Define a name for your service-directory

server:
  port: 9900 # Configure a port for your service

eureka:
  client:
    service-url:
      defaultZone: http://localhost:${server.port}/eureka/
```

As you can see, the middleware uses Netflix Eureka Service-Discovery component to make services discoverable in the middleware network as well as checking their health status.

The last thing you need to do is to create a startup java class for your spring boot application. For this purpose, create a new java file in any package you like (except the default package), and enter the following content.

## ServiceDirectoryApplication.java

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class ServiceDirectoryApplication {

	public static void main(String[] args) {
		SpringApplication.run(ServiceDirectoryApplication.class, args);
	}

}
```

Having done this, you Service-Directory is ready to be launched.
