 
# Introduction to the EFS-Middleware

The **E**co**F**leet**S**ervices-Middleware is a pseudo-decentralized digital platform for offering and consuming mobility services. It is intended to provide a standardized API for searching and booking mobility services (if you are a consumer) as well as registering and providing such services (if you are a mobility service provider).

First of all, what is *the* middleware? What we call the middleware is not a particular software component that can be installed somewhere but rather it is a network of microservices that work together hand in hand to achieve the goal described above. *The* middleware is a network. It is the network between the consumers and providers that brings them together on the platform. It is totally valid to say that the middleware as a network also is the platform.

A the following picture you see a fictional middleware that connects varous players together in one network making them able to communicate with each other:

// TODO: Add high level middleware depiction.

As you can see there is a central component in the middleware, called the service-directory, that all other components are connected to. This service-directory, as the name already suggests, serves as a registry for all mobility services that are provided by the mobility-service-providers within this middleware network. The service-directory provides an API for registering such services as well as for querying the registry. The consumers in the middleware use this API to search for mobility providers that match their needs.

If a provider joins the middleware-network it only needs to register it's mobility service at the service-directory to be discoverable for potential consumers.


