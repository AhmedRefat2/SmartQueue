# ADR-001: Adopt Clean Architecture

## Status

Accepted

## Context

SmartQueue is a multi-tenant SaaS platform for appointment scheduling,
digital queues, real-time customer flow, notifications, reporting,
subscriptions, and external integrations.

The system contains business rules related to appointments, queue
lifecycles, tenant isolation, authorization, concurrency, and
idempotency.

At the same time, the system depends on infrastructure concerns such as
SQL Server, EF Core, Redis, SignalR, background jobs, email, and
external payment integrations.

We need clear boundaries between business logic and infrastructure
details to keep the system maintainable, testable, and easier to evolve.

## Decision

We will use Clean Architecture for SmartQueue.

The solution will be organized into four main projects:

- SmartQueue.Domain
- SmartQueue.Application
- SmartQueue.Infrastructure
- SmartQueue.Api

The Domain layer will remain independent from infrastructure and
framework-specific concerns.

Dependencies will follow inward toward the Domain layer.

The Application layer will contain application use cases and business
orchestration.

The Infrastructure layer will contain implementations for persistence,
external services, caching, and other infrastructure concerns.

The API layer will expose the application's HTTP endpoints and handle
transport-related concerns.

## Alternatives Considered

### Traditional Layered Architecture

A traditional API → Services → Repositories → Database structure would
be simpler initially, but the increasing number of infrastructure
concerns and business rules in SmartQueue makes stronger boundaries
valuable.

### Single Project Architecture

Keeping the entire system in one project would reduce initial setup
complexity, but would make separation between business logic and
infrastructure less explicit as the system grows.

## Consequences

### Positive

- Clear separation of responsibilities.
- Business logic is less coupled to infrastructure details.
- Better testability.
- Easier replacement or modification of infrastructure implementations.
- Clear dependency boundaries.

### Negative

- More projects and abstractions.
- More initial development and architectural overhead.
- Simple features may require more code than a simpler architecture.

## Related Decisions

- ASP.NET Core Web API
- Entity Framework Core
- SQL Server
- Redis
- JWT Authentication
- SignalR
