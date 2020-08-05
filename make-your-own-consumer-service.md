# How to make your own Consumer-Service

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](./middleware-concept-introduction.md) to the EFS-middleware first.
- You have experience with building micro services using the Spring Boot framework.
- You have an instance of a service-directory that you can connect to, either by setting it up yourself or by connecting to an existing one.

**Table of Contents:**

1. [Create a new maven project](#Create-a-new-maven-project)
1. [pom.xml](#pom.xml)
1. [application.yml](#application.yml)
1. [Configuring the Consumer-API](#Configuring-the-Consumer-API)
    1. [Using the Consumer-REST-API](#Using-the-Consumer-REST-API)
    1. [Using the middleware-core as a library](#Using-the-middleware-core-as-a-library)
1. [Swagger-UI Setup](#Swagger-UI-Setup)



## Create a new maven project

Set up a new maven project with your favorite tool or IDE.

## pom.xml

Open the pom.xml and insert the following parent-tag and dependencies:

```xml
<parent>
    <groupId>com.github.keim-hs-esslingen</groupId>
    <artifactId>efs-parent-starter</artifactId>
    <version>1.0.0</version>
</parent>

<dependencies>
    <dependency>
        <groupId>com.github.keim-hs-esslingen</groupId>
        <artifactId>middleware-core</artifactId>
    </dependency>
</dependencies>
```
The parent POM contains information about which versions the required dependencies need to have, so you only need to worry about setting the version number of the parent project.

The middleware-core components are build around the Spring Boot framework. The framework is already included in middleware-core as a transitive dependency. However you still need to provide your own configuration using an application.yml file.

## application.yml

Create and/or open your application.yml file and add the following properties to it:

```yml
# Define a name for your service
service-name: demo-consumer

# Put the base-url of your service-directory here.
service-directory-base-url: http://localhost:9900/

spring.application.name: ${service-name}

server.port: 9910 # Configure a port for your service

eureka:
  client.service-url.defaultZone: ${service-directory-base-url}/eureka/
  instance.appname: ${service-name}

efs.middleware.consumer-api.enabled: true
```
Let's go quickly over the differenct properties that are defined in this file:

The first two properties `service-name` and `service-directory-base-url` really are nothing else put variables that are used later in this file again. If you want, you can remove them and replace their references with the values.

The `spring.application.name` is a spring property that defines a name for your service. This value is used in various places.

The `server.port` property defines on which port the spring application should run. This port can be freely choosed.

The properties underneath `eureka` are some important properties that let your middleware service know, where to find the service-directory. This is necessary because the service-directory will tell the consumer service where to find the registered provider-services. Without these, you can actually not find them.

The property `efs.middleware.consumer-api.enabled` is of special interest to us, so we will have a closer look at what setting this value actually does.

## Configuring the Consumer-API

The value for the `consumer-api.enabled` property is set to `true` in the above setup. For making a consumer service, this can *both* be set to `true` or `false`. Setting it to `true` is *not* a requirement for making a consumer service.

The difference between both values is, that `true` will activate the built-in ready-to-go consumer REST-API, making this consumer service accessible for other services **over HTTP**. This is helpful if you want to integrate the consumer service into your existing micro service landscape by using an HTTP-API.

However you can also integrate the consumer service by using the middleware core as a library. In such a case you would set the `consumer-api.enabled` property to `false` and instead make your own spring boot components that autowire the built-in and ready-to-go spring boot components of the middleware-core.

You can use both integration methods simultaneously. Enabling the consumer API does not restrict you to using the middleware-core with HTTP.

### Using the Consumer-REST-API

If you decide to use the REST-API by enabling the corresponding property above, you are basically done already. The only thing you need to do is to create a simple Spring Boot Application class annotated with the usual Spring Boot annotations and that's it.

The next step of course now is to use the consumer REST-API. If you want, you can make your service generate a Swagger-UI for you, which might be helpful for communicating with it. See the chapter [Swagger-UI Setup](#Swagger-UI-Setup) below for instructions on how to do this.

### Using the middleware-core as a library

If you decide to use the middleware-core as a library you can autowire the `ConsumerService` component, which gives you methods for accessing the middleware platform from right within you own spring boot components.

Here's an example on how you would do that:

```java
@Service
public class MyInternalService {

    @Autowire
    private ConsumerService middleware;

    public List<Options> getOptions(OptionsRequest request, String credentials){
        var options = middleware.getOptions(request, credentials);
        
        // ...
        
        return options;
    }
}
```
---

## Swagger-UI Setup

If you want your consumer service to generate a Swagger-UI, add the following dependencies to your pom.xml:

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
</dependency>
```

The Swagger-UI will be available at http://localhost:9910/swagger-ui.html, as soon as you restart your consumer service.