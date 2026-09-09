# OpenLinkHub VOID MAX Wireless V2 test package

`openlinkhub-xbox-test` is an explicit opt-in build with experimental support for
the Corsair VOID MAX WIRELESS V2 through its "for Xbox" receiver
(`1b1c:2a05`) and paired headset (`1b1c:2a07`). The source is pinned to a
signed commit in the public experimental fork.

Device recognition, audio playback, microphone recording, microphone muting,
and LED control were verified on the target hardware on 2026-08-30. The package
remains experimental because not every device mode and protocol control has
been covered.

## Known limitation

OpenLinkHub puts the headset into software mode while controlling it. Raising
the microphone still mutes it, but the headset's onboard audible mute/unmute
signal is unavailable in that mode. The same mode-dependent behavior was
[reproduced upstream with iCUE on another Corsair wireless headset][issue-209].
No evidence-backed protocol command for restoring the signal is known, so this
package does not emulate or guess one.

[issue-209]: https://github.com/jurkovic-nikola/OpenLinkHub/issues/209

## Prepare the first installation

Before switching from a distribution package:

1. Record the current package and keep its signed package file for rollback.
2. Stop the service and make a consistent backup of `/var/lib/openlinkhub`.
3. Record the backup path.

Fish example:

```fish
pacman -Q openlinkhub
sudo pacman -Sw openlinkhub
sudo systemctl disable --now openlinkhub
set backup "$HOME/openlinkhub-test-backup-"(date +%Y%m%d-%H%M%S)
mkdir -m 700 "$backup"
sudo cp -a /var/lib/openlinkhub "$backup/"
printf '%s\n' "$backup"
```

## Install and update

Install the test package explicitly:

```fish
paru -S openlinkhub-xbox-test
```

Pacman will report that `openlinkhub-xbox-test` conflicts with `openlinkhub`.
Confirm only that expected removal. Removal and installation then occur in one
transaction; do not uninstall `openlinkhub` separately.

Start the service after the transaction:

```fish
sudo systemctl enable --now openlinkhub
systemctl --no-pager --full status openlinkhub
```

Once installed, routine package updates use the normal command:

```fish
paru -Syu
```

Stop if a package transaction or service fails, unrelated devices regress, or
audio and microphone operation change unexpectedly. Redact USB serial numbers
before sharing logs or descriptors.

## Roll back

Install the saved distribution package or a previous test package with
`pacman -U`. When returning to `openlinkhub`, Pacman removes
`openlinkhub-xbox-test` as part of the same transaction. Do not remove the test
package separately.

```fish
sudo pacman -U /path/to/saved-package.pkg.tar.zst
```

Preserve the current state before restoring the recorded pre-test backup. Set
`backup` to the path printed during preparation:

```fish
set backup /path/recorded/during/preparation
sudo mv /var/lib/openlinkhub \
  "/var/lib/openlinkhub.after-test-"(date +%Y%m%d-%H%M%S)
sudo cp -a "$backup/openlinkhub" /var/lib/openlinkhub
sudo systemctl enable --now openlinkhub
```
