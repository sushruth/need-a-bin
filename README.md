# need-a-bin

Some useful tools don't publish ready-to-run binaries on GitHub. This repo builds the ones I need.

Each tool sits here as a git submodule that points at its upstream repo. I add nothing to the source. GitHub Actions builds it as-is, for the platforms I use, and publishes the binaries as release assets.

## Tools

### Valkey

`valkey-server` with TLS off and no extra libraries needed.

1. Releases are tagged `valkey-<version>`, for example `valkey-8.1.11`.
2. Assets are named `valkey-server-<os>-<arch>.tar.gz`, for `darwin-arm64`, `darwin-x64`, `linux-x64` and `linux-arm64`. Each archive holds only `valkey-server`.
3. A daily workflow checks Valkey's latest release. If there's no matching release here yet, it moves the submodule to that tag and builds it.

### MySQL

`mysqld` and the `mysql` client, ready to run without installing anything else.

1. These aren't my builds. They're Oracle's MySQL binaries, pruned down to what a local dev server needs. Linux archives also carry four libraries in `lib/private`, taken from Ubuntu 22.04 packages: `libaio.so.1`, `libncurses.so.5`, `libtinfo.so.5` and `libnuma.so.1`. Oracle's binaries need them, and current distros like Ubuntu 24.04 either ship renamed versions or none at all.
2. Releases are tagged `mysql-<version>`, for example `mysql-8.0.45`.
3. Assets are named `mysql-<os>-<arch>.tar.gz`, for `darwin-arm64`, `darwin-x64`, `linux-x64` and `linux-arm64`. Each archive holds `bin/`, `lib/` and `share/` at its root.
4. There's no submodule, since Oracle's binaries are the upstream. The version lives in `mysql/VERSION`, and I bump it by hand.

### MySQL from source

`mysqld` and `mysql` built from the `mysql-server` submodule by `mysql-source.yml`, with OpenSSL, libaio, ncurses and libstdc++ linked in statically.

1. Releases are prereleases tagged `mysql-src-<version>`, for example `mysql-src-8.0.45`.
2. Assets use the same names and root layout as the MySQL releases above.
3. A daily workflow checks for the newest upstream `mysql-8.0.*` tag. If there's no matching `mysql-src-<version>` release yet, it moves the submodule to that tag and builds it.

### Building another upstream tag

`valkey.yml` and `mysql-source.yml` take an optional `tag` input when run by hand. With a tag such as `8.1.12` or `mysql-8.0.44`, the workflow checks out that upstream tag, builds it and publishes it as `valkey-8.1.12` or `mysql-src-8.0.44`. It doesn't move the submodule. Without a tag, it builds the pinned submodule.

## Add a tool

1. Add its upstream repo as a submodule.
2. Add a workflow that builds it without changes and publishes a release per upstream version.
3. Add a section here.
