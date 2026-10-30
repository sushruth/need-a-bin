# need-a-bin

This repo builds standalone `valkey-server` binaries on GitHub Actions and publishes them as release assets, so local dev scripts can download them instead of compiling.

Each release is tagged `valkey-<version>`, for example `valkey-8.1.11`. It has one asset per platform, named `valkey-server-<os>-<arch>.tar.gz`, where the platform is one of `darwin-arm64`, `darwin-x64`, `linux-x64` or `linux-arm64`. Each archive holds only the `valkey-server` binary at its root.

A daily workflow checks Valkey's latest release. When it has no matching release here, the workflow moves the `valkey` submodule to that tag, pushes, and starts the build.

To bump Valkey by hand, check out a new tag in the `valkey` submodule, commit the submodule change, and push to `main`. The workflow builds the new version and publishes its release.
