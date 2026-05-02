# Suji App Templates

Public catalog of one-click app manifests for the [Suji](https://suji.fr) platform.

The Suji control plane syncs this repo every 6 hours (and immediately on push, via webhook) and exposes its contents as the in-product app marketplace.

## Layout

```
suji/<slug>/
  manifest.yaml      # required — see schema below
  icon.svg           # optional — referenced from manifest's icon_url

community/<slug>/
  manifest.yaml
  icon.svg
```

- `suji/` — first-party apps maintained by the Suji team.
- `community/` — community-contributed apps. PRs welcome.

## Manifest schema (summary)

Each `manifest.yaml` declares:

- **Identity** — `slug`, `tier` (`suji` | `community`), `name`, `description`, `version` (semver), `categories`, `license`, `homepage_url`, `icon_url`, `release_notes`.
- **Resources** — `min_memory_mib`, `recommended_memory_mib`, `min_cpu_millicores`, `recommended_storage_gib`.
- **Exposure** — whether the app is reachable from outside the tenant VM (`exposable`, `default`, `port`, `subdomain_hint`).
- **Network** — `internal_hostname` used by sibling apps inside the tenant VM.
- **Upgrade policy** — `automatic` | `manual` | `blocked`.
- **Registry allowlist** — only images from these registries can be pulled (`docker.io`, `ghcr.io`).
- **Form schema** — fields shown to the user at install time. Each field has a `key` (matches the env var in `compose`), a `type` (`text` / `secret` / `number` / `select` / `multiselect`), label/description, and (optionally) `required`, `default`, `auto_generate`, `options`.
- **Compose** — a docker-compose YAML where `${VAR}` tokens are substituted from the user's form input at install time, then re-validated.

The full Zod schema lives at [packages/shared/src/manifest/zod-schema.ts](https://github.com/Arizona-dev/SujiSaaS) in the (private) Suji monorepo.

## Contributing

1. Fork this repo.
2. Add a directory under `community/<slug>/` with `manifest.yaml` (and optional `icon.svg`).
3. Open a PR. The Suji team reviews for safety (image origin, exposed ports, secrets handling) before merging.

## License

Each app manifest declares its own `license` field. The repo itself is under MIT.
