# Contract-first API generation

## Source of truth

ASP.NET Core endpoint metadata and serialized DTOs are the source of truth. Generate OpenAPI from backend code, with stable unique operation IDs, explicit schemas, and documented success/error/bodyless responses. Pin the resulting contracts to a repository revision or verified immutable artifact. Record provenance so a frontend change can identify the backend contract it consumes. For a gateway/BFF, generate browser clients from the contract the browser actually calls; do not expose internal services just to simplify generation.

Inspect existing contract generation, Orval configuration, generated output, base URLs, authentication, and package versions first. If an operation or schema is missing or inaccurate, fix/regenerate the backend contract within authorized scope or report the missing dependency. Do not guess an endpoint or handwrite a fallback client to get around an unavailable specification. External protocols outside OpenAPI coverage require an explicit documented boundary and their real protocol/SDK; they are not permission to duplicate covered operations.

## Generate, then consume

Use Orval to generate TypeScript models, request functions, and TanStack Query hooks, with Fetch preferred as the HTTP transport. Keep each API contract's generated files in its own `generated/` subtree. Never manually edit them or duplicate their API DTO definitions. UI view models and form schemas are allowed when they represent distinct UI concerns; map explicitly to/from generated types.

Configure centralized authentication, base URL resolution, and error handling outside `generated/`. Use the mutator/custom-client interface compatible with the installed Orval version. Verify that interface by generating and compiling a small real contract before adopting configuration from documentation. Preserve request method, body, headers, serialization, credentials policy, and AbortSignal. Do not concatenate undocumented paths in the transport.

Fetch does not reject HTTP error status responses automatically. Ensure the generated/client transport's documented behavior produces rejected query/mutation promises for failures. Preserve status, safe error details, and documented validation/Problem Details information; handle non-JSON failures and bodyless responses without unconditional JSON parsing. Account for response envelopes or status-discriminated unions actually emitted by that generator version instead of assuming `data` is always an unwrapped success DTO.

Use the established session/token flow. Do not add persistent token storage or change credential modes casually. Never send credentials to arbitrary origins. Cookie sessions require the application's CSRF protections for state-changing requests. Bound token refresh and retries; avoid refresh loops and duplicate writes. Retry mutations only when the operation's idempotency contract makes that safe. Cancellation should not appear as a user-facing server failure.

## Server state and tenancy

Consume actual generated names and query-key helpers. Configure staleness, retries, error presentation, and cache invalidation to match business needs. Distinguish initial loading from background refresh, and preserve valid displayed data where appropriate. Invalidate affected query families after successful mutations; use optimistic changes only with a defined rollback and reconciliation strategy.

Scope cache identity to the authenticated tenant/user context when those contexts can return different data for the same operation. If generated keys omit context, use a consistent supported key extension or an isolated QueryClient per context. Clear/cancel relevant cached and in-flight work on logout or tenant/account changes, and prevent late results from repopulating the wrong session. Do not display retained data from a prior tenant. The backend must independently enforce tenant isolation and permissions.

## Reproducible verification

Regenerate on every backend contract change with a pinned generator and lockfile. Review diffs for removed operations, schema/nullability/required-field changes, enum additions, error behavior, and other compatibility risks. Run type checking and tests using the regenerated code. Treat generation as build tooling: use trusted pinned inputs and configuration.

In CI, regenerate from pinned contracts in a clean checkout and fail if committed generated output changes or new untracked generated files appear. `git diff --exit-code` alone misses untracked files. Scope status checks to the contract/output paths and include deletions and newly generated schemas. If backend and frontend share a repository, also regenerate and compare the backend specification so stale source-to-contract drift cannot pass. Across repositories, use a versioned contract publication/update workflow and verify provenance/checksums.

## Official documentation

- [ASP.NET Core OpenAPI generation](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/aspnetcore-openapi?view=aspnetcore-10.0)
- [Orval quick start](https://orval.dev/docs/quick-start/), [TanStack Query generation](https://orval.dev/docs/guides/react-query/), [Fetch client](https://orval.dev/docs/guides/fetch-client/), [custom HTTP client](https://orval.dev/docs/guides/custom-client/)
- [TanStack Query keys](https://tanstack.com/query/latest/docs/framework/react/guides/query-keys)
