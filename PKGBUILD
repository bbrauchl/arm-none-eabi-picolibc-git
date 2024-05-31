# Maintainer: Sebastian Weiss <dl3yc@darc.de>

_target=arm-none-eabi
pkgname=$_target-picolibc-git
pkgver=1.8.6.r232.g695583c3b
pkgrel=1
pkgdesc='Fork of newlib with stdio bits from avrlibc'
arch=('i686' 'x86_64')
url='https://github.com/picolibc/picolibc'
license=('GPL')
makedepends=("$_target-gcc" 'git' 'meson')
provides=("arm-none-eabi-picolibc=$pkgver")
source=("git+https://github.com/picolibc/picolibc.git")
sha256sums=('SKIP')
options=(!strip !buildflags)

pkgver() {
  cd "picolibc"

  _tag=$(git tag -l --sort -creatordate | grep -E '^v?[0-9\.]+$' | head -n1)
  _rev=$(git rev-list --count $_tag..HEAD)
  _hash=$(git rev-parse --short HEAD)
  printf "%s.r%s.g%s" "$_tag" "$_rev" "$_hash" | sed 's/^v//'
}

build() {
  cd "picolibc"

  meson setup \
    --prefix="/usr/${_target}" \
    --includedir="include/picolibc" \
    --libdir="lib/picolibc" \
    --debug \
    --optimization=s \
    -Dspecsdir="lib" \
    --cross-file="scripts/cross-${_target}.txt" \
    "_build"

  meson compile -C "_build"
}

package() {
  cd "picolibc"

  meson install -C "_build" --destdir "$pkgdir"
  install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname/" "$srcdir/picolibc/COPYING."{GPL2,NEWLIB,picolibc}
}
