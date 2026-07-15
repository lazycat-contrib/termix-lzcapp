# Termix for LazyCat

This repository packages [Termix](https://github.com/Termix-SSH/Termix) as an
LPK v2 application for LazyCat.

## Runtime

- Package: `community.lazycat.app.termix`
- Termix image: `docker.1ms.run/bugattiguy527/termix:2.5.0`
- guacd image: `docker.1ms.run/guacamole/guacd:1.6.0`
- guacd host: `GUACD_HOST=guacd`
- Persistent data: `/lzcapp/var/data` → `/app/data`
- Minimum LazyCat OS: `1.5.0`

## Build

```bash
lzc-cli project release -o dist/application.lpk
lzc-cli lpk info dist/application.lpk
```

## Automated updates

`.github/workflows/lazycat-update.yml` checks Docker image updates every day at
02:17 UTC and can also be started manually. Termix is the package version
source; the workflow updates its explicit service image target, builds a
versioned GitHub Release asset, and publishes it to the MiaoMiao private store.
The guacd dependency remains pinned to `1.6.0`, because changing a secondary
image without changing the Termix package version would conflict with the
existing Git tag and versioned Release asset.

Configure these GitHub Actions secrets:

- `APPSTORE_URL` (required)
- `APPSTORE_TOKEN` (required)
- `APP_ID` (optional for an existing store application)
- `PRIVATE_STORE_GROUP_CODES` (optional, comma-separated private group codes)

The workflow uses mirror delivery with digest verification, so the official
LazyCat store is intentionally disabled while `docker.1ms.run` is used.
