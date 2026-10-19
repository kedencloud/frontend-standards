# Contract-first API generation

## Source of truth

ASP.NET Core endpoint metadata and serialized DTOs are the source of truth. Generate OpenAPI from backend code, with stable unique operation IDs, explicit schemas, and documented success/error/bodyless responses. Use the latest running gateway contract during the manual generation step and review/commit the generated changes with the feature. Record the consuming gateway and generation command in the application README; do not introduce contract checksums or a second contract publication system. For a gateway/BFF, generate browser clients from the contract the browser actually calls; do not expose internal services just to simplify generation.

Inspect existing contract generation, Orval configuration, generated output, base URLs, authentication, and package versions first. If an operation or schema is missing or inaccurate, fix/regenerate the backend contract within authorized scope or report the missing dependency. Do not guess an endpoint or handwrite a fallback client to get around an unavailable specification. External protocols outside OpenAPI coverage require an explicit documented boundary and their real protocol/SDK; they are not permission to duplicate covered operations.

## Generate, then consume

Use Orval to generate TypeScript models, request functions, and TanStack Query hooks, using Axios as in Endorz. Preserve another compatible established transport when explicitly required. Keep each API contract's generated files in its own `generated/` subtree. Never manually edit them or duplicate their API DTO definitions. UI view models and form schemas are allowed when they represent distinct UI concerns; map explicitly to/from generated types.

Configure centralized authentication, base URL resolution, and error handling outside `generated/`. Keep Axios configuration small: same-origin credential behavior and the gateway CSRF cookie/header names. Use standard Axios response/error types. Do not add a custom request wrapper, error envelope/parser framework, or mutator when normal generated Axios handling suffices. Preserve request method, body, headers, serialization, credentials policy, and AbortSignal. Do not concatenate undocumented paths in the transport.

Axios rejects unsuccessful HTTP statuses by default; preserve the configured behavior. If an existing application uses Fetch, remember it does not reject HTTP error statuses automatically. Ensure the generated/client transport's documented behavior produces rejected query/mutation promises for failures. Preserve status, safe error details, and documented validation/Problem Details information; handle non-JSON failures and bodyless responses without unconditional JSON parsing. Account for response envelopes or status-discriminated unions actually emitted by that generator version instead of assuming `data` is always an unwrapped success DTO.

Use the established session/token flow. Do not add persistent token storage or change credential modes casually. Never send credentials to arbitrary origins. Cookie sessions require the application's CSRF protections for state-changing requests. Bound token refresh and retries; avoid refresh loops and duplicate writes. Retry mutations only when the operation's idempotency contract makes that safe. Cancellation should not appear as a user-facing server failure.

## Server state and tenancy

Consume actual generated names and query-key helpers. Configure staleness, retries, error presentation, and cache invalidation to match business needs. Distinguish initial loading from background refresh, and preserve valid displayed data where appropriate. Invalidate affected query families after successful mutations; use optimistic changes only with a defined rollback and reconciliation strategy.

Scope cache identity to the authenticated tenant/user context when those contexts can return different data for the same operation. If generated keys omit context, use a consistent supported key extension or an isolated QueryClient per context. Clear/cancel relevant cached and in-flight work on logout or tenant/account changes, and prevent late results from repopulating the wrong session. Do not display retained data from a prior tenant. The backend must independently enforce tenant isolation and permissions.

## Manual generation and verification

Developers run the repository's `npm run generate:api` against the latest running gateway before deployment, then commit the generated models/functions/hooks. Pin Orval in the lockfile. Use the actual configured schema environment variable (for example `ADMINISTRATION_GATEWAY_API_SCHEMA_URL` or `GATEWAY_API_SCHEMA_URL`), not an invented endpoint. If the gateway contract is unavailable, report that blocker rather than handwrite its client.

Review changes to operations, required/null fields, enums, statuses and serialized shapes. Format with the repository's configured generator formatter, then type check, test and build the consumers. Never patch generated code manually.

CI runs checks against committed generated output. Do not regenerate API clients automatically in CI or add checksum/provenance/drift scripts. This deliberate manual workflow comes from the Endorz repository instructions and replaces the earlier automatic-generation gate.

Gateway login, logout and invitation sign-in endpoints are browser navigation/form targets. Generate their URL helpers while disabling unused Query/mutation hooks when the configured Orval version supports it. Use full document navigation or native CSRF-protected POST for those endpoints; React Router and background fetch must not swallow OIDC redirects. See [sessions and access](sessions-and-access.md).

## Official documentation

- [ASP.NET Core OpenAPI generation](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/openapi/aspnetcore-openapi?view=aspnetcore-10.0)
- [Orval quick start](https://orval.dev/docs/quick-start/), [TanStack Query generation](https://orval.dev/docs/guides/react-query/), [Fetch client](https://orval.dev/docs/guides/fetch-client/), [custom HTTP client](https://orval.dev/docs/guides/custom-client/)
- [TanStack Query keys](https://tanstack.com/query/latest/docs/framework/react/guides/query-keys)
