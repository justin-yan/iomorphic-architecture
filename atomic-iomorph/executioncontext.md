# ExecutionContext

The Execution Context is where you stitch together the Domain System and Adapters in order to define the program that _actually runs_.  Typically, this is what you'll want to do:

* Instantiate your infrastructure.  It is typically a requirement that your execution substrate support the ability to execute custom middleware so that this infrastructure functionality can be performed.
  * config
  * observability:
    * logging & global log formats
    * global error handling \(typically a base exception handler for exception capturing\)
    * feature flagger, event publisher, tracer, and metrics
* Use your config object and wire together your object graph.
  * Typically this is downstream ports, domain system, upstream ports.
  * If you have bidirectional ports, then this may look more like instantiating actor systems.



