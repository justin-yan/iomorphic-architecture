# Verification

## Observability

When you think about a traditional monolith, it is extremely rare to see internal metrics tracking latencies and error rates for the different modules.  Iomorphs want to close this gap since we see no practical reason why telemetry should be difficult to capture just because we aren't using HTTP.

When you think about the entire ecosystem of observability tools \(metrics, exception tracking, logging, paging, tracing\), it's all fundamentally various aggregations and filters on the same data stream:

* For every single closure \(e.g. function frame, HTTP request, etc.\), we wish to capture:
* Input values
* Output values
* Start
* End
* ID
* Parent ID

Of course, this could be an impractical amount of information to capture - imagine instrumenting all of this data for the innermost function inside of a tight loop.  The main advantage that microservices have for telemetry is that there is a _natural integration point_: the boundary of the service.  Iomorphs bring that advantage to **all isolation mechanisms**.  We focus our telemetry on the following closures:

* The public interface exposed by the domain system.
* All IO layers implemented in the adapters.

The rationale is pretty simple: if you know what happened on the edges, you typically have a type system to help guide you through the rest of your business logic. There are certain things that are impossible to figure out unless you know what happens on the edge.

However, instrumenting at the domain system layer ensures that _no matter how the Iomorph is isolated_, you have a _consistent and universal_ set of metrics that you can rely on.

Errors are a useful case to consider: with the existence of a standard domain error hierarchy, we have a reasonable standard interpretation that can be used to provide error rate information that is both detailed and consistent across a variety of Iomorphs.  Additionally, we can declare that the Iomorph responsible for the _origination_ of an _unhandled_ error bears the responsibility of forwarding it to a dedicated observability tool \(bugsnag, custom log line, etc.\) that can be monitored and properly remedied \(e.g. by adding the proper code to handle, etc.\)

Examples:

* TODO

## Testing

### Unit

The bulk of testing belongs in the `/test` folder, which should wire up all of the unit tests of the Iomorph.

Examples:

* TODO

All domain logic can be heavily unit tested, as it should be pure and can utilize fake or mock implementations of all ports. This can be done with whatever methodology you want, but property-based tests, fuzzing, and standard unit tests all work great.

When it comes to the adapters, there may be bits of pure logic that can be unit tested, but one should be very careful not to write "unit" tests that are actually integration tests requiring a database, etc.

### Integration

Every port should ideally have a Spec written, and each adapter should extend and implement that spec as part of an _integration testing suite_.

```text
IPort1Spec.code (the property-based specification of the port)
InMemPort1Spec.code (extends the interface spec and validates the adapter)
```

These tests should execute against an orchestration harness that spins up all relevant systems, and therefore must be written to be _independent_ of each other, which often requires generating unique IDs, etc.

