# Dev Spaces multi-cluster redirector

One URL in front of two CheClusters. A GTM/load-balancer pool targets the
redirector Route on **primary** and **DR**. Each replica authenticates the user
(OpenShift OAuth) and 302s them to their home Dev Spaces dashboard. Factory URL
fragments stay in the browser.

```
developer → https://devspaces.<org-tld>   (GTM / LB)
                 │
                 ├─ redirector on primary
                 └─ redirector on DR
                        │
                        └─ 302 → https://devspaces.apps.<home-cluster>/…
```

`thisSite` is inferred from the cluster DNS base domain (`match` in
`config.json`). Do not add a per-cluster overlay file.

## Mapping

Edit the shared `config.json` (same file on both clusters):

- `users` — OpenShift username → `primary` or `dr` (wins first)
- `groups` — OpenShift Group name → site (first match in file order)
- `defaultSite` — everyone else (`primary`)

## Load balancer

Create a pool whose members are the two redirector Route hosts (primary and DR).

- Health check: `GET /healthz` (no auth)
- Point Hub `app.devspacesBaseUrl` at the GTM hostname once DNS exists
- Both clusters need the same identity provider so usernames match after OAuth

## Install notes

- **Primary:** GitOps Application `devspaces-redirector` (day-1 path)
- **DR:** apply with the documented deploy script until GitOps is bootstrapped on that cluster

Implementation lives under `components/devspaces-redirector` in the day-1 components repo.
