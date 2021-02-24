# App Configuration

### Dynamic Configuration

Configuration that can change and interact while the application is running \(e.g., feature flags\) should either be a first-class port, or simply be an infrastructure side-effect like logging.

### Static Configuration

Standard values:

* name
* environment
* dependencies\(url, credentials\)

Static configuration is for the following use cases:

* Per Environment/Stage configuration.  You want prod and test to run with slightly different behavior \(different database URLs, etc.\).
* Per Application configuration.  Sometimes your code will be re-used by others and needs to be configurable for domain-specific use cases.
* Secrets.  You have static values like passwords that need be used but also as obscured as possible.

The recommended approach is straightforward:

1. There should be a typed config object that is _singleton_ hydrated \(or cached\) on application start-up in your ExecutionContext, either through environment variables or a config file with environment variables for secrets.  This helps applications fail immediately on invalid config values.
2. This configuration value \(or sub-configuration objects\) should be injected into the functions/Systems/Adapters that need them, instead of global references.  This makes testing easier and allows for the tracing of provenance.
3. In your ExecutionContext, the singleton config should be used in the instantiation of all Systems/Adapters, but more importantly, there should be a single _switch_ statement that is used to do environment-based \(dev/test/prod\) swapping of code \(in-mem vs. db, etc.\).
   1. Generally speaking, all data-based environment switching \(DNS, worker counts, etc.\) should be pushed out of your application: either your runtime provisioner should supply the correct value for the environment, or it should be resolved in a higher-level system, such as DNS resolution through service discovery.

#### 

