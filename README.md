# Getting Started with the EFS-Middleware

This document is a guideline to get started with developing your own components for your own middleware. The EFS-middleware aims to create a fully decentralized and open platform for providing and consuming mobility. This can be useful for companies, e.g. by lowering the effort to seamlessly integrate external mobility services into their own mobility management software and processes, but also for individuals, for managing their own mobility.

Since making a fully decentralized platform that integrates with services of external providers involves dealing with a big amount of stakeholders with varying interests, the EFS-project is only a first step towards moving to such a decentralized platform. Getting the whole thing to run is just to big for us at the moment.

The EFS-middleware therefore is not really a decentralized platform. In the current setup it actually is a centralized network of components that allows translating the APIs of external providers to a standardized mobility API, which makes integrating external services much easier. At least thats the goal.

To understand how you can setup a middleware on your own, that represents such a network, it is helpful to read the [middleware concept introduction](./middleware-concept-introduction.md), that gives a high level overview about how the middleware network is set up and how the components of it communicate with each other.

Assuming you have read this document, you can start following one of the linked guides to creating your own middleware components.

1. [Setting up a Service-Directory](./set-up-service-directory.md)
1. [Making a Provider-Adapter-Service](./make-a-provider-adapter-service.md)
1. [Making your own Consumer-Service](./make-your-own-consumer-service.md)

If you aim to create you own Middleware network, that has a Service-Directory, Provider-Adapter-Services and one or more Consumer-Services, it's probably best to start with 1. and continue down the list in the written order.

If you are contributing to an existing Middleware, you can pick the guide that you think suits your goal best.