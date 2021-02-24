# Infrastructure

The Infrastructure folder of an Iomorph holds the logic that spans across multiple ports, the domain, or the ExecutionContext.  Similar to the idea of infrastructure _systems_, any code that is within the Iomorph can use the modules in this folder.

There is nothing special about the internal structure of this folder, and it generally should not collect a _large_ amount of functionality - it is the holding point for those bits of code that are horizontal within an Iomorph but have not been converted to _libraries_ that get imported from elsewhere.

### Clients

Clients or connection pools that need to be instantiated at application start-up and may be re-used will typically have their definitions in the infrastructure folder.  This often includes services like schematized eventing or feature flagging.

