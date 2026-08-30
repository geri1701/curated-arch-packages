# OpenLinkHub Xbox headset test package

`openlinkhub-xbox-test` is an explicit opt-in build for hardware testing of the
Corsair VOID MAX WIRELESS for Xbox receiver (`1b1c:2a05`) and paired headset
(`1b1c:2a07`). The source is pinned to a signed commit in the public
experimental fork. Protocol compatibility has not yet been proven on real
hardware.

The package provides and conflicts with `openlinkhub`, but does not replace it
automatically. Do not remove the installed CachyOS package separately: Pacman
must remove it as part of the same transaction that installs this package.

## Prepare

Before testing:

1. Confirm the current package and keep its signed package file available for
   rollback. For the initial test, the expected package is
   `openlinkhub 0.8.8-3`.
2. Stop the service and make a consistent backup of `/var/lib/openlinkhub`.
3. Record the backup path.

Example backup:

```sh
pacman -Q openlinkhub
sudo pacman -Sw openlinkhub
sudo systemctl disable --now openlinkhub
backup="$HOME/openlinkhub-test-backup-$(date +%Y%m%d-%H%M%S)"
mkdir -m 700 "$backup"
sudo cp -a /var/lib/openlinkhub "$backup/"
printf '%s\n' "$backup"
```

## Install

Install explicitly:

```sh
paru -S openlinkhub-xbox-test
```

Pacman will report that `openlinkhub-xbox-test` conflicts with `openlinkhub`
and ask whether `openlinkhub` may be removed. Confirm only that expected
removal. The removal and installation then occur in one transaction.

Start the service after the transaction:

```sh
sudo systemctl enable --now openlinkhub
systemctl --no-pager --full status openlinkhub
```

Stop testing if the package transaction or service fails, unrelated devices
regress, or audio and microphone operation change unexpectedly. Redact USB
serial numbers before sharing logs or descriptors.

## Roll back

Install the saved CachyOS package with `pacman -U`. Pacman will remove
`openlinkhub-xbox-test` as part of the same transaction. Do not remove the test
package separately.

```sh
sudo pacman -U /path/to/openlinkhub-0.8.8-3-x86_64.pkg.tar.zst
```

After the transaction, preserve the test state and restore the recorded
pre-test backup. Set `backup` to the path printed during preparation:

```sh
backup=/path/recorded/during/preparation
sudo mv /var/lib/openlinkhub \
  "/var/lib/openlinkhub.after-test-$(date +%Y%m%d-%H%M%S)"
sudo cp -a "$backup/openlinkhub" /var/lib/openlinkhub
sudo systemctl enable --now openlinkhub
pacman -Q openlinkhub
```

The expected restored package for the initial test is `openlinkhub 0.8.8-3`.
