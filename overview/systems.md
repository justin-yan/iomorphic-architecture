# Systems

A **System** is the _generalized_ version of a "Service".  It is the conceptual representation of the _actor/agent_ _that encapsulates a domain_, but **independent of the specific technologies that provide isolation and communication**.

It has all of the same requirements as a "Service": a public API, encapsulation, SLAs, contracts, but is not technology specific.  Indeed, the only requirement is that a System must choose _some_ Isolation Mechanism in order to hide its internal implementation from other Systems, but the _correct_ Isolation Mechanism is ultimately a domain, organization, and context-specific decision.

The important thing to note is that if a System starts off by using function dispatch to communicate and eventually switches to exposing a REST API - conceptually, it is still the _**same**_ System.  Consider the following:

SYSTEM COMPARISON IMAGE TODO

Even if we communicate with "pricing" by _importing_ it and using _function dispatch_, it is a distinct conceptual entity.  **Every domain is represented by a System**, but **every System may independently choose** _**which**_ **Isolation Mechanism it wants to use to encapsulate its internals**.  This decouples the concerns of how to _represent_ domains from how to _isolate_ them.

## Composition

Systems are only as useful as their ability to talk to each other, for which we allow three main patterns.

### Peering

**Peering** is what most people are familiar with.  System **Peers** are allowed to talk to each other, through whichever communication mechanisms they each support.  The only requirement is that in a given peering group, the _same_ Isolation Mechanism is used to separate all of the systems.

PEERING IMAGE TODO

### Hierarchy

A **Hierarchical** relationship is when one System \(the parent\) _encapsulates_ another System \(the child\).  Specifically, the **parent** uses an Isolation Mechanism to prevent its **Peers** from communicating with its **Children**.  The **children** of a System do not have any requirements for their inter-relationships - they may all be peers, they may all be standalone peering groups, or somewhere in the middle.

HIERARCHY IMAGE TODO

### Infrastructure

There is one main exception, which are the **Infrastructure** systems that need to implement some horizontal, cross-cutting concern, which may necessitate the breaking of certain encapsulation boundaries.

The primary rule is the **Infrastructure** System itself may directly communicate with _**any descendant**_ of its parent.  This means siblings, children of siblings, and so on and so forth.  Of course, this does not apply to _children_ of the infrastructure system, so any such interactions must go through the specific system that is the child of the domain system for which you want to provide infrastructure.

INFRA IMAGE TODO

## Conclusion

This provides the conceptual basis that underpins the Iomorphic Architecture.  What follows is the description of how one can structure a codebase following these principles in order to achieve a highly flexible architecture that is responsive to business needs while preserving the ability to make pragmatic and thoughtful technical decisions.

