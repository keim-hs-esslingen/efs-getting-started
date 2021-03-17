# Use the middleware core library to create a consumer adapter service

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](Introduction-to-the-Middleware-concept.md) to the EFS-middleware first.
- You have experience with building micro services using the Spring Boot framework.
- You have an instance of a middleware-network that you can connect to, either by setting it up yourself or by connecting to an existing one.

**Table of Contents:**

1. [Create a new maven project](#Create-a-new-maven-project)
1. [pom.xml](#pomxml)
1. [application.yml](#applicationyml)
1. [Using the library](#Using-the-library)

## Create a new maven project

Set up a new maven project with your favorite tool or IDE.

## pom.xml

Open the pom.xml and insert the following parent-tag and dependencies:

```xml
<parent>
    <groupId>com.github.keim-hs-esslingen.efs</groupId>
    <artifactId>efs-service-parent</artifactId>
    <version><!-- Use the latest available version: https://search.maven.org/artifact/com.github.keim-hs-esslingen.efs/efs-service-parent --></version>
    <relativePath/> <!-- skip parent lookup in parent directory. -->
</parent>

<dependencies>
    <dependency>
        <groupId>com.github.keim-hs-esslingen.efs</groupId>
        <artifactId>middleware-core</artifactId>
    </dependency>
</dependencies>
```
The parent POM contains information about which versions the required dependencies need to have, so you only need to worry about setting the version number of the parent project.

The middleware-core component contains all the logic you need for connecting to a middleware network. It is built around the Spring Boot framework. The framework is already included in middleware-core as a transitive dependency. However you still need to provide your own configuration using an application.yml file.

## application.yml

Create and/or open your application.yml file and add the following properties to it:

```yml
# Define a name for your service
service-name: demo-consumer

# Insert the base-url of the service-directory, that you want to connect to.
service-directory-base-url: http://localhost:9900/

spring.application.name: ${service-name}

server.port: 9910 # Configure a port for your service

eureka:
  client.service-url.defaultZone: ${service-directory-base-url}/eureka/
  instance.appname: ${service-name}
```
Let's go quickly over the differenct properties that are defined in this file:

The first two properties `service-name` and `service-directory-base-url` really are nothing else put variables that are used later in this file again. If you want, you can remove them and replace their references with the values.

The `spring.application.name` is a spring property that defines a name for your service. This value is used in various places.

The `server.port` property defines on which port the spring application should run. This port can be freely choosed.

The properties underneath `eureka` are some important properties that let your middleware service know, where to find the service-directory. This is necessary because the service-directory will tell the consumer service where to find the registered provider-services. Without these, you can actually not find them.

## Using the library

After setting everything up like this, you can autowire the following spring boot components that are offered by the middleware-core library:

- `ConsumerService`: All in one component for easy access to the middleware.
- `ServiceDirectoryProxy`: Component for easy access to the Service-Directory.
- `ProviderProxyBuilder`: Component for creating proxies which allow easy access to providers in the middleware network. (not yet there)

The best way to get started with these components is to autowire them and let the IDE show you which methods they provide. Understanding the concept of communication on the middleware helps a lot in understanding on how to use the methods provided by these components. For more detail have a look at their respective JavaDoc.

Since your project is a child of `efs-service-parent`, it automatically is also a child of Spring Boots starter parent. Therefore you can easily add your own Controllers, Services, Repositories and whatever else you want.
