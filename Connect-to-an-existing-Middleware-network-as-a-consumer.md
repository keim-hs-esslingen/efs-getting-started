# Connect to an existing Middleware network as a consumer

This guide assumes the following:

- You have basic understanding of the EFS-middleware concept. If you think this guide is hard to understand it might be helpful to read the [introduction](Introduction-to-the-Middleware-concept.md) to the EFS-middleware first.
- You have an instance of a middleware-network that you can connect to, either by setting it up yourself or by connecting to an existing one. You can also connect yourself to the public Middleware network available at mobility-middleware.de.

## Choose your way to connect:

There are two possibilities for you to connect to a Middleware-network as a consumer.

1. Talk to the REST-APIs (e.g. [these](REST-APIs-of-the-public-Middleware.md)) of the involved services on your own.
    - Full control
    - Allows for best performance (because of less involved intermediates)
    - No restrictions in runtime environment
1. [Use the middleware-core library](Use-the-middleware-core-library-to-create-a-consumer-adapter-service.md) to take care of the REST-stuff for you.
    - Easy-to-use and easy-to-get-started
    - Restriction to Java (or other JVM-language) and Spring Boot (in a particular version).
    - Performance: ranges from medium to good, depending on how you use it.

The first possibility is pretty straight forward: [Get to know the API](Introduction-to-the-Middleware-REST-APIs.md) and do whatever you want with it. The other options will be described in detail in it's respective guide which is linked above.