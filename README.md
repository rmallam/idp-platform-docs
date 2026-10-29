# IDP platform docs

Working documentation for the Acme IDP lab (Developer Hub, Dev Spaces, mesh, GitOps operations notes).

## Scope

| Goes here | Stays in gitops (`rhdh-platform-gitops`) |
|---|---|
| Design notes, runbooks, Jira story drafts, how-tos | Chart/README that explain how to install that chart |
| Cross-cutting platform docs | Values/templates and sync-specific notes |
| Customer-share drafts (sanitized) under `share/` if needed | Operator/CR wiring that must live next to code |

**Rule:** put new docs in this repo unless they are specific to a GitOps chart or Application and must travel with that code.

## Layout

```
devspaces/     Dev Spaces topics (redirector, plugin-registry, connector, …)
jira/          Story drafts (summary + acceptance criteria)
share/         Optional sanitized share packs
```

## Related repos

- GitOps (day-2 CRs): `rmallam-rhdh/rhdh-platform-gitops`
- Day-1 components: Digi / `RHDH-SM-DevSpaces` clone
