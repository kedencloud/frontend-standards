# Tests that establish behavior

## Frontend unit and component tests

Use Vitest for unit tests, React Testing Library for component tests, and MSW for HTTP mocking. Exercise user-visible behavior with accessible role/label queries and realistic `userEvent` interactions; avoid assertions against component state, hook internals, CSS classes, or private methods unless those are the explicit public contract under test. Test business rules and validation independently where useful.

For data-driven components, exercise loading, error, success, and empty outcomes when applicable. Include meaningful validation and permission failures, retry/recovery, mutation pending behavior, and regression cases for the change. Assert resulting UI and consequential side effects rather than merely that a mock was called. Do not manufacture an empty state for a component with no such behavior.

Intercept the network boundary through MSW so tests exercise actual generated request/response handling. Use deterministic fixtures conforming to the real API contract; generated random mocks alone do not prove behavior. Fail unexpected HTTP requests, reset handlers after each test, and close the test server after the suite. Use a fresh QueryClient per test; control retries to make error cases deterministic. Avoid shared cache, shared mutable fixtures, real network dependencies, and arbitrary sleeps. Test the shared transport's HTTP error, cancellation, and bodyless-response behavior.

## Browser journeys and visual checks

Use Playwright for E2E. Cover critical business journeys and high-risk integrations; not every utility change requires a browser test. Prefer role and label locators, using stable test IDs only where semantics are insufficient. Use isolated, repeatable test data and independent browser contexts. Avoid order dependence, shared mutable accounts, arbitrary timeouts, production records, and live third-party dependencies in routine tests.

Run an identified smoke suite on pull requests against a built application in a reproducible environment. At least critical backend integration journeys must exercise the real non-production application boundary; browser routes that mock all APIs are useful UI checks but are not evidence of backend integration. Provision/clean data per test or worker and keep parallel tests independent. Include login/session expiry, permissions, and tenant-switch leakage checks where relevant.

Use Playwright screenshot comparisons for shared components and important states, including configured themes and responsive sizes. Keep browser/OS/fonts, viewport, locale/timezone, data, animation, and clock deterministic where they affect output. Review baseline changes as product changes; never blindly accept snapshots to make CI green. Use stories or an existing component harness when available. Pair visual checks with behavior and accessibility assertions.

## .NET boundary requirements

When the requested change includes backend implementation or contract behavior, use xUnit for unit/integration tests, WebApplicationFactory for HTTP integration tests, and Testcontainers with the production database engine. Keep unit and integration projects separate. Test business rules at unit level and cross-boundary behavior where integration is required, including tenant isolation, permissions, and relevant worker idempotency, retries, and failure recovery.

Honor established Kedencloud backend naming, BDD, fixtures, and architecture conventions where applicable. Never substitute an in-memory provider for proof of production database behavior. Routine tests must not call live external providers. A frontend-only task does not authorize unrelated backend test rewrites. If existing guidance limits routine integration tests to happy paths, add risk-specific isolation/permission/failure integration cases when unit tests cannot establish that boundary and explain why.

## Failure discipline

Never delete, skip, weaken assertions, disable suites, inflate retries, lower thresholds, or update visual baselines solely to pass CI. Correct obsolete tests only when intended behavior has actually changed, with an explanation. Use coverage to locate untested risk; do not add implementation-mirroring tests or impose arbitrary coverage targets absent a project requirement. Report environment-blocked checks as unexecuted, not passing.

## Official documentation

- [Vitest](https://vitest.dev/guide/), [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/), [user-event](https://testing-library.com/docs/user-event/intro/), [MSW](https://mswjs.io/docs/)
- [Playwright introduction](https://playwright.dev/docs/intro), [best practices](https://playwright.dev/docs/best-practices), [visual comparisons](https://playwright.dev/docs/test-snapshots)
- [xUnit](https://xunit.net/), [WebApplicationFactory integration tests](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests?view=aspnetcore-10.0), [Testcontainers for .NET](https://dotnet.testcontainers.org/)
