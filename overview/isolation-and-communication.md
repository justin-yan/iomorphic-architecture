# Isolation and Communication

An **Isolation Mechanism** is a collection of technical choices that provides _some degree of encapsulation_ between systems.  Consider a few examples:

* **Programming Language Modules**: using things like public/private visibility and interfaces to hide implementation detail.
* **Process Isolation**: running programs on the same machine but in different processes can additionally isolate memory space, etc.
* **Containers**: Containers can isolate most aspects of the OS services \(CPU, Mem, Filesystem, etc.\), while sharing the parent OS.
* **Virtual Machines**: VMs can provide a fully isolated OS and corresponding set of services, while allowing for sharing hardware.
* **Bare Metal Isolation**: Running services on their own physical hardware can isolate effects like CPU steal and security risks of VM 0-days
* **VPC and Network Isolation**: Services can be hidden from each other by residing on their own _private networks_ that aren't visible to the external world except through a Gateway.

There are an infinite number of varieties and flavors: putting containers in VMs, running sidecar proxies next to containers, putting load balancers in front of VM fleets - on and on.

When an organization uses an Isolation Mechanism, they _also_ choose the **Communication Mechanisms** that they consider _appropriate_ for communicating _across_ the Isolation Mechanism.  Some examples of this might include:

* **Public Function Dispatch**
* **Inter-Process Communication** \(e.g. going through a file or local OS sockets\)
* **TCP/IP**

There are a few critical observations to make here:

1. There is no such thing as a "universally correct" Isolation Mechanism.  Each mechanism will have advantages and disadvantages, and for each system, an organization should select the mechanism that best meets their needs.  This is often a decision that turns on non-technical factors: which technologies will empower individuals and teams to effectively solve problems that matter?
2. Just as two systems may want two different isolation mechanisms, a single system at _two different times_ may want two different isolation mechanisms.  As the world changes, a system may need to evolve, split, or isolate in order to accomodate changing requirements, and an architecture should facilitate **the migration of a system to different isolation mechanisms**.

There are a huge variety of case studies from around the internet that illustrate this point:

* [Uber's DOMA](https://eng.uber.com/microservice-architecture/): Uber had _**2000**_ services \(Container/VM isolation + HTTP/TCP/IP communication\).  What's clear from the problems described in the post is that their existing mix of isolation levels they were using didn't allow them to _express and encapsulate their domain hierarchy_.  Perhaps they had a "mapping" unit encompassing 50 services - if any other unit was allowed to call any of those other services, it becomes nearly impossible to truly _encapsulate_ the responsibilities of "mapping".  
  * The mistake that Uber has made here is to convolute the idea of a "domain" with a specific isolation mechanism.  They themselves say "A common question in designing a domain is 'how big should a domain be?' We give no guidance here."
  * The truth is that DOMA is simply a new _isolation mechanism_ \(VPC + Gateway isolation + HTTP/TCP/IP communication\) that they are using _alongside_ traditional services and language modules in order to organize their _entire domain hierarchy_.
* [Segment's Superstar](https://segment.com/blog/goodbye-microservices/): Segment needed to forward events from their firehose to a variety of different destinations, and they experienced a variety of problems ranging from downtime blast radius to irrelevant test failures which they decided to isolate using containers, HTTP, and separate VCS repositories.  Eventually, as they accumulated hundreds of destinations, the operational overhead of hundreds of services and repositories became unwieldy.  Cross-cutting changes became too time-consuming to deploy, and alternative solutions were discovered for the previous testing problems.
  * I think this beautifully illustrates the point-in time nature of isolation mechanisms.  Going from 10 destinations to 100 destinations is a _business_ evolution, and the decision to stay with microservices vs. going back to an atomic deployment unit is something that largely depends on non-technical factors: How important is rolling out changes across all 100 destinations simultaneously? How valuable is each individual destination and how many engineering headcount can we devote to them?
  * Every engineering team is constantly solving for the right mix of _isolation_ of things that don't matter and preserving the ability to make the cross-cutting changes they require.  An architecture should make it easy to express the business logic of your systems and then modify the isolation levels between them as needed.

