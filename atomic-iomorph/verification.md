# Verification

## Testing

### Unit

The bulk of testing belongs in the `/test` folder, which should wire up all of the unit tests of the Iomorph.

Examples:

* TODO

All domain logic can be heavily unit tested, as it should be pure and can utilize fake or mock implementations of all ports. This can be done with whatever methodology you want, but property-based tests, fuzzing, and standard unit tests all work great.

When it comes to the adapters, there may be bits of pure logic that can be unit tested, but one should be very careful not to write "unit" tests that are actually integration tests requiring a database, etc.

### Integration

Every port should ideally have a Spec written, and each adapter should extend and implement that spec as part of an _integration testing suite_.

```text
IPort1Spec.code (the property-based specification of the port)
InMemPort1Spec.code (extends the interface spec and validates the adapter)
```

These tests should execute against an orchestration harness that spins up all relevant systems, and therefore must be written to be _independent_ of each other, which often requires generating unique IDs, etc.

