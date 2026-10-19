# Gateway sessions, access and cache isolation

## Browser boundary

For Endorz, the gateway owns Kinde/OIDC, OAuth tokens and protected persisted sessions. React receives only the safe session response through a Secure HttpOnly cookie; never add a browser identity SDK, client secret, token refresh flow, or token storage in local/session storage. The readable CSRF cookie proves a mutation request, not authentication. Use the matching cookie/header names for the application; keep administration and customer sessions isolated.

Browser API calls use same-origin `/api`. Keep Axios credential policy aligned with the gateway. Endorz's `withCredentials: false` disables cross-origin credentials while normal same-origin cookies still work. Do not enable credentials for arbitrary origins. Unsafe requests use the established CSRF proof; native logout POST uses the gateway's expected verification field.

Gateway sign-in, invitation sign-in and sign-out are document navigation/form endpoints. Use generated URL helpers with full browser navigation or native POST so the browser follows provider redirects. Do not let React Router intercept `/api` authentication links or replace the navigation with background fetch. Ordinary application links retain client-side routing.

## Protected shell and termination

Confirm the session before exposing protected content. Visitors without a session follow gateway sign-in directly; failed callbacks show the established retry state rather than a new sign-in landing page. Do not treat an incomplete/cached session check as confirmed denial.

On sign-out, expiry or authenticated 401, hide protected content immediately, cancel in-flight queries and discard scoped data so late responses cannot repopulate the previous session. Replace/clear the business QueryClient when the confirmed user or organisation changes. Cross-tab notifications carry only a session-ended signal, never credentials or profile records.

Keep session-level denial separate from operation failures. A confirmed unusable/denied session follows the existing CSRF-protected sign-out/re-authentication policy. A business-operation 403 or dependency outage must not trigger global logout. Temporary workspace/session failures retain a safe shared error message and manual retry. Respect gateway expiry and existing focus/visibility rechecks; do not invent another idle policy or silently extend a server session.

## Workspace scope and recovery

Use server-verified Account/Company access. The absence of an Account permission is not proof of Company membership. Resolve the current workspace through the owning gateway/API and route to a verified scope; UI hiding never replaces server policies.

Where the established product flow automatically recovers from the wrong identity-provider organisation, bound the attempt through trusted server state and revalidate the requested organisation on callback. Do not create client redirect loops or automatic dependency retry loops. Do not grant membership or permissions during routing. Keep recipient-mismatch recovery tied to the invitation and verified identity; preserve its credential for an explicit retry without exposing it in diagnostics.

Role names, polling intervals, fallback routes and organisation mappings are product decisions to inspect. Preserve the active application's policy rather than copying Endorz constants into unrelated projects.
