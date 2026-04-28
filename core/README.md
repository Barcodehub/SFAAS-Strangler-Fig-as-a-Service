# Core Module

Pure Java 21 hexagonal architecture core with domain models and port interfaces.

## Principles

1. **No Framework Dependencies**
   - Pure JDK 21
   - No Spring, no Reactor
   - Only jakarta.validation-api

2. **Immutable Models**
   - Java records
   - Validation in constructors
   - Defensive copies

3. **Async Interfaces**
   - CompletableFuture for all I/O
   - Adapters handle blocking/non-blocking
