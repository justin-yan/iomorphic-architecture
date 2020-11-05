# Vendoring

An important case to consider is when your application needs to communicate with systems _built by others_, which most prominently includes systems like databases or Saas APIs that will _not be Iomorphs you can import_, but must nonetheless be part of your architecture.

We accomplish this by giving each vendored system its own folder with the following rules:

1. The vendored system must be the _child of_ \(i.e. it must be _encapsulated by_\) an Iomorph.
2. The vendored system _does not have any peers_.
3. The parent Iomorph must interact with the vendored system through an explicit port and adapter, which ensures a clean separation of concerns.
4. The vendored system must obey all other compositional rules, including support an orchestration mechanism.

## Layout

Examples:

* TODO

```text
TODO
```

## Persistence

The most common use case here is for persistence layers/databases.  Traditionally, we have managed things like schema migrations and the orchestration of persistence layers alongside the service\(s\) that _query_ the database.

However, a major implication of point 4 above is that within the Iomorphic architecture, we are treating _each database as its own Iomorph_.  Additionally, because the vast majority of these require communicating over a network, they look a lot like we are maintaining them as their own dedicated _"services"_.

Functionality such as schema migrations, scaling operations, and provisioning should _all be managed within the database's folder_, separate from the application.  Databases are not deployed atomically with the application, and therefore require everything you would do with a network-isolated Iomorph: backward-compatibility, no-downtime, so on and so forth.

Additionally, all of these life-cycle operations should be encapsulated and then hook into the broader orchestration tools \(such as docker-compose, etc.\) that are used by the rest of the application to handle integration testing, local dev environments, etc.

