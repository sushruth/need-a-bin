# need-a-bin

Some useful tools don't publish ready-to-run binaries on GitHub. This repo builds the ones we need.

Each tool sits here as a git submodule that points at its upstream repo. We add nothing to the source. GitHub Actions builds it as-is, for the platforms we use, and publishes the binaries as release assets.

## Tools

### Valkey

`valkey-server` with TLS off and no extra libraries needed.

1. Releases are tagged `valkey-<version>`, for example `valkey-8.1.11`.
2. Assets are named `valkey-server-<os>-<arch>.tar.gz`, for `darwin-arm64`, `darwin-x64`, `linux-x64` and `linux-arm64`. Each archive holds only `valkey-server`.
3. A daily workflow checks Valkey's latest release. If there's no matching release here yet, it moves the submodule to that tag and builds it.

## Add a tool

1. Add its upstream repo as a submodule.
2. Add a workflow that builds it without changes and publishes a release per upstream version.
3. Add a section here.
