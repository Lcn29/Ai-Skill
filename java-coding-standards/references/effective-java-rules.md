# Effective Java Rules

Use this self-contained reference for Java API design, class design, generics, enums, lambda/stream, method contracts, exception design, concurrency, performance, and serialization. The rules are bundled here so they work without access to any external source files.

## Object Creation And Resource Management

1. Consider static factory methods before public constructors. Use named factories, instance control, subtype returns, and provider frameworks when they improve API clarity.
2. Use Builder when constructors or static factories have many parameters, especially optional or same-type parameters.
3. Implement singletons with private constructors plus public final field/static factory when appropriate; prefer single-element enum singletons when possible, especially for serialization/reflection safety.
4. For non-instantiable utility classes, add a private constructor that throws if called. Do not rely on `abstract`.
5. Prefer dependency injection over hard-wired resources, singletons, or static utilities when behavior depends on underlying resources.
6. Avoid unnecessary object creation: reuse immutable/expensive objects, cache compiled patterns, avoid accidental boxing, but do not build object pools for lightweight objects.
7. Eliminate obsolete object references when managing memory manually, such as arrays, caches, listeners, and callbacks. Prefer narrow scopes and weak references or eviction where appropriate.
8. Avoid finalizers and cleaners except as noncritical native-resource safety nets; they are unpredictable, slow, and risky.
9. Use `try-with-resources` instead of `try-finally` for resources that implement `AutoCloseable`; implement `AutoCloseable` for closeable resource classes.

## Common Methods

10. Override `equals` only when logical equality is needed. Preserve reflexivity, symmetry, transitivity, consistency, and non-nullity; compare all significant fields.
11. Always override `hashCode` when overriding `equals`; equal objects must have equal hash codes.
12. Override `toString` for instantiable classes unless a superclass already provides a good one. Return concise, useful, human-readable state. Document format if clients may rely on it.
13. Override `clone` cautiously. Prefer copy constructors or copy factories. If supporting `Cloneable`, call `super.clone`, repair mutable internals, avoid overridable calls, and synchronize when needed.
14. Implement `Comparable` for value classes with natural ordering. Make ordering consistent with equality where practical and use `compare` methods rather than `<`/`>`.

## Classes And Interfaces

15. Minimize accessibility of classes and members. Keep APIs small and deliberate. Public static final fields must refer to immutable values.
16. In public classes, use accessors instead of public fields. Package-private or private nested classes may expose fields when appropriate.
17. Minimize mutability. Make classes immutable unless mutability is necessary; avoid reflexively adding setters.
18. Prefer composition and forwarding over inheritance unless there is a true subtype relationship and the superclass is designed for inheritance.
19. Either design and document inheritance, or prohibit it with `final` or inaccessible constructors.
20. Prefer interfaces to abstract classes for defining types with multiple implementations. Provide skeletal implementations when useful.
21. Design interfaces carefully for future evolution. Default methods can break existing implementations; test multiple implementations and clients before release.
22. Use interfaces only to define types. Do not use constant interfaces.
23. Prefer class hierarchies over tagged classes. Replace explicit type tags with subtype polymorphism where appropriate.
24. Prefer static member classes when a nested class does not need an enclosing instance. Nonstatic member classes retain hidden outer references and can leak memory.
25. Limit source files to one top-level class or interface. Use static member classes or separate files.

## Generics

26. Do not use raw types except for legacy interop. Prefer parameterized types or unbounded wildcards.
27. Eliminate unchecked warnings. If suppression is proven safe, use `@SuppressWarnings("unchecked")` in the smallest scope and document why.
28. Prefer lists to arrays when working with generics. Arrays are covariant/reified; generics are invariant/erased.
29. Prefer generic types to types that force clients to cast.
30. Prefer generic methods to methods that force clients to cast.
31. Use bounded wildcards to increase API flexibility. Remember PECS: producer extends, consumer super. Comparables and comparators are consumers.
32. Combine generics and varargs carefully. Ensure type safety before using `@SafeVarargs`.
33. Use type-safe heterogeneous containers when a container needs values of many unrelated types; use type tokens such as `Class<T>` or custom key types.

## Enums And Annotations

34. Use enum types instead of int or string constants. Attach data and behavior to enum constants when appropriate; use constant-specific methods or strategy enum patterns for varying behavior.
35. Use instance fields instead of enum ordinals for associated values. Avoid `ordinal` except for enum-based infrastructure like `EnumSet`/`EnumMap`.
36. Use `EnumSet` instead of bit fields for sets of enum values.
37. Use `EnumMap` instead of ordinal indexing. For multidimensional enum relationships, use nested `EnumMap`.
38. Simulate extensible enums with interfaces when clients need to provide additional enum-like values.
39. Prefer annotations to naming patterns for tool/framework metadata. Use predefined Java and static-analysis annotations when useful.
40. Always use `@Override` when intending to override a superclass or interface method.
41. Use marker interfaces to define types when type information matters; use marker annotations for non-type program elements or annotation-heavy frameworks.

## Lambdas And Streams

42. Prefer lambdas to anonymous classes for small function objects. Use anonymous classes only when a non-functional-interface instance is required.
43. Prefer method references to lambdas when they are shorter and clearer; otherwise keep the lambda.
44. Prefer standard functional interfaces from `java.util.function`. Define custom functional interfaces only when the domain semantics or checked exceptions justify it.
45. Use streams judiciously. Some tasks are clearer with iteration, some with streams, and many with both. Try both when uncertain.
46. Prefer side-effect-free functions in streams. Use `forEach` mainly to report results, not to perform computation. Learn collectors such as `toList`, `toSet`, `toMap`, `groupingBy`, and `joining`.
47. Prefer `Collection` over `Stream` as a return type when callers may want both iteration and stream processing. Return stream or iterable only when collection is impractical.
48. Use parallel streams only with evidence that correctness is preserved and performance improves under realistic measurement.

## Methods And General Programming

### Published API Compatibility Addendum

- Treat public signatures, generic signatures, exposed fields, checked exceptions, DTO shapes, serialized forms, and default interface methods as long-lived API.
- Prefer explicit option objects/builders over ambiguous booleans or int modes in public methods.
- Avoid raw/public mutable collections in public APIs; return typed immutable views or defensive copies.
- Avoid exposing enums across compatibility boundaries when future unknown values must be tolerated; expose stable codes and map internally.
- Prefer schema-based cross-service formats over Java serialization, with versioning and unknown-field behavior documented.
- For async public APIs, document executor ownership, cancellation, timeout, shutdown, and exception semantics.
- For callbacks, avoid static global registries; document lifecycle and thread-safety and avoid invoking foreign code while holding locks.

49. Check parameter validity at method/constructor entry and document restrictions.
50. Make defensive copies when accepting or returning mutable components unless trusted clients and documentation justify otherwise.
51. Design method signatures carefully: choose clear names, avoid too many convenience methods, keep parameter lists short, use helper parameter classes/builders, prefer interface parameter types, and prefer two-value enums over unclear booleans.
52. Use overloading judiciously. Avoid multiple overloads with the same parameter count; ensure identical arguments behave identically across overloads.
53. Use varargs judiciously. Put required parameters before varargs and consider performance.
54. Return empty arrays or collections, not `null`, for empty results.
55. Return `Optional` judiciously for absent return values that callers must handle. Do not use `Optional` elsewhere, and avoid it in performance-critical paths.
56. Write documentation comments for all exported API elements, including contracts, side effects, thread-safety, and exceptions.
57. Minimize local variable scope. Declare variables at first use, initialize them, prefer `for` to `while` when loop variables are local, and keep methods small.
58. Prefer for-each loops to traditional for loops unless you need the iterator, index, or synchronized iterator removal.
59. Know and use libraries. Do not reimplement common facilities without checking the standard or established library first.
60. Avoid `float` and `double` when exact answers are required. Use `BigDecimal`, `int`, or `long` depending on range, decimal scale, performance, and rounding needs.
61. Prefer primitive types to boxed primitives unless null or generic use is required. Beware `==`, unboxing NPE, and accidental boxing costs.
62. Avoid strings where better types exist. Prefer primitives, enums, value classes, or aggregate types over stringly typed code.
63. Beware performance of repeated string concatenation. Use `StringBuilder`, char arrays, or streaming output for many concatenations.
64. Refer to objects by interfaces when suitable. Use the lowest class in the hierarchy only when no appropriate interface exists or extra class-specific methods are required.
65. Prefer interfaces to reflection. If reflection is necessary for unknown classes, instantiate reflectively and then interact through known interfaces or superclasses.
66. Use native methods judiciously. Avoid them for performance unless measured; isolate and test native code heavily.
67. Optimize judiciously. First write good code, design APIs/protocols/formats with performance in mind, then measure, profile, and improve algorithms before low-level tweaks.
68. Follow widely accepted Java naming conventions unless strong local tradition says otherwise.

## Exceptions

69. Use exceptions only for exceptional conditions. Do not use them for normal control flow and do not design APIs that force clients to do so.
70. Use checked exceptions for recoverable conditions and runtime exceptions for programming errors. Provide recovery information on checked exceptions.
71. Avoid unnecessary checked exceptions. If callers cannot recover, use unchecked exceptions; if absence is recoverable, consider `Optional`.
72. Prefer standard exceptions. Common choices include `IllegalArgumentException`, `IllegalStateException`, `NullPointerException`, `IndexOutOfBoundsException`, `ConcurrentModificationException`, and `UnsupportedOperationException`. Do not throw raw `Exception`, `RuntimeException`, `Throwable`, or `Error`.
73. Throw exceptions appropriate to the abstraction. Translate lower-level exceptions and use exception chaining for root-cause analysis.
74. Document every exception a method can throw with `@throws`; declare each checked exception separately and do not declare unchecked exceptions in `throws`.
75. Include failure-capture information in exception detail messages, but never include passwords, keys, or secrets. Provide accessors for failure data when useful.
76. Strive for failure atomicity. Methods that throw should leave objects unchanged, or document the resulting state.
77. Do not ignore exceptions. If an exception is intentionally ignored, comment why and name the variable `ignored`; logging may still be wise.

## Concurrency

78. Synchronize access to shared mutable data. Every reading or writing thread must synchronize, or use a correct volatile/atomic/concurrent design.
79. Avoid excessive synchronization. Do not call foreign methods from synchronized regions; keep synchronized work minimal and document synchronization policy.
80. Prefer executors, tasks, and streams to raw threads. Separate work units (`Runnable`/`Callable`) from execution policy (`ExecutorService`).
81. Prefer concurrency utilities to `wait`/`notify`. If maintaining legacy `wait`, call it in a `while` loop and usually prefer `notifyAll`.
82. Document thread-safety properties. State whether a class is immutable, unconditionally thread-safe, conditionally thread-safe, not thread-safe, or thread-hostile. Document required external locks.
83. Use lazy initialization judiciously. Prefer normal initialization; use holder class idiom for static fields and double-check idiom for instance fields only when necessary.
84. Do not depend on the thread scheduler. Do not use `Thread.yield` or priorities for correctness.

## Serialization

85. Prefer alternatives to Java serialization, such as JSON or protobuf. Do not deserialize untrusted data; use filters if unavoidable.
86. Implement `Serializable` very cautiously. It freezes serialized form as API, increases bug/security risk, and adds compatibility testing burden. Extendable classes and interfaces should rarely be serializable; inner classes should not be.
87. Consider custom serialized forms. Use default serialization only when it matches logical state. Mark nonlogical fields `transient`, declare `serialVersionUID`, and synchronize serialization consistently with other state reads.
88. Write `readObject` defensively. Treat it like a public constructor: validate invariants, defensively copy mutable fields before checks, and do not call overridable methods.
89. Prefer enum types to `readResolve` for instance control. If `readResolve` is required, ensure object-reference fields are `transient` or otherwise safe.
90. Consider serialization proxy instead of serialized instances when a non-extendable class with important invariants needs custom `readObject`/`writeObject`.
