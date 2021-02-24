# Observability

The theoretical observability goal of the Iomorph is, for every single closure \(e.g. function frame, HTTP request, etc.\), to capture:

* Input values
* Output values
* Start
* End
* ID
* Parent ID

In practice, this is an impractically large amount of information to capture - imagine instrumenting all of this data for the innermost function inside of a tight loop.  We instead focus our telemetry on the following spans:

* The public interface exposed by the domain system.
* Any IO layers implemented in the adapters.

The rationale is fairly simple: if you know what happened on the edge of your system, you will typically have a type system to help guide you through the rest of your business logic.  On the flip side, there are certain things that are impossible to figure out unless you know what happens on the edge.  Moreover, instrumenting at the domain system layer ensures that _no matter how the Iomorph is isolated_, you have a _consistent and universal_ set of metrics that you can rely on.

### Tracing

The goal is to create spans that _match_ the spans described above.  Therefore, your execution context must support custom middleware that allows you to open and close spans with a request lifecycle, both for inbound _and_ outbound requests.  Most importantly, you need to be able to attach the spans to some kind of context that mirrors the request lifecycle.

Standard OpenTracing data covers the main needs, though each adapter may benefit from some custom metadata \(e.g. HTTP URL or DB query\).

### Logging

Logging is fairly straightforward, the main thing is to coerce into a global logging format \(JSON with short keys is a common one\), and to support some standard metadata:

* timestamp
* log\_type \(access/app/etc.\)
* loglevel
* traceId

### Eventing

Instantiate your clients for [schematized eventing](https://medium.com/remitly/unified-event-logging-3a4cc64fa1c6).  Support some standard metadata:

* event\_id
* environment
* application\_name
* closure\_name
* event\_timestamp
* schema\_name
* traceId should be injected as metadata

### Metrics

Prometheus-style metrics should be auto-generated matching the spans above.  These metrics should include:

* counts and TP50/90/99 latencies
  * per function
  * per response type \(success, failure by base error classes\)

### Exception Logging

The Iomorph responsible for the _origination_ of an _unhandled_ error bears the responsibility of forwarding it to a dedicated exception logging tool \(bugsnag, custom log line, etc.\) that can be monitored and properly remedied \(e.g. by adding the proper code to handle, etc.\).

* The traceId should be injected as metadata.

It's important to clarify what it means to _originate_ an error: if you make an API call and it times out with a 504 response code, you should certain propagate an error code to your caller: but you _should not log an exception_.  Rather, you should be catching these intermittent errors, mapping it to an appropriate response code, and monitor your error rates via your _metrics_.  If you see a 504 Gateway Timeout exception - it's not actionable on your side, whereas the service that _actually_ timed out should be responsible for monitoring _their_ metrics to ensure they are meeting their SLAs.

If, on the other hand, you didn't implement all of the branches of a function and left a `raise Exception("TODO")`, then _this_ you should log, because it means you're hitting a code-branch that needs _active development work_ to remedy.



