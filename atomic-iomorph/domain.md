# Domain

## Layout

Examples

* TODO

```text
/domain
    /types
        DomainType.code
    DomainSystem.code
    OtherCode.code
```

```text
DomainSystem {
    Constructor(IPort1, IPort2) {
        // wire-up, implementation
    }
    
    def domainMethod1(DomainType input): Try[Bool] output {
        // implementation
    }
    def domainMethod2(DomainType2 input): Try[DomainType3] output {
        // implementation
    }
}
```

## Domain

This is where the pure representation of your system lives, independent from technology choices such as which database to use, or whether to use REST or GRAPHQL.

There are no hard-and-fast rules for how much functionality a single Iomorph should contain and will depend on a variety of factors:

* The substructure of the problem you're solving
* Size and skill-set of team, which affects how they will work together most effectively
* Product and technology constraints.  An example:
  * **Transactional Atomicity**.  Databases are [_vendored iomorphs_](../iomorph-composition/vendor.md#persistence) and therefore, a transaction in a single database is not actually allowed to span two different Iomorphs.  If you want transactional behavior, you'll need to encapsulate all of the relevant domains in a single Iomorph.

There are a few primers that can provide pretty good heuristics:

* Eric Evans' [Domain Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design)
* OOP's SOLID
* Martin Fowler's Clean Code

Many resources like this will prove useful for thinking about how to arrange and size your Iomorphs, but it's important to remember that there is no single solution that works universally.  The point of the Iomorphic architecture is to make it easy to recover from the mistakes that will inevitably be made, and to facilitate the evolution of systems when the world changes around them.

## Types

### Errors

An important group of types for Iomorphs is the Error Hierarchy.  In particular, we do not wish to allow the arbitrary propagation of _mechanism-specific errors_ \(such as SQLException\) from **Adapters** to the **Domain**.  We therefore have a simple error hierarchy for the domain that we _require_ each adapter to translate its mechanism-specific errors into:

* **BaseDomainError** - this should only be used when an unexpected codebranch is hit, e.g. a fallthrough that shouldn't be hit under normal circumstances, and therefore cannot be modeled more precisely.
  * **ServerError**
    * **IntermittentError** - this is thrown when an intermittent issue arises and the same request should be succeed upon a retry.
    * **NonretriableError** - there is some known fatal error deterministically causing issues.
  * **ClientError**
    * **BadRequestError** - the caller has made an invalid request that needs to be corrected in some way.
    * **NotFoundError** - the resource the caller wants to locate cannot be found.
    * **StateConflictError** - the request is fine, but there is some known, stateful reason the request hasn't succeed which needs to be resolved.

All additional custom domain errors should then extend from one of these.

Let's consider a basic example: You are working with a SQL database, and it throws a SQLException.  There are a few possible ways to handle this:

* There are no meaningful differences in how you want to handle these errors, so the adapter catches the exception and maps it to **NonretriableError** to return.
* There are meaningful differences, but they are encapsulated within the Adapter.  For example, if a SQLException\("Timeout expired"\) returns, then perhaps you want to retry the query once.  This is caught and handled within the Adapter, but if it fails a second time, it still gets transformed to **NonretriableError** and the DomainSystem is none the wiser.
* There are meaningful differences that want to be handled at the Domain level.  For example, if a SQLException\("Conflict"\) occurs, then you may want to map that to **StateConflictError**, while other SQLExceptions still get mapped to **NonretriableError**, because your _domain logic_ may wish to behave differently in the event of an operation expressing a conflict.

In reality, a sufficiently complex port will combine all of these approaches - some errors you'll encapsulate within the adapter, some will need to be surfaced to the domain layer - but the important thing is that the domain never develops a dependency on the implementation detail of an adapter.

