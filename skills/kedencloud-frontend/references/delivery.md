# Delivery and CI gates

Discover real commands from package scripts, workspace configuration, and CI; do not report illustrative command names as executed checks. Keep the runtime/package manager pinned and use frozen-lockfile installation. Use TypeScript checking separately when the bundler only transpiles. Keep ESLint (including Hooks rules and appropriate typed rules), formatting, tests, and production build reproducible. Introduce stricter options incrementally in existing projects rather than weakening them or causing unrelated migrations.

For frontend pull requests, the applicable pipeline gates are:

1. Review the manually regenerated and committed gateway client when the contract changed. Do not add automatic generation, checksums or drift scripts to CI.
2. Run lint, formatting checks, type checking, Vitest unit/component tests and the production build using actual package scripts.
3. Run relevant Playwright desktop/mobile behavior, accessibility and screenshot checks for changed shared structure or critical journeys in the established test environment. Do not automatically replace baselines.
4. Run affected backend suites only when backend implementation/contracts are in scope. Review API compatibility with the contract diff.

Independent jobs may run in parallel after their required inputs exist. Required CI failures block completion; a scheduled suite does not silently replace a required pull-request check. Set up branch protection/rulesets only when authorized; do not claim that merely adding a workflow makes its checks mandatory.

For GitHub Actions, minimize job permissions, pin third-party actions to reviewed commit SHAs, keep secrets out of untrusted PR code, and upload useful failure diagnostics with appropriate retention. Avoid `pull_request_target` execution of untrusted changes. Use synthetic data and redact tokens and personal records from screenshots, traces, logs, and artifacts.

Before handing back a feature, check relevant loading/error/empty/success states, keyboard use, narrow/wide viewports, configured themes, permission boundaries, and changed shared-component stories. Report:

- The resulting behavior and why it changed.
- Tests/checks actually executed and their outcomes.
- Failed or unexecuted required checks, with concrete causes.
- Material risks, contract dependencies, or approved deviations.

Skill text does not itself enforce these gates. A consuming application must configure commands, workflows, tests, and required checks for its actual environment. Do not claim that a standards-only repository ran an application's build or E2E suite.

## Static runtime and deployment boundaries

Build with the pinned Node/package manager and lockfile, then copy only production assets into the standard unprivileged Nginx runtime. Keep test harnesses/artifacts out of production routing. Configure same-origin `/api` proxying, Cookie/Set-Cookie and correct host/protocol forwarding, while preserving the gateway's security assumptions. SPA fallback applies to application routes, not failed API requests. Do not add a custom Node server/proxy.

Use trusted local HTTPS when verifying Secure cookies. Local OrbStack/Kubernetes manifests are development conveniences and do not establish hosted deployment readiness. Browser code uses its own origin's `/api`; internal gateway addresses and secrets stay in server/deployment settings. A mocked HTTP browser test does not prove real secure-cookie or hosted SSO behavior.
