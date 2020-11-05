# Isolation Mechanisms

The most critical thing for a particular Iomorphic application is to define the _allowable isolation mechanisms_.  Any Isolation Mechanism must support the following requirements:

* A way of restricting and scoping communication to a defined API or contract.
* A way of restricting and scoping access to only parents, peers, children, and infrastructure.
* An ability to **orchestrate** and **build** the Iomorphs together across the Isolation mechanism to support local development, testing, and deployment.
* Support all major Evolution operations.

### Taxonomy

Before diving into the different kinds of common Isolation Mechanisms, we'll cover some of the high-level dimensions that are typically considered when choosing.

* **Atomic vs. Distributed deployments**.  Some Isolation Mechanisms will result in a single artifact to be deployed, others will result in multiple artifacts that need to be deployed separately and with careful sequencing.
* **Polyglotism**.  While we recommend minimizing the number of programming languages used, as that facilitates the most seamless transitions between Isolation Mechanisms, some mechanisms allow for the use of multiple languages while others demand the use of the same language.



