# Evolution

The most important concern, now, is whether the architecture is flexible enough to handle the kinds of changes that are commonly required in operating software systems.

There are a few main classes of evolution that we consider:

1. Atomic Evolution occurs when two Iomorphs are separated by an isolation mechanism that allows for an _atomic_ lifecycle: changes to one occur simultaneously as changes to the other.
2. Distributed Evolution occurs when two Iomorphs talk over the network or a similarly non-atomic deployment lifecycle.
3. Isolation Evolution occurs when an Iomorph needs to change _how_ it isolates.

For each of these, we'll consider the following operations:

* Addition: new functionality being added.
* Deletion: removing functionality.
* Partition: splitting an Iomorph into two.
* Agglomeration: combining two Iomorphs into one.

## Atomic

This is the easy case, and is one of the primary benefits of keeping an atomic isolation mechanism such as programming language modules for as long as possible:

* Addition: All changes in clients and servers can simply be made simultaneously and handled with a single atomic deployment.
* Deletion: Uncalled code can be simply deleted, while called code will just need to be migrated to an alternative.
* Partition: Create a child Iomorph, with a new DomainSystem.  Import into parent.  Copy functionality into child, including entire ports if necessary.  Replace implementation in parent with dispatch to child.
* Agglomeration: Copy all ports and DomainSystem from one Iomorph to the other.  Combine methods into single DomainSystem and eliminate duplication.

## Distributed

### Addition

To migrate in place:

1. Add functionality in the called Iomorph, deploy.
2. Add usage in the calling Iomorph via port, deploy.
3. Remove deprecated functionality in the called Iomorph, deploy.

If using a compatibility layer:

1. Add a compatibility Iomorph as a peer of the called Iomorph, deploy.
   * If there is only one caller, handle migration in the middleware of the caller's port/adapter.
   * Alternatively, the "new" Iomorph can simultaneously act as the compatibility layer.
2. Convert caller to use compatibility Iomorph, deploy.
3. Create new Iomorph, deploy.
4. Add implementation from compatibility Iomorph to new Iomorph, deploy.
5. Use compatibility Iomorph to migrate with desired atomicity requirements.
6. Convert caller to use new Iomorph, deploy.
7. Delete compatibility Iomorph, delete old Iomorph.

### Deletion

1. If functionality is no longer being invoked, it can simply be deleted.
2. Otherwise, callers must be notified and re-written to stop using deprecated functionality.

### Partitioning

1. Create a new Iomorph, deploy.
2. Create port in caller, integrate to new Iomorph, deploy.
3. Migrate domain logic in caller to adapter of port.
4. Duplicate logic from adapter to new Iomorph, deploy.
5. Inject a migration adapter to rotate between new Iomorph adapter & domainsystem adapter
6. Delete all now unused code.

### Agglomeration

1. De-inject the port by duplicating domain logic to adapter implementation.
2. Migrate logic out of port into domain implementation.

## Isolation Evolution

Generally speaking, there are not hard-and-fast rules for evolving isolation mechanisms, because the _specifics_ will depend on the implementation details of which Isolation Mechanisms you've chosen.

Generally speaking, however, the process should not require _any change_ to the DomainSystem because we've cleanly separated the communication into a port, and therefore will look something like this:

1. Create a new port/adapter for the new isolation-communication mechanism.
2. Update the executionContext, orchestration, and build files to ensure that the new isolation mechanism is wired up correctly.
3. Handle adding port/adapters to the calling Iomorphs following the rules above: if the new isolation mechanism has Distributed deployments, then the addition of functionality must proceed in a backward-compatible fashion, etc.

