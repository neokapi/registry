# Neokapi Plugin Registry

This repo serves the plugin registry index that drives
`kapi plugin install`, `kapi plugin search`, and the auto-install
prompt triggered by `requires:` in `.kapi` recipes.

## Files

| File | Purpose |
|---|---|
| `plugins.json` | Legacy v1 index — Okapi bridge gRPC plugins. Consumed by `kapi plugins` (plural). |
| `manifest-plugins.json` | New v2 index — manifest-driven plugins (issue [neokapi/neokapi#438](https://github.com/neokapi/neokapi/issues/438)). Consumed by `kapi plugin` (singular). |
| `channels/snapshot.json` | Mirror of the snapshot channel for `--channel snapshot`. |

## Schema (v2)

```jsonc
{
  "schema": "v2",
  "plugins": {
    "<plugin-name>": {
      "description": "...",
      "homepage": "https://...",
      "author": "...",
      "license": "Apache-2.0",
      "group": "...",
      "channels": ["stable", "beta"],
      "versions": {
        "1.0.0": {
          "released": "2026-04-15",
          "channel": "stable",
          "min_kapi_version": "0.1.0",
          "platforms": {
            "darwin/arm64": {
              "url": "https://github.com/.../v1.0.0/kapi-<plugin>_1.0.0_darwin_arm64.tar.gz",
              "sha256": "...",
              "signature": "https://.../...sig",
              "cert_identity": "https://github.com/.../release.yml@refs/tags/v1.0.0",
              "cert_oidc_issuer": "https://token.actions.githubusercontent.com"
            }
          }
        }
      }
    }
  }
}
```

## Updates

This repo is updated by the neokapi release workflow on each tagged
release of a plugin. See [`.github/workflows/release.yml`](https://github.com/neokapi/neokapi/blob/main/.github/workflows/release.yml).

For manual updates, edit the JSON file directly and commit. Files are
served via GitHub Pages at `https://neokapi.github.io/registry/`.
