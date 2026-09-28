# plugin-builder-npm

The `npm` builder for OpenCharly — the Node.js/npm package builder as a plugin
`builder:` word.

A candy that ships a `package.json` is built by this builder: the host selects
it by **detection** (the candy's `package.json`), never by an authored
`external_builder:`. The provider serves the build-time multi-stage
(`FROM <builder> AS npm-build` + COPY artifacts) and the deploy-time teardown IR
(global-package uninstall, when the globals are known).

## What it provides

| Capability | Surface |
|---|---|
| `builder:npm` | the `npm` builder word — the npm multi-stage builder |

The builder authors no `plugin_input` (it is triggered by detection, not an
authored field); its self-contained `#NpmBuilderInput` (`schema/npm.cue`) is
empty by design and ships so the schema travels with the plugin.

## How to use it

Compose the plugin candy in a box's `candy:` list:

```yaml
- '@github.com/opencharly/plugin-builder-npm/candy/plugin-builder-npm:<tag>'
```

Then a candy with a `package.json` is built through this builder.

## Layout

- `candy/plugin-builder-npm/` — the plugin module: `plugin.go`,
  `schema/npm.cue`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-image:image` — box/builder configuration and the box
  dependency graph. This candy carries no `skill:` entity of its own; the gap is
  tracked in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-internals:plugin` — the plugin/provider model, including the
  `builder` provider class.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI.
