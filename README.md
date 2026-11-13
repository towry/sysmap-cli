# sysmap-cli

Install the latest stable release:

```sh
curl -fsSL https://github.com/towry/sysmap-cli/releases/latest/download/install.sh | bash
```

The default download selects the latest published stable version, not a snapshot
or an opt-in prerelease. Stable releases have immutable `vMAJOR.MINOR.PATCH`
tags and an exact matching CLI version. `release-manifest.json` records the
private source commit and public workflow commit; `checksums.txt` covers all six
payload files, including the installer and manifest.

Maintainers: run the manual `release` workflow with a full 40-hex `source_sha`
already merged into source `main` and a new stable `release_tag`. It creates a
local source tag only on the build runner, builds without snapshot mode, verifies
the binary version, and publishes a new stable release as GitHub's latest. Never
overwrite a published version. A failed partial release must be inspected before
another publication attempt.

## Isolated prereleases

Prereleases are opt-in and do not replace `latest`. After an approved release
exists, replace the example tag below with its exact tag:

```sh
tag=v0.3.0-ul.1
curl -fsSL "https://github.com/towry/sysmap-cli/releases/download/$tag/install.sh" |
  SYSMAP_VERSION="$tag" SYSMAP_BIN_DIR="$HOME/.local/sysmap-prerelease/$tag/bin" bash
"$HOME/.local/sysmap-prerelease/$tag/bin/sysmap" --version
```

Use that explicit binary path; do not change `PATH` or overwrite the stable
installation. `checksums.txt` covers all six payload files, including the
installer and `release-manifest.json` (build identity, tool versions, and target
format/schema).

Maintainers: the manual `prerelease` workflow accepts only a full 40-hex
`source_sha` already merged into source `main` and a new `v0.3.0-ul.N` tag
(positive N). Source CI and explicit merge/publication approval are required
before dispatch. The public tag targets the public workflow commit; the manifest
records the source commit. Never rerun a partially published tag: inspect its
state and use a new N. Stable publishing remains a separate workflow.
