# Kamorion ARM64 builds

This is a Kamorion fork of [`redirectionio/libnginx-mod-redirectionio`](https://github.com/redirectionio/libnginx-mod-redirectionio) that adds ARM64 (aarch64) Debian packages, since upstream only publishes amd64.

## Supported distributions

| Distro | Debian | nginx |
|---|---|---|
| bullseye | 11 | 1.18.0 |
| bookworm | 12 | 1.22.1 |
| trixie | 13 | 1.26.2 |

## Releases

ARM64 `.deb` packages are published as GitHub Release assets. File naming:

```
libnginx-mod-http-redirectionio_<upstream>-kamorion<N>-1~<distro>_arm64.deb
```

Example: `libnginx-mod-http-redirectionio_3.1.0-kamorion1-1~bookworm_arm64.deb`

## Building locally

The [build workflow](.github/workflows/build-arm64.yml) is the source of truth. To run manually:

1. GitHub Actions tab → "Build ARM64 Debian packages" → "Run workflow"
2. Inputs:
   - `module_version` — e.g. `3.1.0-kamorion1`
   - `libredirectionio_ref` — usually `master` or an upstream libredirectionio tag
   - `create_release` — true to publish as a GitHub Release

## Consuming in a Dockerfile

Phase 1 (current) — direct download from GitHub Releases:

```dockerfile
ARG MODULE_VERSION=3.1.0-kamorion1
ARG DISTRO=bookworm
ADD https://github.com/KamorionLabs/libnginx-mod-redirectionio/releases/download/v${MODULE_VERSION}/libnginx-mod-http-redirectionio_${MODULE_VERSION}-1~${DISTRO}_arm64.deb /tmp/module.deb
RUN dpkg -i /tmp/module.deb && rm /tmp/module.deb
```

Phase 2 (future) — apt repo on GitHub Pages.

## Upstream sync

Keep the fork rebased on upstream `master` periodically. Only files Kamorion owns:

- `.github/workflows/build-arm64.yml`
- `README.kamorion.md`

Upstream workflows `.github/workflows/beta-release.yml` and `stable-release.yml` are removed in this fork (they trigger a private CircleCI accessible only to the upstream org).
