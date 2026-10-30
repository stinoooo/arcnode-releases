# arcnode-releases

Signed game releases for [Arcnode Games](https://arcnode.dev/downloads): the catalog,
the release manifests and the packages. The Arcnode Games launcher and each game's own
updater read everything from here.

Nothing in this repository is edited by hand. `arcnode-publish` (in
[`stinoooo/arcnode`](https://github.com/stinoooo/arcnode), see `publishing/`) signs
and writes every file, so every file a client trusts carries an Ed25519 signature.

## Current releases

| Product | Version | Channel | Released | Download |
| --- | --- | --- | --- | --- |
| Exit Only | 0.5.0 "The pursuit overhaul" | stable | 2026-10-30 | [Windows](https://github.com/stinoooo/arcnode-releases/releases/download/exit-only-0.5.0/exit-only-0.5.0-windows-x64.zip), [Linux](https://github.com/stinoooo/arcnode-releases/releases/download/exit-only-0.5.0/exit-only-0.5.0-linux-x64.tar.gz), [macOS](https://github.com/stinoooo/arcnode-releases/releases/download/exit-only-0.5.0/exit-only-0.5.0-macos-universal.zip) |
| Arcnode Games (launcher) | 0.1.0 | stable | 2026-10-29 | [Windows installer](https://arcnode.dev/api/download/launcher) |

The catalog is at sequence 6. Clients refuse a catalog with a lower sequence than the
highest one they have seen, so it only ever goes up.

### Exit Only history

| Version | Released | Title |
| --- | --- | --- |
| 0.5.0 | 2026-10-30 | The pursuit overhaul |
| 0.4.0 | 2026-10-29 | Settings and smarter police |
| 0.3.1 | 2026-10-29 | Linux and macOS |
| 0.3.0 | 2026-10-29 | A new look |
| 0.2.0 | 2026-10-29 | Most Wanted |

## Layout

Served by GitHub Pages at
[stinoooo.github.io/arcnode-releases](https://stinoooo.github.io/arcnode-releases/catalog.json):

```text
catalog.json(.sig)                      every game, its channels and artwork
games/<game>/releases.json(.sig)        the release history of one game
games/<game>/<version>/release.json(.sig)  the manifest: files, sizes, SHA-256, launch info
games/<game>/art/                       cover, banner, logo, icon and screenshots
launcher/latest.json                    the launcher's own update feed (minisign)
```

The packages themselves are GitHub Release assets:

- `<game>-<version>`: the full game package per platform;
- `files`: individual game files by SHA-256, so an update only downloads what changed;
- `launcher-<version>`: the Arcnode Games installer.

The catalog pins the SHA-256 of each manifest, and each manifest pins the size and
SHA-256 of every file, so a client verifies everything it downloads before using it.

## Line endings

`.gitattributes` turns off line-ending conversion (`* -text`). Signatures cover the
exact bytes, so a checkout that rewrote line endings would fail verification.
