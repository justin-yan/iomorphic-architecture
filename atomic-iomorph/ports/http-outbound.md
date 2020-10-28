# HTTP Outbound

An extremely common pattern is to have a critical domain area that delegates functionality to another service. These are typically fairly lightweight adapters with minimal testing requirements and minimal on-going operational cost.

## Layout

Examples:

* TODO

```text
TODO
```

* Define the Port Interface \(use the Repository convention\), which may simply look like the API calls, but with translated responses and errors.
* Build a mock implementation of the Port Interface to use for internal testing.
* Build the service implementation of the Port Interface, using an encapsulated `retrofit` interface and generated client.
  * Handle mapping response codes to domain errors and other internal types.
* Inject the relevant implementation and use within your domain as required.

This approach is fairly effective at completely isolating the HTTP-layer requirements from your core domain logic, allowing you to operate fully at the level of domain types and methods, along with having total flexibility for future evolution to entirely new implementations.

### Principles

1. Use DNS templating in order to switch between the different environments of a service
2. Build a client with standard OpenTracing and a local environment endpoint
3. Handle protocol level errors separately from the response code error semantics
4. Use an object mapper to create the on-wire format

