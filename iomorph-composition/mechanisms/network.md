# Network-based

Network-based isolation mechanisms are pretty straightforward and well-understood \(Containers, VMs, VPCs, etc.\)

* It introduces distributed system concerns, but allows you to achieve a polyglot and resource-isolated world.
* Network-isolated systems are _deployed independently_.
* Additionally, they require _recursively isolated resource boundaries_, which means VPC, AWS namespace, etc. boundaries established.
* Network-isolated systems require _recursive_ orchestration definitions, so that they can be spun up together for integration testing as required.

## Layout

Examples:

* TODO

```text
/system1
    /system
        buildfile
    /childsystem
        /system
            buildfile
```

## Composition

In order to compose network-isolated Iomorphs together, an `api` port is written in the dependency, and a `service` port is written in the caller in order to facilitate the composition.

