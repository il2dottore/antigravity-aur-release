# Maintainer: il2dottore
pkgname=antigravity-ide
pkgver="${ANTIGRAVITY_IDE_VERSION:-1}"
pkgrel=1
pkgdesc='Agent-first desktop IDE from Google'
arch=('x86_64')
url='https://antigravity.google/'
license=('custom')
options=('!strip' '!debug')
depends=(
  'alsa-lib'
  'at-spi2-core'
  'cairo'
  'dbus'
  'expat'
  'gcc-libs'
  'glib2'
  'gtk3'
  'libdrm'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxfixes'
  'libxkbcommon'
  'libxrandr'
  'libxss'
  'libxtst'
  'mesa'
  'nss'
  'pango'
)
depends+=(
  'desktop-file-utils'
  'hicolor-icon-theme'
)
optdepends=('git: version control integration')
conflicts=('antigravity')

if [[ -z ${ANTIGRAVITY_IDE_URL:-} ]]; then
  printf '%s\n' 'Set ANTIGRAVITY_IDE_URL to the upstream tarball URL before running makepkg.' >&2
  exit 1
fi

_tarball_sha256="${ANTIGRAVITY_IDE_SHA256:-SKIP}"
source=(
  "antigravity-ide.tar.gz::${ANTIGRAVITY_IDE_URL}"
  'antigravity-ide.desktop'
)
sha256sums=(
  "${_tarball_sha256}"
  'SKIP'
)

prepare() {
  desktop-file-validate antigravity-ide.desktop

  # The upstream archive has a directory name containing a space. Rename it
  # once so the installation phase stays readable and deterministic.
  mv 'Antigravity IDE' antigravity-ide
}

package() {
  local appdir="$pkgdir/opt/antigravity-ide"

  install -d "$appdir"
  cp -a --no-preserve=ownership antigravity-ide/. "$appdir/"

  # Electron's sandbox requires this exact mode when installed system-wide.
  chmod 4755 "$appdir/chrome-sandbox"

  install -d "$pkgdir/usr/bin"
  ln -s /opt/antigravity-ide/bin/antigravity-ide \
    "$pkgdir/usr/bin/antigravity-ide"

  install -Dm644 antigravity-ide.desktop \
    "$pkgdir/usr/share/applications/antigravity-ide.desktop"
  install -Dm644 "$appdir/resources/app/resources/linux/code.png" \
    "$pkgdir/usr/share/icons/hicolor/1024x1024/apps/antigravity-ide.png"
  install -Dm644 "$appdir/resources/app/resources/linux/code.png" \
    "$pkgdir/usr/share/pixmaps/antigravity-ide.png"
  install -Dm644 "$appdir/resources/app/LICENSE.txt" \
    "$pkgdir/usr/share/licenses/antigravity-ide/LICENSE.txt"

  install -Dm644 \
    "$appdir/resources/completions/bash/antigravity-ide" \
    "$pkgdir/usr/share/bash-completion/completions/antigravity-ide"
  install -Dm644 \
    "$appdir/resources/completions/zsh/_antigravity-ide" \
    "$pkgdir/usr/share/zsh/site-functions/_antigravity-ide"

  # The archive is a prebuilt binary distribution; no stripping or compiler
  # work is required, and preserving its runtime files is intentional.
  find "$appdir" -type d -exec chmod 755 {} +
}
