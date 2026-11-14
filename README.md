# Jupiter Broadcasting Hugo theme

Shared theme for [jupiterbroadcasting.com](https://github.com/goodwinlabs/jupiterbroadcasting.com) and
[archive.jupiterbroadcasting.com](https://github.com/goodwinlabs/archive.jupiterbroadcasting.com), consumed as a
[Hugo Module](https://gohugo.io/hugo-modules/). History is carried over from `themes/jb` in the main site
(`git subtree split -P themes/jb`).

> Proof of concept in the `goodwinlabs` forks. Upstream, replace `goodwinlabs` with `JupiterBroadcasting` in
> `go.mod`, the sites' `config.toml` imports and the workflows.

## Use it in a site

```toml
# config.toml
[module]
  [[module.imports]]
    path = "github.com/goodwinlabs/jb-hugo-theme"
```

```sh
hugo mod get github.com/goodwinlabs/jb-hugo-theme@develop
```

Requires Hugo >= 0.169.0 and Go (Hugo Modules use it to fetch). Defaults for every param the theme reads are in
[`hugo.toml`](hugo.toml); a site overrides only what differs, e.g. the archive sets
`params.support_url = "https://www.jupiterbroadcasting.com/membership/"`.

The theme also ships the emoji-tag content adapter (`content/tags/_content.gotmpl`) and its slug map
(`data/emoji_slugs.toml`), so both sites get working emoji tag pages.

## Branches and releases

| Branch | Purpose |
|---|---|
| `develop` (default) | All theme PRs merge here. Each merge opens a "bump theme" PR into both sites' `develop`. |
| `main` | What production runs. Merge `develop → main` and tag `vX.Y.Z` alongside a site release. |

## Local development

Clone the theme next to the site repos and run a site with `make dev-theme`:

```text
jb-hugo-theme/
jupiterbroadcasting.com/
archive.jupiterbroadcasting.com/
```

`make dev-theme` sets `HUGO_MODULE_REPLACEMENTS` to the sibling checkout, so saving a theme file live-reloads the
site.

## CI

- `ci.yml`: every PR builds **both** sites against the PR's theme (module replacement) and runs each site's
  non-episode e2e tests.
- `notify-sites.yml`: on push to `develop`, sends `repository_dispatch: theme-updated` to both sites. Needs a
  `SITE_DISPATCH_TOKEN` secret (fine-grained PAT or GitHub App token with `contents:write` on both site repos).
  Without it the sites still pick changes up on their hourly bump schedule.
