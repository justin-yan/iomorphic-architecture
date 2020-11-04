# Inspiration

## A Short History

If you search for "Monolith vs. Microservice", you'll find untold thousands of articles and blogposts going back and forth, spawning many variants, such as nanoservices, modular monoliths, so on and so forth.  The industry has seemingly converged on a tenuous agreement that monoliths are the right place to start, but that microservices are necessary for organizational scale and eventually become worth the "operational tax".

The question that's been top of mind for many software engineers is simply: what is it about isolating services in different VMs and using HTTP to compute that results in _**organizational**_ scale?  Why do we struggle to accomplish the same with programming language modules and function dispatch?

At an anecdotal level, **microservices** seem to clearly win on the encapsulation point.  The average service within a microservice _feels_ much less tangled than the average domain within a monolith.  A commonly proposed rationale for this is that HTTP APIs are harder to evolve due to backward-compatibility requirements and therefore discourage uninformed modifications of reaching into other domains and changing their APIs.  This feels like it misses the mark, however, as there has been no corresponding revolution of monoliths adding static analysis throwing errors when two subsystems are modified concurrently.

If we try to focus on the technical definition of **VM-level Isolation** and **HTTP** as a communication mechanism, the _theoretical_ advantages of microservices are significant but limited to:

* Polyglotism
* Ability to tolerate conflicting dependencies between two Systems
* Independent scaling & operational lifecycles
* Ease of Attribution
  * e.g.: who do you page for `OutOfMemory` ?

Of course, there are a number of _practical_ advantages, which are due less to the fundamental advantages of the technology, and more to the momentum of the ecosystem:

* Telemetry \(Consider: how many monoliths have error rate and latency metrics for internal domain boundaries?\)
* Supporting Toolchain

Some of these advantages definitively assist with organizational scale.  Ease of Attribution, for example, is an extremely common problem for teams with a monolith and a 40-person pager rotation.  On the other hand, these advantages don't seem to _really_ support the conclusion that services should be extremely small in size - the argument for that seems to revolve more around _having headroom for future growth_, precisely because existing architectures _are not sufficiently flexible_ when it comes to how we isolate systems from each other.

Why do we struggle with this flexibility?  The most dominant structure for both **Monoliths** and **Services** \(within a microservice architecture\) over the last decade has been the **MVC** structure.  The fundamental problem with this structure is that it demands, at the top level of a codebase, a functional split between model, view, and controller.  At the highest level of the architecture, it is essentially _impossible to encapsulate a **domain**_.

Consider an example of the **pricing team**, trying to add their domain to this monolith.  Even if they add a perfectly encapsulated domain system in `controllers.pricing`, and express their APIs in `views.pricing`, and put all of their database accessors in `models.pricing`, it doesn't change the following two facts:

1. There is no single folder `pricing` that you can open and see _all relevant code_.
2. When you open `models`, you will see the models for _all domains_, and more importantly, most languages don't have a mechanism for making sure that `controllers.accounting` can access `models.accounting` but not `models.pricing`.

This extremely simple reason is our argument for why microservices have, in practice, felt far better encapsulated than monoliths.  A microservice has a single folder that contains its logic, and its internals are _truly_ encapsulated behind an API.  A domain within a monolith, under the MVC style, has to be discovered across multiple folders commingled with other domains, and its internals are exposed to other coexisting domains.

The conclusion here, however, is that the real problem is not with using language modules and function dispatch.  The problem is that **MVC,** as an architecture, is not designed to encapsulate domain systems in a way that is compatible with organizational scale.  Which leads us to the question: can we design an architecture that yields the encapsulation benefits of microservices while retaining the operational characteristics of a monolith? and even more intriguingly, can we make it so that we can _smoothly change how systems communicate_ and getting the benefits of _both_ architectures wherever they are most needed?

## Prior Art

There is no doubt that this architecture borrows almost all of its ideas from other architectures that have come before it:

* OOP and SOLID provide much of the language around **encapsulation** that is such a major focus, but need to be generalized beyond the boundary of a process.
* Erlang, Akka, and the Actor Model introduce the concept of **Location Transparency**, which is a direct inspiration of our principle "Isomorphic under Isolation", but you then mostly see MVC monoliths being built on top of these frameworks, suggesting we need a different _architectural_ approach to realize the full potential.
* Finally, the Hexagonal and Clean Architectures, with the illustration of the **domain core**, provided the final piece of the puzzle for this architecture, and are almost exactly how Atomic Iomorphs are expected to be implemented.  However, while these architectures provide more flexible functional layering than MVC, they still don't solve the problem of multiple colocated domain services needing to isolate from each other.

More than anything else, this architecture is a synthesis of these many great ideas into a single approach to enable rapid, iterative, and flexible development of complex back-end systems.

