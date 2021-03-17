# Introduction to the Middleware REST APIs

This document gives an introduction to the APIs used in the Middleware networks. If the Middleware-Concept is new to you, we recommend reading the [introduction to the concept](Introduction-to-the-Middleware-concept.md).

**The APIs:**

Within a middleware network there are a handful of APIs that can be communicated with:

- The **[Services-API](#Services-API)** is used to register mobility service providers at a service directory.
- The **[Search-API](#Search-API)** is used to search for mobility service providers in a service directory.
- The **[Options-API](#Options-API)** is used to search for mobility options.
- The **[Booking-API](#Booking-API)** is used to book mobility options.
- The **[Credentials-API](#Credentials-API)** is used to generate tokens and credentials that are sometimes needed for using the Booking-API and/or Options-API.
- The **[Consumer-API](#Consumer-API)** is a special API that wraps some of the other APIs, which we'll get to later.

Let's go over them step by step. If you only need information about a particular API you can use their link to jump to their chapter immediately.

# Services-API

This API belongs to the *Service-Directory* and is used by *Mobility-Service-Providers* to register their mobility services at the service directory. As long as a mobility service is not registered in the service directory it cannot be found by other players in the network, except they know where to find it.

The Services-API provides a common CRUD-style API for managing mobility services. Have a look at the Swagger-UI of this API to get not know how it works. To register a new service you can simply use the corresponding endpoint.

# Search-API

This API belongs to the Service-Directory and is used by players in the network to search for *Mobility-Service-Providers* that match their needs of mobility. That means this API is mainly used by users who want to consume mobility and search for matching mobility providers. Search criteria are mobility types and modes. Later also geographical search criteria might be added, but they are currently not supported.

If you want to consumer mobility, this is the starting point. The search result contains the URLs of the mobility providers and tells you where to find their Options- Booking- and Credentials-APIs.

# Options-API

This API belongs to the providers. Each provider has zero or one instances of this API available. It is used to search for mobility options using a bunch of search criteria like where to start from, when to start, when to arrive, where to go to and a couple more. Use this API to find mobility options that you can later book using the Booking-API.

The Options-API has a header param called `x-credentials`. This param is a serialized JSON-String of the credentials used by this Provider. A few providers require you to authenticate before giving you information about their options. This is what this param is used for.

Currently the form of this JSON object is not standardized. This means you must know the form of the JSON-object for each provider beforehand. In future we want to change this to either have a standardized form of credentials or to make the form known to users so they can adapt their consuming services to it.

# Booking-API

This API is used to book mobility options that were found using the Options-API. It has a endpoint for creating new bookings, for modifying existing bookings and for getting bookings. Deleting bookings is not supported.

Creating a new booking requires sending a `NewBooking` object. This object resembles the `Options` object quite much and can be created from one of those. Copy as much information from the `Options` object as possible and as provided in the `Options` object to avoid errors from missing parameters when trying to create a new booking.

Modifying bookings is an endpoint that is mainly used to advance bookings through their states. To understand the states, read [this dedicated wiki page](Booking-Lifecycle.md) that explains each states and what it means. The modify booking endpoint can also be used to change data of a booking, but doing this is often very much restricted and is often prohibited by the backing providers, so don't expect to be free to change anything.

The `x-credentials` header param is almost always needed when using this API as most providers require authentication when it comes to booking. The param works identically to the one in the Options-API.

# Credentials-API

Often the mobility providers to not require people to authenticate themselves when searching for options. However if you want to proceed to booking some of their options, almost all of them DO require you to authenticate yourself. For this purpose you must use credentials and sometimes these credentials need to be managed. That is what this API is used for. IF you are lucky, you will never need to use it at all because you can simply use the credentials of your particular accounts at the providers Options- and Bookings-APIs.

This API is not well supported and not all of the providers actually implement it.

The `x-credentials` header param is almost always needed when using this API as most providers require authentication when it comes to managing credentials. The param works identically to the one in the Options-API.

# Consumer-API

This API is a special case of APIs that is only relevant for mobility consumers,and only if they use a so called Consumer-Service to access the middleware network. This service provides one API for everything, combining the other ones as needed to simplify the process of consuming mobility in the middleware. The endpoints of this API resemble the other ones almost identically except that many endpoints have an additional parameter, the `serviceId` or `serviceIds`. Whenever this parameter is available or required, this means that you are supposed to tell the Consumer-API which provider/s it should talk to, which means actually forwarding your request to them.

If you search for mobility options, the Consumer-API will first send a search request to the Service-Directory, searching for Mobility-Services that match your options search request. If you provide a list of serviceIds, this step is skipped. As soon as the Consumer-API has a list of matching providers, it forwards the options request to each one of them and collects their results in a list. After all of them have answered or their response has timed out, the Consumer-API sends the collected list of options back to you.

If you are using the Consumer-API for booking, it will almost always simply forward your request to the relevant provider of this booking. If you request to get all bookings, it will ask all providers that you supplied credentials for, to send their list of stored bookings. These are again collected and sent back to you as soon as possible.

The Consumer-API sometimes talks to multiple providers for you. Therefore the credentials sent in the request to the Consumer-API are actually a map of credentials for each of the providers it should forward your request to. The syntax for this object is quite simple:

```json
{
  "serviceIdOfProvider1": "... // their credentials JSON string (not the object!)",
  "serviceIdOfProvider2": "... // their credentials JSON string (not the object!)",
}
```
For each provider you simply add the serialized JSON-string of their credentials to their service id. This way the Consumer-API can cimply add the corresponding credentials to the matching request to that provider as if you had contacted that API directly.

If you are using an endpoint of the Consumer-API that will forward your request to one particular provider only, e.g. the modify booking endpoint, you do not need to build the credentials using the method above, as there is only one relevant service that your request is forwarded to. Simply use the credentials as if you were talking to the providers directly.