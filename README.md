# Introduction

**Status: First Draft.  Code Examples, Images are coming, followed by editing and a few more sections.**

The Iomorphic Architecture is an **application** _****_**architecture** - an alternative to MVC and Microservices.  The goal is to get the benefits of both microservices _and_ monoliths while simultaneously making it as easy as possible to change how systems communicate with each other.  This is captured in the two core principles:

1. **Isolation of Systems**.  OOP, Actor Systems, and Microservices have taught us that Encapsulation is the primary way to achieve organizational scale, and we take the viewpoint that **Systems** \(not _Services_, because a single web service could conceptually be multiple systems\) are the primary building blocks that require isolation.
2. **Isomorphic under Isolation**.  All of the core domain functionality of our systems will be implemented without dependencies on _how_ systems are isolated from each other, giving us the ability build our systems first, and to choose between different isolation mechanisms later.

## Next Steps

* If this idea is new to you, start by checking out the [Overview](overview/), which will guide you through the rest of the architecture.
  * Especially take a look at the [Prior Art](overview/inspiration.md#prior-art) for the ideas that inspired this.
* The Iomorphic Architecture can be introduced incrementally \(starting with just one experimental domain\), so browse an example project to get started.
* Go directly to the reference materials for the Atomic, Composition, or Evolution functions.

