# antigravity-aur-release

Builds a user-provided Antigravity IDE Linux tarball as an Arch package.

The package installs the IDE under `/opt/antigravity-ide`, adds the command
`antigravity-ide` to `PATH`, and registers it as a normal desktop application
with an application-menu entry and icon. Arguments are passed through to the
upstream launcher, for example:

```sh
antigravity-ide .
antigravity-ide path/to/file
```

## Build locally

Install the Arch packaging tools, then provide the tarball URL and run:

```sh
ANTIGRAVITY_IDE_URL='https://example.invalid/Antigravity%20IDE.tar.gz' \
ANTIGRAVITY_IDE_VERSION='2.5.5' \
  makepkg --syncdeps --clean --cleanbuild
sudo pacman -U antigravity-ide-*.pkg.tar.zst
```

`ANTIGRAVITY_IDE_SHA256` is optional, but should be supplied whenever the user
has a checksum. Add `ANTIGRAVITY_IDE_SHA256='actual-sha256'` to the command if
available. If omitted, makepkg uses `SKIP` for the user-provided archive.

## GitHub Actions

Run the workflow manually, enter the tarball URL and package version, and
optionally provide a checksum and release tag. It builds the package in an
Arch Linux container, uploads it as an Actions artifact, and creates a GitHub
release when a release tag is supplied. It does not submit to the AUR
automatically.
