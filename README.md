# grace-releases

Signed build artifacts for Grace, a macOS desktop app. This repository holds no source code — only
the release bundles the app's updater downloads, plus the `latest.json` manifest that points at them.

## Why this exists

The updater has to fetch its manifest without credentials, so the manifest and the bundles have to
live somewhere public. Distributing them here keeps the application source private without shipping
a token inside the app.

## Is a public artifact host safe?

Yes. Every bundle is signed with a minisign key that lives only in CI, and the app ships with the
matching **public** key. A bundle served from anywhere other than a real release — or tampered with
in transit — fails signature verification and is refused before it is ever installed.

## Layout

Each release carries:

| Asset | Purpose |
| --- | --- |
| `Grace_<version>_universal.app.tar.gz` | The update payload the app downloads |
| `Grace_<version>_universal.app.tar.gz.sig` | Its detached minisign signature |
| `Grace_<version>_universal.dmg` | Manual download for a fresh install |
| `latest.json` | Update manifest: version, notes, and a signed URL per platform |

The updater reads `releases/latest/download/latest.json`, so the newest published release is always
the one offered.

## Publishing

Releases are pushed here automatically by CI in the source repository. Nothing in this repository is
edited by hand.
