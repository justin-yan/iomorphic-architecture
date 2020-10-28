# Persistence

Persistence Adapters are built for ports that directly delegate functionality to a _data system_.

## Layout

Examples:

* TODO

```text
TODO
```

While a decent chunk of these systems might utilize a REST API and therefore look like "[yet another service](http-outbound.md)", we consider it its own type of port because:

1. The majority of these systems _don't_ use REST.
2. The majority of data systems provide _extremely flexible_ APIs, which means that virtually no contract is enforced, making it nearly impossible to conclude what can be safely modified.
3. There is no indirection layer that _you control_, meaning that you can't easily perform migrations.

People have struggled with these ports because our code architectures do not lend themselves to organizing database schema migrations and other such operational concerns separately from code, so our mental models have been muddied.

The first rule to recognize here is that any persistence system, is integrated via _Composition_, and more specifically, since these persistence systems are rarely built as Iomorphs themselves, are _Vendored_ systems.

The aspects that are specific to the persistence system must be managed in the section of the Iomorph dedicated to the _persistence system itself_. The adapter here is exclusively for managing the relationship of the domain system _to_ the persistence system.

