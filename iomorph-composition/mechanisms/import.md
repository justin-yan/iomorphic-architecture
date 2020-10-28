# Language Module

The Language Module isolation mechanism is my preferred starting point. It allows you to preserve a huge number of desirable benefits \(atomic changes, no network overhead, minimal distributed systems concerns\) while still allowing you to cleanly separate concerns.

## Layout

Examples:

* TODO

```text
/system1
    /system
        buildfile
    /childsystem
        /system
            buildfile
```

## Composition

The called Iomorph is instantiated in the ExecutionContext of the parent, and then directly injected as a dependency into any Iomorph that wishes to communicate with it.  No **Port** is necessary, as there is no special communication protocol or Serde.

