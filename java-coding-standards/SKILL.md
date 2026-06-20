---
name: java-coding-standards
description: Use when creating, editing, reviewing, refactoring, or testing Java code, APIs, Maven projects, Spring-style services, MySQL persistence code, concurrent code, serialization code, or any `.java` task that must follow Java coding standards, Alibaba Java Development Manual rules, or Effective Java practices.
---

# Java Coding Standards

## Overview

Use this self-contained skill to write Java code with portable rules distilled from the Alibaba Java Development Manual and Effective Java. It does not require any external vault, private notes, or separate source material.

Project instructions, repository conventions, language level, framework constraints, and generated-code policies take precedence. When this skill conflicts with a user's project rules, follow the project rules and keep the Java safety intent where possible.

## Required Workflow

1. Read the current project context before editing: `AGENTS.md`, build files, nearby Java code, tests, existing package layout, logging style, exception model, and persistence conventions.
2. Classify the Java task:
   - For any `.java` implementation or review, read `references/java-handbook-rules.md`.
   - For API design, class/interface design, generics, enums, lambda/stream, exception design, concurrency, performance, or serialization, also read `references/effective-java-rules.md`.
   - For database, ORM, security, logging, tests, Maven dependency, server, or layering work, use the corresponding sections in `references/java-handbook-rules.md`.
3. Use the bundled references as authoritative for this skill. Do not try to open any original knowledge-base path unless the user explicitly provides one.
4. Apply the strictest relevant rule unless the project has a stronger existing convention.
5. Make the smallest correct change. Do not add dependencies, frameworks, global conventions, or broad refactors unless the Java rule and the local codebase both justify it.
6. Before finishing, self-review against the checklist below and run the most relevant tests or static checks available in the repository.

## Core Defaults

- Prefer clear English names, `UpperCamelCase` classes, `lowerCamelCase` methods/variables, `UPPER_SNAKE_CASE` constants, lower-case packages, and no pinyin, Chinese identifiers, arbitrary abbreviations, or offensive terms.
- Keep methods small and focused. Aim under 80 lines; use guard clauses, strategy/state patterns, or extraction when nested conditionals exceed three levels.
- Minimize visibility. Default to `private`; expose only deliberate API. Avoid public mutable fields.
- Prefer interfaces in parameters, variables, fields, and returns when an appropriate interface exists. Instantiate concrete classes only at construction boundaries.
- Prefer composition over inheritance. If inheritance is intended, document self-use, overridable methods, and extension rules; otherwise prevent inheritance.
- Prefer immutable value objects. Avoid setters unless mutability is necessary; defensively copy mutable inputs and outputs.
- Use dependency injection for resources whose behavior affects the class. Avoid hard-wired singletons/static utilities for such dependencies.
- Use enums instead of int/string constants for finite internal domains. Do not derive business values from enum ordinals.
- Avoid raw types, unchecked warnings, unsafe varargs, and arrays mixed with generics. Use PECS: producer extends, consumer super.
- Prefer standard library utilities over reimplementation. Measure before optimizing.
- Use `try-with-resources` for closeable resources. Avoid finalizers/cleaners except as a safety net for noncritical native resources.
- Return empty collections/arrays rather than `null` for sequence results unless an existing contract explicitly requires nullable results and documents them.
- Use `Optional` only as a return type when absence is expected and callers must consider it. Do not use `Optional` for fields, parameters, collection elements, or performance-critical paths.
- For precise decimal or money calculations, do not use `float`/`double`; use integer minor units or `BigDecimal` with string/valueOf construction and explicit rounding.
- Use primitives for local/internal values when null has no meaning; use wrapper types for POJO fields and RPC method parameters/returns when null carries state.
- Use SLF4J-style logging facade. Do not use `System.out`, `System.err`, `printStackTrace`, string-concatenated log messages, or direct JSON serialization of arbitrary objects for logs.
- Validate public/open API inputs, sensitive permission entries, expensive/low-frequency methods, and batch parameters. Put parameter checks near method entry.
- Do not use exceptions for normal control flow. Catch only what can be handled; never leave an empty catch block without a clear comment and an `ignored` variable.
- For concurrent code, use executors, `java.util.concurrent`, explicit thread pools, named threads, bounded queues, lock-order discipline, and documented thread-safety. Do not rely on `Thread.yield`, priorities, or scheduler behavior.
- Avoid Java native serialization. If unavoidable, design the serialized form, declare `serialVersionUID`, validate invariants in `readObject`, prefer serialization proxy, and never deserialize untrusted data without filters.

## Conflict Resolution

- `Effective Java` says prefer empty collections over `null`; the Alibaba manual allows nullable returns with documentation. Default to empty collections/arrays for collection results. Return `null` only when a local API already uses that contract or absence has a distinct semantic, and document it.
- `Effective Java` says primitives are generally preferable; the Alibaba manual requires wrapper types for POJO fields and RPC signatures. Use wrappers at POJO/RPC boundaries and primitives inside local computations when null is not meaningful.
- Enums are preferred for finite internal state, but public binary-library return objects should avoid enum return values if the local two-party-library rule applies. Use stable DTO/string/code representations at cross-application compatibility boundaries.
- Clean code and self-explanatory names reduce comments, but public APIs, abstract methods, enum values, exceptions, thread-safety properties, serialization forms, and non-obvious business constraints still require documentation.

## Portable Terminology

- Treat "two-party library" as an organization-internal or shared binary dependency consumed by other services, teams, or applications. If the user's project has no such category, apply those rules to published/shared libraries and compatibility boundaries.
- Treat "company/business-line GAV" as a placeholder for the user's own Maven coordinates. Follow the repository's existing group ID and artifact naming scheme rather than inventing company-specific names.
- Treat "online application" as production or production-like deployed software.
- Treat "RPC" as any cross-process service call, including HTTP clients, gRPC, Dubbo-like RPC, messaging request/reply, and internal SDK calls.

## Published API Boundaries

For libraries, SDKs, DTOs, public interfaces, cross-service contracts, or code consumed by another team/application:

- Treat public signatures, DTO fields, exception contracts, serialized forms, JSON fields, enum/string codes, Maven coordinates, and default interface methods as compatibility boundaries.
- Do not expose raw types, public mutable fields, mutable internals, implementation classes, ambiguous booleans, magic int modes, or Java native serialization as public contracts.
- Prefer stable code/string/value DTO fields at cross-application boundaries when unknown future values must be tolerated. Map to enums internally.
- Avoid adding side-effecting default methods to published interfaces. Prefer a new subinterface, utility method, adapter, or major-version migration unless representative implementations prove compatibility.
- For async APIs, define executor ownership, thread names, bounded queues/backpressure, timeouts, cancellation, shutdown, exception propagation, and return type semantics such as `CompletableFuture`.
- For callbacks/listeners, define lifecycle, add/remove APIs, thread-safety, weak-reference or cleanup policy, dispatch threading, exception isolation, and whether callbacks run under locks. Do not keep unbounded static listener lists.
- For cross-service payloads, prefer explicit schemas such as JSON, Protobuf, Avro, or similar project-standard formats. Define versioning, unknown-field behavior, backward/forward compatibility tests, and error-code contracts.

Common public API anti-patterns to flag immediately:

- `public List events;` or any raw/public mutable collection field.
- DTO field exposed as `SomeStatusEnum` when unknown future values must be tolerated by other services or clients.
- Methods like `send(String topic, boolean sync)` or `pay(String id, int mode)` where booleans or integers encode modes.
- Async APIs that call `new Thread(...).start()` directly, lack timeouts, cannot be cancelled, or do not define executor ownership.
- Static mutable callback/listener lists, especially when callbacks are invoked while holding a lock.
- Java native serialization used as an inter-service payload protocol.
- Published interface default methods that mutate implementor state, such as `deleteIf(Predicate)` or `removeIf(Predicate)`.

## Review Checklist

- Names, package layout, visibility, formatting, comments, and constants follow local style.
- Method signatures are small, typed by interfaces where possible, and avoid ambiguous booleans or long same-type parameter lists.
- Nullability, parameter validation, return contracts, and exception behavior are explicit.
- Collections, streams, generics, arrays, and varargs are type-safe and avoid known traps.
- Logging captures context and stack traces without sensitive data or excessive volume.
- Tests are automatic, independent, repeatable, assert-based, and placed under `src/test/java`.
- Database/ORM code avoids SQL injection, ambiguous columns, `select *`, unsafe dynamic SQL, accidental full updates/deletes, and stale `update_time`.
- Concurrent code names threads, bounds resources, releases locks/ThreadLocal values in `finally`, and avoids shared mutable data without synchronization.
- Public API, persistence format, serialized form, and external JSON/HTTP behavior are backward-compatible or explicitly migrated.
- Published library/API changes were checked for binary/source compatibility, DTO evolution, enum/code handling, exception signature changes, async lifecycle, and interface default-method hazards.

## References

- Read `references/java-handbook-rules.md` for portable team Java rules covering coding, exceptions/logging, testing, security, MySQL, engineering structure, and design.
- Read `references/effective-java-rules.md` for the 90 Effective Java practices covering API design, generics, enums, lambda/stream, exceptions, concurrency, and serialization.
