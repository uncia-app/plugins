# Uncia plugins

The default plugin catalog of [Uncia](https://github.com/uncia-app). The app reads
[`catalog.json`](catalog.json) from this repository and lists its plugins in
Settings › 플러그인 › 둘러보기.

## Catalog format

```jsonc
{
  "version": 1,
  "plugins": [
    {
      "id": "example.github-pr",            // reverse-DNS, not uncia.* (reserved for built-ins)
      "name": "GitHub PR",
      "publisher": "example",
      "version": "1.2.0",
      "description": "Pull requests for the open thread's branch.",
      "icon": "arrow.triangle.pull",         // an SF Symbol
      "url": "https://github.com/uncia-app/plugins/releases/download/example.github-pr-v1.2.0/example.github-pr-1.2.0.unciaplugin",
      "sha256": "…",                          // of the .unciaplugin file
      "permissions": ["thread.read", "workspace.vcs.read", "network:https://api.github.com"],
      "runtime": null,
      "engines": { "uncia": "^1.0" }
    }
  ]
}
```

Before installing, the app checks the downloaded package against its entry: the `sha256`, and the
manifest's `id`, `version`, `permissions` and `runtime` must all match. The user is shown the
permissions and asked to allow them; an update that asks for more asks again.

## Publishing a plugin

1. Pack the plugin folder (with `manifest.json` at its root) using
   [`scripts/pack-plugin.sh`](https://github.com/uncia-app) from the Uncia repository:

   ```sh
   scripts/pack-plugin.sh path/to/my-plugin out/
   ```

   It writes `out/<id>-<version>.unciaplugin` and prints the catalog entry, with `url` pointing
   at this repository's release `<id>-v<version>`.
2. Open a pull request that adds (or bumps) the entry in `catalog.json` and attaches the package
   to the pull request description. A maintainer reviews the plugin and its permissions, creates
   the release `<id>-v<version>` with the package as an asset, and merges.

Release assets are never replaced: a new version is a new release, so a `url` and its `sha256`
stay valid forever.

## Checks

CI validates `catalog.json` on every pull request: the format, unique ids, HTTPS URLs and
64-character hashes.
