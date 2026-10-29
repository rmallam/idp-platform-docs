# Jira: Dev Spaces multi-cluster redirector installation

## Summary

Install and configure the Dev Spaces multi-cluster redirector so developers hit a single front-door URL, authenticate once, and are redirected to their home-cluster Dev Spaces dashboard (primary or DR) based on user/group mapping.

## Description

Deploy the redirector on **primary** and **DR** OpenShift clusters. A GTM/LB pool fronts both redirector Routes. Each replica uses OpenShift OAuth, reads shared `config.json` mapping (`users` / `groups` / `defaultSite`), and returns HTTP 302 to the user’s home CheCluster dashboard. Factory/workspace URL fragments are preserved. Primary is GitOps-managed; DR may use the deploy script until GitOps exists there.

## Acceptance criteria

1. Redirector Deployment, Service, Route, SA/OAuth, TLS, and config are running in `devspaces-redirector` on **both** primary and DR.
2. `GET /healthz` on each redirector Route returns **200** without auth (LB health checks pass).
3. Unauthenticated access to the redirector triggers OpenShift OAuth; after login, user receives **302** to their home Dev Spaces dashboard.
4. Shared mapping works on both sites: `users` override, then `groups`, then `defaultSite`; `thisSite` is inferred from cluster DNS (`match` in config) — no per-cluster overlay file.
5. Seed/lab mapping verified (mapped users land on primary vs DR as configured).
6. GTM/LB pool members are both redirector Route hosts; front-door hostname resolves and load-balances.
7. Both clusters use the **same IdP** so usernames match after OAuth.
8. Hub/platform `devspacesBaseUrl` (or equivalent) can point at the front-door hostname once DNS exists.
9. Primary syncs via GitOps (`devspaces-redirector`); DR installed via documented deploy path (or GitOps if available).
10. Docs/runbook cover config edit, LB members, health check, and how to add/move a user between sites.

## References

- Design/runbook: [../devspaces/multi-cluster-redirector.md](../devspaces/multi-cluster-redirector.md)
