# Portable Java Team Rules

Use this self-contained reference when writing or reviewing Java business code, service code, tests, persistence code, security-sensitive code, Maven projects, and team engineering conventions. The rules are bundled here so they work without access to any external source files.

## Programming Rules

### Naming

- Do not start or end names with `_` or `$`.
- Do not mix Chinese and English, pinyin and English, or use Chinese identifiers.
- Do not use discriminatory or insulting terms; use terms such as `blockList`, `allowList`, `secondary`.
- Use `UpperCamelCase` for classes. Keep conventional abbreviations such as `DO`, `PO`, `DTO`, `BO`, `VO`, `UID` uppercase.
- Use `lowerCamelCase` for methods, parameters, member variables, and locals.
- Use uppercase words separated by `_` for constants, with complete semantics.
- Prefix abstract classes with `Abstract` or `Base`; suffix exception classes with `Exception`; suffix tests with `Test` and prefix them with the tested class name.
- Keep array brackets adjacent to the type: `int[] values`, not `int values[]`.
- Do not prefix POJO Boolean fields with `is`; map database `is_xxx` fields explicitly.
- Use all-lowercase packages; package segments should be singular natural English words.
- Avoid same names between parent/child fields, local blocks, and non-setter parameters versus fields.
- Avoid arbitrary abbreviations; prefer complete, self-explanatory word combinations.
- Put type/structure words at the end of names, such as `startTime`, `workQueue`, `nameList`.
- Include design-pattern role names where helpful, such as `OrderFactory`, `LoginProxy`, `ResourceObserver`.
- Do not add redundant `public` modifiers in interfaces; document interfaces with Javadoc.
- Define Service/DAO interfaces externally and suffix internal implementations with `Impl`; capability interfaces may use `-able`.
- Enum class names may use `Enum` suffix; enum constants use uppercase underscore style.
- Use stable layered method prefixes: `get`, `list`, `count`, `save`/`insert`, `remove`/`delete`, `update`.
- Use domain model suffixes consistently: `DO`, `DTO`, `BO`, `Query`, `VO`; do not name a class `xxxPOJO`.

### Constants

- Do not leave magic values directly in code.
- Use uppercase `L`, `D`, and `F` numeric suffixes; never lowercase `l`.
- Do not maintain all constants in one giant constants class; group by function and scope.
- Place constants at the narrowest useful sharing level: cross-application, application, submodule, package, or class.
- Use enums for variables constrained to a fixed range, especially when values have attributes.

### Formatting

- Empty blocks may be `{}`; non-empty blocks use K&R braces: left brace on same line, content on new lines.
- No spaces just inside parentheses; one space before `{`.
- Add one space between keywords such as `if`, `for`, `while`, `switch`, `do` and parentheses.
- Add spaces around binary and ternary operators.
- Use four spaces for indentation; do not use tab characters.
- Add exactly one space after `//`.
- No space between a cast and the casted expression: `(int)value`.
- Keep lines within 120 characters. Break after commas; keep operators and dots with the next line; do not break before `(`.
- Add a space after each comma in parameter definitions and invocations.
- Use UTF-8 and Unix line endings.
- Keep a method under 80 total lines where possible.
- Do not align assignment operators with extra spaces.
- Insert one blank line between different logical or business blocks; do not add multiple blank lines.

### OOP

- Access static members by class name, not through object references.
- Add `@Override` to every overridden method.
- Use varargs only for same-type, same-meaning parameters; keep varargs last; avoid `Object...`.
- Do not change signatures of externally used APIs or two-party library APIs. Deprecate with `@Deprecated` and document replacements.
- Do not use deprecated classes or methods without verifying the replacement.
- Call `equals` on constants or known non-null objects, or use `Objects.equals`.
- Compare all integer wrapper objects with `equals`, not `==`.
- Store money in the smallest currency unit as an integer type.
- Do not compare floating-point equality with `==` or wrapper `equals`; use tolerance or `BigDecimal`.
- Compare `BigDecimal` with `compareTo`, not `equals`.
- Match DO property types to database column types.
- Do not construct `BigDecimal` from `double`; use string constructors or `BigDecimal.valueOf`.
- Use wrapper types for all POJO fields and RPC parameters/returns; use primitives for local variables when null is not meaningful.
- Do not assign default values to POJO fields.
- Do not change `serialVersionUID` when adding compatible serialized fields; change it only for intentionally incompatible upgrades.
- Do not put business logic in constructors; use an initialization method when needed.
- POJOs must implement useful `toString`, including superclass state when relevant.
- Do not define both `isXxx()` and `getXxx()` for the same POJO property.
- Check `String.split` results before indexed access, especially trailing delimiters.
- Place overloads together.
- Order methods: public/protected first, private helpers next, getters/setters last.
- Keep getters/setters free of business logic.
- Use `StringBuilder.append` in loops.
- Use `final` when a class, method, member, or local must not change or be overridden.
- Be cautious with `Object.clone`; it is shallow unless carefully overridden.
- Keep member and method access as strict as possible.

### Date And Time

- Use lowercase `yyyy` for calendar year; do not use uppercase `YYYY` for date formatting.
- Distinguish `M` month from `m` minute, and `H` 24-hour from `h` 12-hour.
- Use `System.currentTimeMillis()` for current milliseconds; use `System.nanoTime()` for nanosecond elapsed time; prefer `Instant` for JDK 8 statistical time cases.
- Do not use `java.sql.Date`, `java.sql.Time`, or `java.sql.Timestamp` in program logic.
- Do not hard-code a year as 365 days.
- Consider leap-day behavior around February 29.
- Prefer enum/calendar constants for months; remember legacy `Date`/`Calendar` month values are 0-based.

### Collections

- If overriding `equals`, override `hashCode`. Objects used in `Set` or as `Map` keys must implement both correctly.
- Use `isEmpty()` instead of `size() == 0`.
- When using `Collectors.toMap`, provide a merge function for duplicate keys.
- Remember `Collectors.toMap` throws NPE if mapped values are null.
- Do not cast `subList` results to `ArrayList`.
- Do not add to `keySet()`, `values()`, or `entrySet()` views.
- Treat `Collections.emptyList()`, `singletonList()`, and similar results as immutable.
- Do not structurally modify a parent list while using a `subList`.
- Convert collections to arrays with `toArray(new T[0])`.
- Null-check inputs passed to `Collection.addAll`.
- Do not modify lists returned by `Arrays.asList`; they are backed by the original array.
- Apply PECS: `? extends T` for producers, `? super T` for consumers.
- When assigning raw collections to generic collections, validate element types before use.
- Do not add/remove collection elements inside for-each loops; use an iterator and lock it for concurrent operations.
- Comparators must be antisymmetric, transitive, and consistent for equality.
- Use diamond syntax on JDK 7+.
- Specify initial collection capacity when size is known; use default `16` if unsure for `HashMap`.
- Iterate maps with `entrySet` or `Map.forEach` when both key and value are needed.
- Know null support for map implementations: `Hashtable` and `ConcurrentHashMap` reject null keys/values; `TreeMap` rejects null keys; `HashMap` allows null key/value.
- Use ordered/sorted collections deliberately; do not rely on unordered or unstable iteration order.
- Use `Set` to deduplicate or test membership instead of repeated `List.contains`.

### Concurrency

- Singleton objects and their methods must be thread-safe.
- Name threads and thread pools meaningfully.
- Provide threads through thread pools; do not explicitly create uncontrolled application threads.
- Do not create thread pools through `Executors` in production rules; use `ThreadPoolExecutor` so queue, size, and rejection policy are explicit.
- `SimpleDateFormat` is not thread-safe; use JDK 8 time classes, local instances, locks, or `ThreadLocal`.
- Always remove custom `ThreadLocal` values in `finally`, especially in pooled threads.
- Keep lock scope small. Prefer lock-free structures, block locks over method locks, and object locks over class locks where appropriate.
- Keep lock acquisition order consistent across resources.
- For blocking locks, call `lock()` before `try` and `unlock()` in `finally`.
- For `tryLock`, only execute and unlock after confirming the current thread holds the lock.
- Prevent lost updates with application, cache, or database locks; use optimistic locking with version and at least three retries when conflicts are rare.
- Prefer `ScheduledExecutorService` over `Timer`; one uncaught `TimerTask` exception can stop all timer tasks.
- Use pessimistic locking for financial sensitive data. Follow lock, recheck, update, release.
- With `CountDownLatch`, ensure `countDown` is called in `finally` or after catching exceptions.
- Avoid shared `Random` under concurrency; use `ThreadLocalRandom`.
- For double-checked lazy initialization, mark the field `volatile`.
- `volatile` solves visibility for one-writer/many-reader cases, not compound updates; use atomics or `LongAdder`.
- Avoid concurrent `HashMap` mutation; resizing can cause severe issues.
- Use `static` for `ThreadLocal` fields; remember it does not make shared object updates safe.

### Control Flow

- Every `switch` case must end with `continue`, `break`, `return`, or a comment explaining fall-through. Always include `default` last.
- Null-check external `String` values before switching on them.
- Always use braces for `if`, `else`, `for`, `while`, and `do`, even for one line.
- Beware ternary expressions that trigger auto-unboxing NPE.
- In high concurrency, avoid equality as a stop condition; use ranges.
- Add a blank line after guard `return`/`throw` blocks when methods exceed 10 lines.
- Prefer guard clauses for exceptional branches. Avoid more than three nested `if`/`else` levels.
- Assign complex conditions to well-named boolean variables.
- Do not hide assignments inside expressions or conditions.
- Move object creation, variable definition, database connection acquisition, and unnecessary try/catch out of loops when possible.
- Prefer positive conditions over negated logic.
- Protect public interfaces and batch operations with input limits.
- Validate parameters for low-frequency methods, expensive methods, high-stability methods, external APIs, and sensitive permission entries.
- Parameter validation may be omitted for high-frequency internal/private/lower-level methods only when callers already guarantee validity and documentation says so.

### Comments

- Use Javadoc for classes, fields, and methods, not `//`.
- Abstract methods and interface methods require Javadoc explaining purpose, params, returns, exceptions, and implementation requirements.
- Add creator and date when the project convention requires it.
- Put single-line comments above the statement; align block comments with code.
- Document every enum constant's purpose.
- Prefer clear Chinese comments over poor English; keep technical terms in English.
- Update comments with code changes.
- Delete unused fields, methods, inner classes, parameters, and locals.
- Do not leave commented-out code without a reason; delete dead code.
- Comments should explain design intent, business meaning, constraints, and non-obvious context, not restate obvious code.
- Mark `TODO` and `FIXME` with owner and time, and clean them regularly.

### Frontend And HTTP Contracts

- API contracts must specify protocol, domain, path, method, request content, status codes, and response body.
- Production protocols must use HTTPS.
- Resource paths should be nouns, lowercase, underscore-separated, and not suffixed with `.json` or `.xml`.
- GET reads, POST creates, PUT updates, DELETE deletes.
- URL parameters must not carry sensitive data; body requests must set `Content-Type`.
- Empty frontend list responses return `[]` or `{}`, not `null`.
- Server errors returned to frontend must include HTTP status, `errorCode`, `errorMessage`, and user-facing tip.
- JSON keys use lowerCamelCase and complete English semantics.
- Keep `errorMessage` useful for tracing but free of sensitive data.
- Return very large integers as strings to JavaScript clients.
- Keep URL parameters under 2048 bytes.
- Control body size; default server limits may reject oversized requests.
- Pagination: values less than 1 become page 1; values greater than total pages return last page.
- Internal redirect uses forward; external redirect uses a unified URL broker.
- Mark cacheability explicitly.
- Prefer JSON over XML.
- Use `yyyy-MM-dd HH:mm:ss` and GMT for frontend/backend time exchange unless local project rules differ.
- Prefer versioning in HTTP headers rather than path segments.

### Other Coding Rules

- Precompile regular expressions instead of compiling inside method bodies.
- Avoid Apache BeanUtils for copying; prefer faster shallow-copy alternatives when appropriate.
- Velocity accesses POJO properties by property name; Boolean wrappers prefer `getXxx`.
- Use `$!{var}` for backend variables in Velocity pages.
- Understand `Math.random()` returns `0 <= x < 1`; use `Random.nextInt`/`nextLong` for integer randoms.
- Enum attributes must be private and immutable.
- Do not put complex logic in view templates.
- Specify initial sizes for data structures when possible.
- Remove stale code and configuration. If temporarily commenting code, explain why.

## Exceptions And Logging

### Error Codes

- Error codes must support quick source tracing and standardized communication.
- Do not encode version or log level in error codes.
- Use `00000` when everything is normal but a code is required.
- Use five-character string codes: source `A`/`B`/`C` plus four digits. `A` is user error, `B` current-system error, `C` third-party error.
- Allocate codes centrally and permanently; do not bind codes to organization structure.
- Do not invent new codes casually; reuse semantically close existing codes.
- Do not display raw error codes as user-facing tips.
- Put business detail in `error_message`, not in the code itself.
- Translate third-party errors upward, preserving original third-party codes in messages.
- Macro codes include `A0001`, `B0001`, `C0001`.
- Error code suffixes are unrelated to HTTP status codes.

### Exception Handling

- Do not catch avoidable runtime exceptions such as NPE or index errors; pre-check instead.
- Do not use exceptions for control flow.
- Separate stable and unstable code; catch unstable code by specific exception type where possible.
- Catch only to handle; otherwise propagate. Top-level business layers must translate to user-understandable results.
- If a caught exception should roll back a transaction, roll back manually when the framework will not.
- Close resources in `finally` or, preferably, use `try-with-resources`.
- Do not `return` from `finally`.
- Catch types must match thrown types or a true parent type.
- For RPC, two-party packages, reflection, or generated classes, consider catching `Throwable` at isolation boundaries to prevent linkage errors from escaping.
- Nullable returns are allowed only with clear documentation of when `null` is returned; callers still own NPE prevention.
- Guard against NPE from unboxing, database nulls, nullable collection elements, remote calls, session data, and chained calls.
- Distinguish checked and unchecked exceptions; avoid directly throwing `RuntimeException`, `Exception`, or `Throwable`; use meaningful business exceptions.
- External HTTP/API interfaces use error codes; internal application flow may use exceptions; cross-application RPC may use `Result` with success flag, code, and message.

### Logging

- Use a logging facade such as SLF4J or JCL, not Log4j/Logback APIs directly.
- Keep application logs at least 15 days; security and sensitive-operation logs at least six months with backup when legally required.
- Name extension logs as `appName_logType_logName.log`.
- Use placeholders for log variables.
- Guard trace/debug/info logs with level checks when parameter construction is costly.
- Prevent duplicate logs with logger additivity settings.
- Do not use `System.out`, `System.err`, or `e.printStackTrace` in production.
- Error logs must include scene/context information and stack traces.
- Do not serialize arbitrary objects with JSON utilities for logs; log relevant fields or safe `toString`.
- Log thoughtfully: no production debug logs; selective info logs; careful warn logs.
- Use warn for user input mistakes unless system logic has failed.
- Use English log messages when clear; Chinese is acceptable when English would be ambiguous.
- Desensitize sensitive user information in logs.

## Unit Testing

- Good unit tests follow AIR: Automatic, Independent, Repeatable.
- Tests must be automatic, non-interactive, and assert-based; do not use `System.out` for human verification.
- Tests must not call each other or depend on execution order.
- Tests must be repeatable and isolated from external environment.
- Unit test granularity is at most class level, usually method level.
- Incremental code in core business/app/module areas must pass unit tests.
- Put tests under `src/test/java`, not production directories.
- Baseline target: 70 percent statement coverage; core modules should reach 100 percent statement and branch coverage when practical.
- Use BCDE: Border, Correct, Design, Error.
- Prepare database data through program/import, not manual assumptions.
- Database tests should roll back or mark test data with clear prefixes/suffixes.
- Refactor untestable code rather than writing nonstandard tests.
- Determine unit-test scope during design review.
- Finish unit tests before handoff/testing phase, not after release.
- Avoid constructors doing too much, excess global/static state, external dependencies, and deep conditionals; these harm testability.
- Unit tests are developer responsibility and must be maintained.

## Security

- Enforce authorization for user-private pages and functions.
- Desensitize sensitive user data in displays.
- Use parameter binding or metadata field limits for SQL; never concatenate user input into SQL.
- Validate all user request parameters.
- Filter or escape user data before outputting to HTML.
- Apply CSRF validation to forms and AJAX submits.
- Whitelist external redirect targets.
- Implement anti-replay controls for SMS, email, phone, ordering, payment, and similar platform resources.
- Strictly check upload file size and type.
- Encrypt passwords in configuration files.
- For user-generated content such as posts, comments, and messages, implement anti-spam and forbidden-word controls.

## MySQL Rules

### Table Design

- Boolean database fields use `is_xxx` and `unsigned tinyint`; POJO Boolean fields do not use `is` prefix and need explicit mapping.
- Table and column names use lowercase letters/digits, no leading digit, no uppercase, and no ambiguous underscore-number patterns.
- Table names are singular nouns.
- Do not use reserved words.
- Name indexes with `pk_`, `uk_`, and `idx_` prefixes.
- Use `decimal` for decimal values; do not use `float`/`double`.
- Use `char` for nearly fixed-length strings.
- Keep `varchar` length under 5000; use separate text table for larger content.
- Every table has `id`, `create_time`, and `update_time`; `id` is unsigned bigint primary key, single-table auto-increment step 1.
- Use logical delete, not physical delete, unless the project explicitly allows otherwise.
- Prefer table names like `business_purpose`.
- Keep database names aligned with application names.
- Update comments when field meanings or states change.
- Redundant fields are allowed only for performance, not frequently modified, not unique, not long varchar/text, and with consistency considered.
- Split database/tables only when data volume justifies it, such as over 5 million rows or 2GB per table.
- Choose compact types and unsigned ranges to save space and improve query speed.

### Indexes

- Business-unique fields must have unique indexes.
- Avoid joins over more than three tables. Join columns must have matching types and indexes.
- Specify index length for `varchar` indexes based on selectivity.
- Do not use left or full fuzzy search on normal indexes; use a search engine if needed.
- Align `order by` with composite index order.
- Use covering indexes to avoid back-to-table reads.
- Use delayed join/subquery strategies for deep pagination.
- SQL optimization goal: at least `range`, preferably `ref`, best `const`.
- Put high-selectivity columns leftmost, but equality columns should precede range columns in mixed conditions.
- Avoid implicit type conversion that invalidates indexes.
- Do not create indexes indiscriminately, avoid being too stingy, and use unique indexes instead of only app-layer uniqueness.

### SQL

- Use `count(*)` for row counts.
- Understand `count(distinct ...)` ignores null combinations.
- `sum(col)` returns null when all values are null; guard with `IFNULL`.
- Use `ISNULL()` for null checks when appropriate; direct comparisons with null do not work.
- If pagination count is zero, return immediately.
- Do not use foreign keys or cascade; handle relationships in the application layer.
- Do not use stored procedures.
- Before data correction updates/deletes, run `select` and confirm.
- Qualify columns with table aliases in multi-table queries and changes.
- Use `as` for aliases and prefer `t1`, `t2`, `t3` in order.
- Avoid `in` when possible; keep `in` element count under 1000 if unavoidable.
- Use `utf8mb4` for international text and emoji.
- Avoid `TRUNCATE TABLE` in application code.

### ORM

- Do not use `*` in select lists; list required columns.
- Map POJO Boolean property names and database `is_` fields explicitly.
- Use `resultMap`, not `resultClass`, and maintain one result map per table.
- Use `#{}` parameters, not `${}` or legacy `#param#`.
- Do not use iBATIS memory-pagination overloads that fetch all rows.
- Do not return raw `HashMap` or `Hashtable` as query result objects.
- Update `update_time` on every row update.
- Do not write giant update APIs that update unchanged fields.
- Do not abuse `@Transactional`; consider QPS, rollback, cache/search/message compensation.
- Understand dynamic SQL tags such as `isEqual`, `isNotEmpty`, `isNotNull`.

## Engineering Structure

### Layering

- Recommended layers include Open API, terminal display, Web, Service, Manager, DAO, third-party service, and external data interfaces.
- DAO catches broad persistence exceptions and wraps them as DAO exceptions without duplicate logging when Manager/Service will log.
- Service logs errors with parameters and context.
- Manager follows DAO or Service logging depending on deployment.
- Web layer should not continue throwing to a higher UI layer; show friendly errors when rendering cannot continue.
- Open API layer returns error code and error message.
- Domain models: DO maps database rows, DTO crosses Service/Manager boundaries, BO represents business objects, Query carries query conditions, VO goes to views.
- If a query has more than two parameters, wrap them in a Query object; do not use Map.

### Two-Party Dependencies And Maven

- Treat two-party dependencies as organization-internal or otherwise shared binary dependencies consumed across modules, services, teams, or applications.
- GAV follows the user's existing organization/project group ID and product-line/module artifact ID conventions.
- Version format is `major.minor.patch`, starting at `1.0.0`.
- Production applications must not depend on SNAPSHOT versions except emergency/security packages.
- Adding/upgrading a library must preserve unrelated dependency mediation or be explicitly evaluated.
- Two-party libraries may use enums in parameters but should not expose enums or POJOs containing enums in return values at compatibility boundaries.
- Custom library builds append `-englishDescription[index]`.
- Use one version variable for a family of related dependencies.
- Do not allow same GroupId/ArtifactId with different versions across subprojects.
- Core infrastructure should be cautious about third-party implementations.
- Put dependencies in `<dependencies>` and version mediation in `<dependencyManagement>`.
- Avoid adding configuration options to two-party libraries.
- Avoid unstable utility packages.
- Library publishers should keep APIs/dependencies lean and versions stable/traceable.
- Treat DTO fields, method signatures, exception contracts, enum/code values, dependency versions, and serialized payloads as compatibility contracts once published.
- Do not expose enums in return DTOs at compatibility boundaries when future unknown values are possible; expose stable codes and map to enums internally.
- Avoid public mutable fields and raw collections in published APIs.
- Avoid same-arity overloads, boolean mode flags, and int mode flags in public APIs; prefer explicit method names, option objects, builders, or stable codes.
- Do not add side-effecting default methods to published interfaces unless representative implementations and clients have been compatibility-tested.

### Cross-Service Payloads

- Prefer explicit interoperable schemas such as JSON, Protobuf, Avro, or a project-standard format over Java native serialization.
- Define versioning, unknown-field behavior, required/optional fields, default values, and backward/forward compatibility tests.
- Define error-code contracts for external interfaces and Result-style contracts for cross-application calls when the project uses them.
- Never deserialize untrusted payloads with Java native serialization without filters and a documented trust boundary.

### Async APIs And Callbacks

- Do not create raw threads for async API work. Use injected or owned executors with named threads, bounded resources, and clear shutdown ownership.
- Remote async operations must have connect/read/overall timeouts, cancellation behavior, and exception propagation semantics.
- Prefer returning explicit handles such as `CompletableFuture`, project task abstractions, or cancellable handles instead of fire-and-forget APIs.
- Listener/callback registries need `add` and `remove`, lifecycle documentation, thread-safety policy, cleanup/weak-reference strategy when appropriate, and dispatch exception isolation.
- Do not keep unbounded static listener lists. Avoid holding locks while invoking foreign callback code.

### Server

- All remote calls must have timeout settings.
- Configure client method/interface timeouts deliberately.
- For high-concurrency servers, tune TCP `time_wait` and file descriptor limits when needed.
- Enable heap dumps on OOM.
- Set production JVM `Xms` and `Xmx` equal when appropriate.
- Isolate slow services with independent thread pools.
- Internal redirect uses forward; external redirect should use a URL broker.

## Design Rules

- Storage scheme and core data structure designs require review and documentation.
- Use use-case diagrams when user types exceed one and use cases exceed five.
- Use state diagrams when business object states exceed three.
- Use sequence diagrams when a call chain involves more than three objects.
- Use class diagrams when more than five model classes have complex dependencies.
- Use activity diagrams for complex collaboration among more than two objects.
- Identify weak dependencies and design degradation/emergency plans for core availability.
- Architecture design should define system boundaries, module relationships, evolution principles, and nonfunctional requirements.
- During requirements and design, evaluate abnormal flows and business boundaries.
- Follow single responsibility for classes.
- Prefer aggregation/composition over inheritance; inheritance must obey Liskov substitution.
- Depend on abstractions and interfaces for extensibility and maintenance.
- Design for open extension and closed modification.
- Apply DRY by extracting common behavior into modules, configs, classes, or methods when meaningful.
- Agile development still requires necessary design and documentation for core points.
- Design documents clarify requirements, logic, and future maintenance, not just coding instructions.
- Extensibility means finding and isolating variation points.
- Design means identifying and expressing system difficulties clearly.
- Code is not complete documentation; dependencies, business scenarios, and nonfunctional requirements need documents.
- Accessibility design should support tab focus, natural focus order, alternatives to visual captcha, and explicit roles for custom controls.
