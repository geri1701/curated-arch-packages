# Curated Arch Packages

A deliberately limited set of manually reviewed Arch package recipes. Packages
are built locally by Paru. This repository does not use the AUR as a package
source and does not distribute binaries.

## Packages

| Package | Purpose |
| --- | --- |
| [`archon-appimage`](packages/archon-appimage/) | Desktop uploader for Archon |
| [`curated-aur-search`](packages/curated-aur-search/) | Read-only AUR metadata search |
| [`ferdium`](packages/ferdium/) | Multi-service messaging app |
| [`headset-charge-indicator-git`](packages/headset-charge-indicator-git/) | Battery indicator for supported wireless headsets |
| [`mediatek-mt7927-dkms`](packages/mediatek-mt7927-dkms/) | DKMS Wi-Fi and Bluetooth drivers for MediaTek MT7927 hardware |
| [`pw-duck`](packages/pw-duck/) | PipeWire audio ducking tray app |
| [`wago-app-bin`](packages/wago-app-bin/) | Wago addon and WeakAura manager |
| [`wowup-cf-bin`](packages/wowup-cf-bin/) | World of Warcraft addon updater with CurseForge support |
| [`xwaylandvideobridge`](packages/xwaylandvideobridge/) | Wayland window sharing for X applications |

Recipes live under `packages/<pkgbase>/` with matching `.SRCINFO` metadata.

## Paru setup

Before starting, make sure none of the packages listed above are installed.

Open `~/.config/paru/paru.conf`, or create it if it does not exist. Add the
contents of [`config/paru.conf`](config/paru.conf) to it. Then use Paru normally:

```sh
paru -Syu
paru -S pw-duck
```

AUR metadata remains available through the separate read-only search command:

```sh
paru -S curated-aur-search
aur-search headset
```

`aur-search` cannot build or install packages. Paru itself is restricted to
official repositories and the reviewed recipes in this repository.

## Validation

Package changes are built and installed as an unprivileged user in Arch Linux.
CI checks `.SRCINFO`, source hashes, package tests, `namcap`, Paru discovery,
and the AUR installation boundary. Full Paru installation is tested manually
before publication. Updates are reviewed and published manually; upstream
checks are report-only.

Repository-authored files are MIT licensed. `PKGBUILD` license metadata and
package-specific notices apply to the packaged software.
