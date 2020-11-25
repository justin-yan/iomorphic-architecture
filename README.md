---
description: 'Status: Rough Draft'
---

# Introduction

The Iomorphic Architecture is an application _****_architecture ****designed to be an alternative to MVC and Microservices.  If you're tired of dealing with hard-to-change Monoliths or having debates about "how big should a Microservice be?", the Iomorphic Architecture gives you a way to seamlessly blend the best of both worlds so you can focus on your real problems.

An architecture should provide the means for you to **express a domain**, and to **wire domains together**.  Microservices, as an example, gives you a "Service" as the unit of modeling for a domain, while also coupling it to a networked HTTP call for wiring two domains together.  This has one major problem, however, which is that _HTTP_ as a communication technology imposes certain constraints in terms of _size_, which means that how you _model_ your domains is being influenced \(and even dictated\) by how your domains _communicate_ with each other.

The Iomorphic Architecture seeks to provide this functionality \(expressing a domain, wiring domains together\) while solving for two problems that will inevitably happen in every system:

1. You draw the wrong boundaries for your domains.
2. The technologies you use to wire domains together needs to change over time.

This is done by making it as easy as possible to **evolve** both the boundaries of your domains and the specific technologies used to wire those domains together, which leads to the two key principles:

1. **Unified Modeling.**  _All domains_ are expressed in the same way, which we call an **Iomorph**.  This applies no matter how big, small, or numerous you want them to be - and indeed, we place no constraints on _how you subdivide your domains_ - whatever works best for your problem is what you should do.  They must only adhere to the **Iomorph** conventions so that it is _easy to change them_.
2. **Isomorphic Communication**.  The business logic of an **Iomorph** is not allowed to depend on _any specifics of how it communicates with other Iomorphs_.  Specifically, an Iomorph communicates with others through **Interface Ports**, which cannot leak any information \(such as whether it's going over the network, HTTP response codes, etc.\).  This gives us a structured way to have two Iomorphs communicate, while making it as easy as possible to change this technology choice in the future if necessary.

## Guide

Throughout this reference, we'll use example projects to orient our discussion:

* TODO

This reference is organized in the following major sections:

* Overview: how to read and use this reference, what the high-level ideas are, and the necessary background information.
* Atomic: how to build a single system compatible with the architecture.
* Composition: how to stitch various systems together.
* Evolution: how to coordinate change across systems in order to safely modify behavior.

## Roadmap

* Library/Framework for building Iomorphs.  \(e.g. what Ruby on Rails was for MVC\)
  * Can standardize errors, telemetry, control plane, interface enforcement, etc.
* Additional research on Composition and Isolation Layers:
  * Where does Isolation Mechanism provisioning live?
  * Tooling requirements for multi-iomorph polyglot builds.
  * Local orchestration, production provisioning/deployments.
  * Implementation requirements for an isolation layer.
* Iomorph Registry
  * Managing runtime and infrastructure configuration \(alerts, dashes, etc.\)

