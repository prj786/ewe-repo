# ewe-repo

The pacman repository for the [ewe desktop](https://github.com/prj786/ewe).
The rolling **x86_64** release of this repo is the repository: `ewe.db` plus
every package, rebuilt by the `publish` workflow.

## Use it

Append to `/etc/pacman.conf` (ewe's `install.sh` does this for you):

```ini
[ewe]
SigLevel = Optional TrustAll
Server = https://github.com/prj786/ewe-repo/releases/download/x86_64
```

Then:

```sh
sudo pacman -Syu        # updates the DE like any other package
sudo pacman -S ewe      # or: pull the whole desktop onto a fresh Arch install
```

`pacman -S ewe` installs the payload to `/usr/share/ewe` and every dependency
(the full package lists from the ewe repo, plus Komble, ewe-settings and
ewe-sync, plus
prebuilt copies of the AUR packages listed in `aur-packages.txt`). The payload
carries the account tools — `ewe-cloud` for the user's own Nextcloud
(RFC-005), `ewe-caldav`, `ewe-mail`, `ewe-conf` with its WebDAV sync — and
**no Google client**: Google is optional and needs the user's own OAuth client
file. Per-user
deployment is `ewe-setup`; system setup (greeter, plymouth, hibernate) is
`/usr/share/ewe/install.sh`. After that, the session refreshes itself at login
whenever pacman has delivered a newer payload.

## Publish

The `publish` workflow (manual `workflow_dispatch`, a `repository_dispatch` of
type `publish`, or the Monday cron) rebuilds everything from the latest
releases and AUR PKGBUILDs and recreates the x86_64 release. Run it after any
ewe / komble-arch / ewe-settings / ewe-sync release.

Packages are currently unsigned (`SigLevel = Optional TrustAll`); repo signing
is planned before the standalone-distro release.
