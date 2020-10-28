# Infrastructure

The Infrastructure folder of an Iomorph holds the logic that spans across multiple ports, the domain, or the ExecutionContext.  Similar to the idea of infrastructure _systems_, any code that is within the Iomorph can use the modules in this folder.

There is nothing special about the internal structure of this folder, and it generally should not collect a _large_ amount of functionality - it is the holding point for those bits of code that are horizontal within an Iomorph but have not been converted to _libraries_ that get imported from elsewhere.

Below, we describe some of the things that we typically include here:

## App Configuration

We recommend that app configuration be handled through config files with secrets injected via environment variables.

### Dynamic Configuration

Configuration that can change and interact while the application is running \(think, feature flags\) simply needs to be modeled as a first-class repository which the application must interact with as a port.  We can therefore consider this out-of-scope for our use case.

### Static Configuration

Static configuration has a few common use cases:

* Per Environment/Stage configuration.  You want prod and test to run with slightly different behavior \(different database URLs, etc.\).
* Per Application configuration.  Sometimes your code will be re-used by others and needs to be configurable for domain-specific use cases.
* Secrets.  You have values that need to remain hidden since they are passwords.

#### Injection

Configuration values should be _injected_ into the functions that need them \(most commonly, the constructors of your DomainSystem and Adapters\).  This prevents the bad practice of pulling values out of global variables, and is substantially easier to test, easier to maintain \(because there's no hidden "action at a distance"\), and will result in faster failures since broken config will fail during hydration instead of at runtime.

#### Single Point of Config

However you decide to wire up all of your objects/data \(dependency injection frameworks, or manual wiring in the app start-up thread\), _all_ configuration values should be simultaneously parsed and accessed at app-startup in order to hydrate a _single_ `ConfigurationDatum` object \(of course, this may have nested types, which is fine\). This is then what should be injected into everything else.

This is substantially easier to maintain, since all configs can be colocated in a single place to view, are hydrated in a single place, and have uniform runtime semantics. It doesn't hinder flexibility, since you can have nested objects to scope whichever values you need to other parts of your application.

#### Environment Switching

Environment switching \(dev/test/prod\) should be pushed _out_ of your application. That is, whatever process is responsible for packaging and deploying your app to different environments should have some kind of hydration process to populate the config mechanism \(whether it's file templates or environment variables\) with the correct values for the environment.

#### Switching Code

Sometimes, you will need to dynamically dispatch to _different code_ based on the environment. Ideally, this should happen in a single place in your codebase, ideally a single switch statement at app start-up to handle these situations, and to then use something akin to a dependency injection framework's override mechanism to properly handle this.

