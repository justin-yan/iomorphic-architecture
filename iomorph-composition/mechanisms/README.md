# Isolation Mechanisms

The most critical thing for a particular Iomorphic application is to define the _allowable isolation mechanisms_.  Any Isolation Mechanism must support the following requirements:

* A way of restricting and scoping communication to a defined API or contract.
* A way of restricting and scoping access to only parents, peers, children, and infrastructure.
* An ability to **orchestrate** and **build** the Iomorphs together across the Isolation mechanism to support local development, testing, and deployment.
* Support all major Evolution operations.

