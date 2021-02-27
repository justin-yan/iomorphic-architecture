# Iomorphic Composition

The Iomorphic Architecture shines when you start combining Iomorphs together to form a more complex application.  It ultimately follows the [Composition](../overview/iomorphs.md#composition) methods:

* Hierarchy
* Peering
* Infrastructure

## Layout

Examples:

* TODO

```text
/<rootiomorphname>
    /iomorph
        <rootiomorph contents>
        Dockerfile
        Makefile
        src/
        tests/
        docker-compose.yml
        ci.yml
        runtime-app-def.yml
        etc.
    /<childiomorph1name>
        /iomorph
            <childiomorph1 contents>
        /<grandchildiomorph1name>
    /<childiomorph2name>
/libraries
/images
/infra-as-code
```

From an organization perspective, the Iomorphic Architecture is quite simple:

1. The entire application is organized in a monorepo.
2. Each Iomorph is in its own folder, which contains the Iomorph implementation.
3. An Iomorph's folder contains the folders of any Iomorphs that are its children.
4. Peer Iomorphs sit alongside each other in their parent's folder
5. Each Iomorph may _only_ interact with the **public interfaces** of its children, peers, parents, and the infrastructure iomorphs of _any ancestor._

