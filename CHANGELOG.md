# Changelog (integration fork)

This file describes **this fork** (`mashkovd/mcp-for-argocd`, branch `integration`), not the
upstream project. The fork is upstream `main` plus the open pull requests listed below, so
that their changes can be used before they are reviewed. Each entry is dropped from the fork
as soon as it lands in [argoproj-labs/mcp-for-argocd](https://github.com/argoproj-labs/mcp-for-argocd).
All credit belongs to the original authors.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/). Versions are
`<next upstream patch>-fork.<n>` (a semver pre-release of the next upstream version, no `v`
prefix). Image: `ghcr.io/mashkovd/mcp-for-argocd:<version>`; pin the version tag or the digest,
there is no `latest`.

## [Unreleased]

## [0.9.1-fork.1]

First release under the `fork` naming. It carries the same code as `0.9.1-portal.2`; only the
release engineering changed (naming, changelog, no floating `latest` tag).

### Included from open pull requests in the upstream repository

| PR | Author | What it adds |
|---|---|---|
| [#124](https://github.com/argoproj-labs/mcp-for-argocd/pull/124) | @bbartels | Correct MCP safety annotations (read-only / destructive hints) for the Argo CD tools |
| [#125](https://github.com/argoproj-labs/mcp-for-argocd/pull/125) | @containeers | `ARGOCD_EXTRA_HEADERS` for Argo CD API requests |
| [#149](https://github.com/argoproj-labs/mcp-for-argocd/pull/149) | @anastasiia-flink | Opt-in JWT verification of inbound requests (identity-aware proxy support, ES256 too) |
| [#153](https://github.com/argoproj-labs/mcp-for-argocd/pull/153) | @dorian-tsorfy | `--json-response` flag to avoid SSE-only responses on the HTTP transport |
| [#155](https://github.com/argoproj-labs/mcp-for-argocd/pull/155) | @oladejotunde60 | `list_applications` search applied locally, compact summaries |
| [#159](https://github.com/argoproj-labs/mcp-for-argocd/pull/159) | @YamonBot | Preserve sync options and multi-source revisions |
| [#163](https://github.com/argoproj-labs/mcp-for-argocd/pull/163) | @javidaslan | Docker base image upgraded to `node:24-slim` |
| [#164](https://github.com/argoproj-labs/mcp-for-argocd/pull/164) | @hazel-RH | Optional TLS listener via `MCP_TLS_CERT_FILE` / `MCP_TLS_KEY_FILE` |
| [#165](https://github.com/argoproj-labs/mcp-for-argocd/pull/165) | @gopkri24 | `--port`, `--stateless`, `--allow-unauthenticated` can be set from the environment |
| [#168](https://github.com/argoproj-labs/mcp-for-argocd/pull/168) | @mashkovd | `get_application_sync_windows`, `get_appproject_sync_windows` (read-only) |

### Included from other forks (no upstream pull request)

| Source | Author | What it adds |
|---|---|---|
| [lewis-0924/mcp-for-argocd](https://github.com/lewis-0924/mcp-for-argocd) | liam | `set_helm_parameters`: change one Helm parameter without dropping the others |
| [mamrezb/mcp-for-argocd](https://github.com/mamrezb/mcp-for-argocd) | Mamrez Behfar | `fix(http): bound Argo CD backend requests` |

### Changed in this fork (on top of the above)

- `set_helm_parameters` is annotated as a write tool, so it is not registered with
  `MCP_READ_ONLY=true`; the `V1alpha1HelmParameter` type is exported.
- Inbound JWT verification goes through the listener security middleware and fails closed
  (follow-up to #149).
- `ARGOCD_EXTRA_HEADERS` are sent only to the configured Argo CD base URL, never to other
  hosts (follow-up to #125).
- The Argo CD API token is trimmed of surrounding whitespace, including where the
  `Authorization` header is built (a trailing newline in a mounted secret breaks auth).
- Test pinning that `MCP_READ_ONLY=true` registers only read-only tools; auth tests no longer
  use top-level `await`, so `tsc --noEmit` is clean.
- CI: `ghcr.yml` publishes the image to GHCR on version tags (no `latest`).

## Earlier tags

`0.9.1-portal.1` and `0.9.1-portal.2` are earlier builds of the same branch under the former
`-portal` naming (the branch was then called `integration/portal`). They remain available and
are not rewritten.

[Unreleased]: https://github.com/mashkovd/mcp-for-argocd/compare/0.9.1-fork.1...integration
[0.9.1-fork.1]: https://github.com/mashkovd/mcp-for-argocd/releases/tag/0.9.1-fork.1
