# layer-language-runtimes

The polyglot system-runtime meta-layer — Go, PHP, .NET 9 SDK, Node.js headers,
and system Python 3 — as a standalone OpenCharly layer repo.

The candy installs the .NET 9 SDK (distro package on Fedora/Arch, Microsoft's
`dotnet-install.sh` on Debian/Ubuntu), the PHP CLI, and the system Python 3
interpreter — each landing at a `/usr/bin` path. Every runtime exposes a version
subcommand, so its presence is directly verifiable from a fresh build with no
service or deploy state. Go and Node.js are installed too, but their acceptance
is owned by the `golang` and `nodejs` candies (not duplicated here).

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `language-runtimes` |
| Runtimes | .NET 9 SDK (`/usr/bin/dotnet`), PHP CLI (`/usr/bin/php`), system Python 3 (`/usr/bin/python3`) |
| Dependencies | `layer-nodejs`, `layer-rust` |
| Service / port | none |

The system Python here is the RPM/DEB interpreter, **not** a pixi env — the
candy declares no `python`/`pixi` dependency, so consumers get only the system
Python stack.

## How to use it

Compose the layer as a nested `candy:` list inside a named box body:

```yaml
my-polyglot:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-language-runtimes:v2026.243.0409'
```

## Layout

- `charly.yml` — the `language-runtimes:` candy entity (the per-distro package
  sections, the `dotnet-install.sh` cross-distro plan step, the `check:`
  assertions, and the embedded `language-runtimes-skill:` skill entity).
- `CHANGELOG/` — per-CalVer release history.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:language-runtimes` — the runtime meta-layer, the
  system-Python-vs-pixi distinction, and the `dotnet-install.sh` parity story.
- `/charly-coder:nodejs`, `/charly-coder:rust` — direct dependencies.
- `/charly-image:layer` — candy authoring reference.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
