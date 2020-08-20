# Connect to an existing Middleware-Network as a Consumer

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](./middleware-concept-introduction.md) to the EFS-middleware first.
- You have an instance of a middleware-network that you can connect to, either by setting it up yourself or by connecting to an existing one.

## Choose your way to connect:

There are three possible ways for you to connect to a middleware network as a consumer.

1. Talk to the REST-APIS of the involved services on your own.
    - Full control
    - Allows for best performance
    - No restrictions in runtime environment
1. [Create an instance of a Consumer-Adapter](/create-an-instance-of-a-consumer-adapter.md) that works as a proxy and exposes one REST-API for everything.
    - Less APIs to understand
    - Performance: medium
    - Potential bottleneck
    - No restrictions in runtime environment
1. [Use the middleware-core library](./use-the-middleware-core-library-as-a-consumer.md) to take part fo the REST-stuff for you.
    - Easy-to-use and Easy-to-Get-Started
    - Restriction to Spring-Boot and Java (or other JVM-language)
    - Performance: ranges from medium to good, depending on how you use it.

The first possibility is pretty straight forward: Get to know the API and do whatever you want with it. *(TBD: Add link to API-Docs/Swagger-UI)*

The two toher options will be described in detail in their respective guides which are linked above.
