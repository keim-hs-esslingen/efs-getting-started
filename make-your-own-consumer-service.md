# How to make your own Consumer-Service

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](./middleware-concept-introduction.md) to it first.
- You have experience with building micro services using the Spring Boot framework.



**Table of Contents:**

1. Create a new maven project
   1. pom.xml
   2. application.yml
      1. Configuring the consumer-API



## Create a new maven project

Set up a new maven project with your favorite tool or IDE.

### pom.xml

Open the pom.xml and insert the following parent-tag and dependencies:

```xml
<!-- Parent POM containing version information about the needed dependencies. -->
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

The middleware-core components are build around the Spring Boot framework. The framework is already included in middleware-core as a transitive dependency. However you need to provide your own configuration.

### application.yml

Create and/or open your application.yml file and add the following properties to it:

```yml
spring:
  application:
    name: my-consumer-service

server:
  port: 9910

efs: 
  middleware:
    provider-api:
      enabled: false
    consumer-api:
      enabled: true
```

The really interesting part is the lower block. It sets two different properties which are of interest to us now.

First there is the `provider-api.enabled` property. It configures the middleware to enable the built in and ready to go provider REST-API, which makes it accessible for other consumer services as a provider. This is set to `false` as we want to be a consumer service and not a provider service.

#### Configuring the consumer-API

The second one is the `consumer-api.enabled` property which is now set to `true`. For making a consumer service, this can both be set to `true` and `false`. Setting this to `true` is *not* a requirement for making a consumer service. The difference is, that with using `true` in this place, the built in ready-to-go consumer REST-API will be enabled, making this consumer service accessible for other services over HTTP. This is helpful if you want to integrate the consumer service into your existing micro service landscape by using an HTTP API.

However you can also integrate the consumer service by using the middleware core as a library. In such a case you would set the `consumer-api.enabled` property to `false` and instead make your own spring boot components that autowire the built-in and ready-to-go spring boot components of the middleware-core.

You can use both methods simultaneously. Enabled inthe consumer API does not restrict you to using the middleware-core in that way. Also you can enable both provider API and consumer API at the same time, making your micro service a bidirectional adapter to your own mobility backend.

##### Using the consumer-REST-API

If you decide to use the REST-API by enabling the corresponding property above, you are basically done already. The only thing you need to do is to create a simple Spring Boot Application class annotated with the usual Spring Boot annotations and thats it.

The next step of course is now to use the consumer REST-API. If you want, you can make your service generate a Swagger UI for you, which might be helpful for communicating with it. A guide to get started with the APIs used in the middleware might be written at a later point in time.

##### Using the middleware-core as a library

If you decide to use the middleware-core as a library you can autowire the ConsumerService component, which gives you methods for accessing the middleware platform from right within you own spring boot components.

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

