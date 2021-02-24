# REST API

The REST API port is used when you wish to expose the ability to communicate with your system via REST API.

## Layout

Examples:

* TODO

```text
/ports
    /restapi
        /types
            /v1
                CRUDCreateRequest
                CRUDCreateResponse
                CRUDGetRequest
                CRUDGetResponse
        /v1
            CRUDResource
        /v2
        ...
```

## General Infrastructure

As one of the main sources of inbound traffic, this port has many infrastructure concerns to implement, and it typically requires the implementation of custom middleware prior to and after response handling business logic for all major observability concerns.

* **access\_logs**: Conventionally includes all of the important span-tracking metadata but also includes:
  * Requester IP chain
  * HTTP protocol, method, URI, query params, request size in bytes
  * HTTP Response code, duration in milliseconds, response size in bytes
* **documentation**: Your typesystem should generate an OpenAPI specification which can then power redoc or swagger-doc.



## API Design

[Some principles](https://research.google/pubs/pub32713/) to keep in mind:

* Plasticity.  You need to be able to change, and versioning is a huge part of making this possible.
* Communication Patterns.
* Eliminate _workflows_.  Especially anything that isn't _domain specific_.  People find stateful APIs to be the most frustrating, because they are the hardest to experiment with - generating auth tokens to download a WSDL to pipe into requests, etc. is friction that people detest.
* Simple to use and understand HTTP Methods and path structures
* Simple to use and understand response structures and use of status codes
* Simple to use headers and rich examples and code samples for more complex HTTP features
* Consistency, Consistency, Consistency - the more consistent and simple your structures, the easier it is for people integrating

## Errors

Errors don't _have_ to be modeled with Exceptions, but for languages with them, this is generally convenient because there will be _some_ root exception handler that must understand how to translate uncaught exceptions into responses to the client.  With that in mind, it's generally convenient to have one set of mappings to responses, but the key is that however you model them, they should:

1. Show up as additional response types in auto-generated docs.
2. Be captured and managed by custom middleware so that you can emit unhandled originating exceptions to a base handler.

With that in mind, this is a fairly straightforward mapping:

* Every Domain exception class should have its own unique response code so clients can handle polymorphism appropriately.
* All domain exceptions should be mapped to an appropriate wire payload
* Server-induced exceptions should return 5xx codes
* Client-induced exceptions should return 4xx codes
  * Validation may _re-use_ logic from the domain, but if handled in the port, then returns an error that is scoped to the port.

