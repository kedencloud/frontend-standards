# Delivery and CI gates

Discover real commands from package scripts, workspace configuration, and CI; do not report illustrative command names as executed checks. Keep the runtime/package manager pinned and use frozen-lockfile installation. Use TypeScript checking separately when the bundler only transpiles. Keep ESLint (including Hooks rules and appropriate typed rules), formatting, tests, and production build reproducible. Introduce stricter options incrementally in existing projects rather than weakening them or causing unrelated migrations.

For frontend pull requests, the applicable pipeline gates are:

1. Obtain verified pinned OpenAPI contracts; regenerate backend specifications too when owned in the same change/repository.
2. Generate clients with pinned Orval tooling; reject tracked or untracked generated-output drift. Type check downstream consumers after generation.
3. Run lint, formatting checks, Vitest unit/component tests, and the production build.
4. Run the Playwright smoke suite against the built application and appropriate test services. Run shared-component visual checks with a stable rendering environment. Broader browser matrices can run separately according to repository policy.
5. Run affected backend unit/integration suites when backend contracts or implementation change. Review API breaking changes with the repository's compatibility tool or explicit contract diff review.

Independent jobs may run in parallel after their required inputs exist. Required CI failures block completion; a scheduled suite does not silently replace a required pull-request check. Set up branch protection/rulesets only when authorized; do not claim that merely adding a workflow makes its checks mandatory.

For GitHub Actions, minimize job permissions, pin third-party actions to reviewed commit SHAs, keep secrets out of untrusted PR code, and upload useful failure diagnostics with appropriate retention. Avoid `pull_request_target` execution of untrusted changes. Use synthetic data and redact tokens and personal records from screenshots, traces, logs, and artifacts.

Before handing back a feature, check relevant loading/error/empty/success states, keyboard use, narrow/wide viewports, configured themes, permission boundaries, and changed shared-component stories. Report:

- The resulting behavior and why it changed.
- Tests/checks actually executed and their outcomes.
- Failed or unexecuted required checks, with concrete causes.
- Material risks, contract dependencies, or approved deviations.

Skill text does not itself enforce these gates. A consuming application must configure commands, workflows, tests, and required checks for its actual environment. Do not claim that a standards-only repository ran an application's build or E2E suite.
