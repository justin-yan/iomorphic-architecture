# Atomic Iomorph

An **Iomorph** is the representation, in code, of a single System and the isolation and communication mechanisms it supports.  The _internal_ structure of an Iomorph follows the principles of the [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/):

1. _**How**_ an Iomorph communicates should be separated from the conceptual interface.  Specifically, there should be a **domain system** that encapsulates all of the pure business logic, and **ports** that provide the abstract interface to IO that are concretely implemented by **adapters**.  Indeed, a system may be called in multiple ways \(API, CLI, Admin API, etc.\), and none of those specifics should leak into the _domain system_.
2. The pure domain logic can be implemented in any style, but as it grows to an unwieldy size, _new Iomorphs_ should be built and composed instead of accumulating more domain logic in a single Iomorph.

While this structure doesn't solve the domain encapsulation problem that MVC presents \(colocating two systems will allow them to access each others' ports still\), it provides a number of concrete benefits over MVC, such as creating containment around communication mechanisms that prevents their specifics from leaking all over the application.

## Layout

Examples:

* TODO

The high-level layout of an Iomorph in source code will look something like this:

```text
/app
    /domain
        /types
            DomainDataType.code
        DomainSystem.code (The public interface to the system)
        ...
    /ports
        /port1
            /types
                IOSerdeType.code
            IPort1.code (The public interface)
            DBAapter1.code (A DB impl of port 1)
            InMemAdapter1.code (An in-mem impl of port 1)
            ...
        /port2
        ...
    /infrastructure (anything shared across domain and ports)
    ExecutionContext.code
/integrationTest
/test
buildfile
orchestrationConfig
```

## Rules

Before we dig into each of those layout sections in detail, it's important to first cover some overall conventions we follow beforehand:

* **Domain** must follow a number of rules governing _purity_:
  * All domain `types` must be pure data types, in the functional programming sense.
  * The domain may only import and reference the _**port interfaces.**_
    * This ensures that the domain does not develop a dependency on the specific mechanism of communication.
  * The domain must implement a `System` that presents its public interface _as well as_ contains the concrete implementations of the system functions \(whether it's all in one file or delegates to other modules is up to you\).
* **Ports** are _defined_ as an abstract `Interface` in the programming sense.
* **Adapters** are the concrete _implementation_ of a particular port, for a particular communication mechanism.
  * If needed, custom types for Serde \(e.g. POJOs for JSON serialization\) are placed in the `types` package.
* **All interfaces** \(both domain and ports\) must follow the following rules:
  * Function input and return values must be defined with language primitives or the **types of the domain** \(the`domain.types` package\).
    * This ensures that concrete details of a communication mechanism don't leak into the rest of the application.
  * _All_ return values must use an error container.  Scala's `Try[A]`, Kotlin's `Result[A,E]`, Haskell's `Error`, and so on.  This does many things:
    * It forces you to transform your errors into _domain errors_ \(e.g. avoiding SQLException propagating all over your codebase\).
    * It avoids assumptions about function totality, preparing you for the transition to a communication mechanism that can fail, like HTTP.
* **ExecutionContext** is where the application is actually wired together into the main function that actually gets executed.
  * **Adapters** are injected into the **DomainSystem** as concrete implementations of the **Ports**.
  * Concerns like _config_ and _environment switching_ \(dev/test/prod\) are handled here.
* **Test** should hold _unit tests_ exclusively.
* **IntegrationTest** should hold any tests that require communication across system boundaries \(e.g. exercising a database, or talking to another service\).

