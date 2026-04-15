# pi-vault-tec

`pi-vault-tec` is a pi package that brings a restrained Vault-Tec terminal treatment to pi. It combines four elements:

- a phosphor-green `vault-tec` theme;
- a compact Vault-Tec prompt layer injected through a pi extension;
- a text-only `PI-BOY 3000` header treatment in place of the earlier graphic crest;
- a telemetry widget below the editor that carries the model, thinking level, workspace, context, input/output, and cache data in the `PI-BOY 3000` style.

The package is inspired by [`kommander/oc-plugin-vault-tec`](https://github.com/kommander/oc-plugin-vault-tec), but it is implemented directly against pi's extension and theme APIs rather than as a line-for-line port.

## Installation

Install directly from GitHub:

```bash
pi install https://github.com/lulucatdev/pi-vault-tec
```

For local development from this working copy:

```bash
pi install /Users/lucas/Developer/pi-vault-tec
```

For a one-off run without installation:

```bash
pi -e /Users/lucas/Developer/pi-vault-tec/extensions/vault-tec/index.ts \
  --theme /Users/lucas/Developer/pi-vault-tec/themes/vault-tec.json
```

## What it enables

After installation, the package can automatically:

- switch pi to the bundled `vault-tec` theme;
- append a light Vault-Tec terminal persona to the system prompt;
- replace the default header with a text-only `PI-BOY 3000` banner;
- show a telemetry panel below the editor with model, thinking level, workspace, context, input/output, and cache data;
- suppress the default lower-footer clutter with a custom footer override.

All of these features are individually toggleable.

## Command surface

The package registers `/vault-tec`.

### Interactive control panel

```text
/vault-tec
```

This opens a toggle list for the current session.

### Direct toggles

```text
/vault-tec on
/vault-tec off
/vault-tec prompt off
/vault-tec telemetry off
/vault-tec theme on
/vault-tec header off
/vault-tec status
/vault-tec reset
```

`reset` restores the current session to the merged disk-backed configuration.

### Persist settings

```text
/vault-tec save global
/vault-tec save project
```

Persistence is stored in these files:

- global: `~/.pi/agent/vault-tec.json`
- project: `.pi/vault-tec.json`

Project settings override global settings.

## Feature mapping from the upstream plugin

The upstream OpenCode plugin has two major halves: prompt transformation and TUI slot customization. The pi port maps those capabilities as follows.

| Upstream idea | pi implementation |
|---|---|
| server prompt injection | `before_agent_start` system-prompt append |
| bundled theme | `themes/vault-tec.json` |
| settings dialog | `/vault-tec` interactive settings list |
| sidebar Pip-Boy panel | below-editor `PI-BOY 3000` telemetry widget |
| terminal header | custom `setHeader()` with a text-only `PI-BOY 3000` banner |
| lower console chrome | custom `setFooter()` used to suppress default footer clutter |

## Intentional differences

This package does not attempt a pixel-identical reproduction of the OpenCode plugin.

- pi does not expose the same home-screen slot and post-processing APIs, therefore true scanlines, vignette, and sidebar slot injection are not replicated.
- The prompt layer is append-only. It does not replace pi's base system prompt because pi's tool contracts and runtime constraints must remain intact.
- The persona is intentionally lighter than the upstream prompt so that normal coding work remains readable.

## Files

```text
pi-vault-tec/
├── extensions/vault-tec/index.ts
├── extensions/vault-tec/prompt.txt
├── themes/vault-tec.json
├── package.json
└── README.md
```

## Development notes

Useful local checks:

```bash
npm run pack:check
npm run typecheck
```

`typecheck` assumes the pi peer dependencies are available in the development environment.

## License

MIT. See `LICENSE`.
