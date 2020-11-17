---
description: 'Status: Rough Draft'
---

# Introduction

The Iomorphic Architecture is an application _****_architecture ****designed to be an alternative for MVC and Microservices.  If you're tired of dealing with hard-to-change Monoliths or having debates about "how big should a service be?" and drowning under the operational burden of Microservices, the Iomorphic Architecture gives you a way to seamlessly blend the best of both worlds so you can focus on your real problems.  The architecture revolves around two core principles:

1. **Unified Modeling.**  We want to model _all domains_ with the same entity, which we call an **Iomorph**.  This can be as big or small as you want, you can have as many as you want, and you can nest them as deeply as you want - the important thing is that every system in your application is modeled and expressed with the same building block.
2. **Isomorphic Communication**.  _How_ the Iomorphs communicate with each other is decoupled from the business logic.  This allows us to pick the appropriate communication mechanism \(function dispatch, HTTP, etc.\) for the job, which is what allows us to avoid questions like "how big should a service be?"  Your Iomorphs are what they are, and you can wire them together with whatever mechanism makes the most sense.

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

