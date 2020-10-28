# Ports

The Domain is separated from concrete details by _interfaces_ \(Ports\) which are then implemented \(Adapters\). Every "real-world" interaction is modeled as such a port, and calls or is called \(or potentially even both\) by the domain.

This layering is what enables us to correctly handle concerns such as: where does HTTP-specific logic go? How do you handle wanting multiple ways to interact with your system? How do you handle a partner system that wishes to both drive and be driven by your system?

## Layout

Examples:

* TODO

```text
/ports
    /port1
        /types
            DBIOType.code
        IPort1.code
        DBAdapter.code
        InMemAdapter.code
```

```text
IPort1 {
    def method1(DomainType1 input): Try[DomainType2] output
    def method2(DomainType1 input, DomainType3 input2): Try[DomainType2] output
    ...
}
```

```text
DBAdapter implements IPort1 {
    Constructor(SystemConfig config) {
        // initialization logic using config (includes dev/test/prod, etc.)
    }
    def method1(DomainType1 input): Try[DomainType2] output {
        // implementation
    }
    def method2(DomainType1 input, DomainType3 input2): Try[DomainType2] output {
        // implementation
    }
}
```

## Ports

Every port should be defined as an abstract Interface with the following characteristics:

* Input/output types should come exclusively from the domain.
* Monadic return or algebraic effects should be used to express side-effects
* Testing suites should be expressed against the public interface, which should then be importable into unit or integration tests as best fits the adapter implementations.

## Adapters

Adapters should then be built against this interface and injected into the domain system as appropriate.

* Adapters should have their own `/types` package which implements the on-wire SerDe
* Adapters should fully map any implementation-specific types or errors into proper corresponding domain types and errors.

## Examples

The rest of this section contains a variety of types of ports you may wish to create, and how you might go about best implementing them.

