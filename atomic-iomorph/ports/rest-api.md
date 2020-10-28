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

## Infrastructure Concerns

* Validation.  Handle serialization, deserialization, and ExceptionMapping explicitly.
* Observability.  It's important to convert paths, headers, request/response bodies into the "inputs" and "outputs" that get captured by your observability substrate, and to do so in a structured way.
* Generation.  Documentation and Client SDKs should be generated _from_ the API specification itself.
* Auth/Auth, Security, and RESTAPI-specific protections
* HTTP protocol concerns: Disabling or enabling cache-control, compression, keep-alive/timeouts, content negotiation/unicode/language negotiation/time formatting

## API Design

Some principles to keep in mind:

* Plasticity.  You need to be able to change, and versioning is a huge part of making this possible.
* Communication Patterns.
* Eliminate _workflows_.  Especially anything that isn't _domain specific_.  People find stateful APIs to be the most frustrating, because they are the hardest to experiment with - generating auth tokens to download a WSDL to pipe into requests, etc. is friction that people detest.
* Simple to use and understand HTTP Methods and path structures
* Simple to use and understand response structures and use of status codes
* Simple to use headers and rich examples and code samples for more complex HTTP features
* Consistency, Consistency, Consistency - the more consistent and simple your structures, the easier it is for people integrating

## Errors

Errors should be mapped in fairly straightforward fashion:

* Every Domain exception class should have its own unique response code so clients can handle polymorphism appropriately.
* All domain exceptions should be mapped to an appropriate wire payload
* Server-induced exceptions should return 5xx codes
* Client-induced exceptions should return 4xx codes
  * Validation may _re-use_ logic from the domain, but if handled in the port, then returns an error that is scoped to the port.

