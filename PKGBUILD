# Maintainer: Ralph Torres <mail at ralphptorr dot es>
# Contributor: Carl Smedstad <carl.smedstad at protonmail dot com>
# Contributor: ny-a <nyaarch64 at gmail dot com>
# Contributor: Jean Lucas <jean at 4ray dot co>

pkgname=tiny-irc-client
_pkgname=tiny
pkgver=0.11.0
pkgrel=2
pkgdesc="A terminal IRC client written in Rust"
arch=(x86_64)
url=https://github.com/osa1/tiny
license=(MIT)
provides=("$pkgname")
conflicts=("$pkgname")
depends=(
  dbus
  gcc-libs
  glibc
)
makedepends=(cargo)

source=(
  "$_pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz"
  "fix-tab-config-test-with-desktop-notifications-feature.patch"
)
sha256sums=(
  '4bd412760a35ff41220ab918702d003c710379537db9621477f63ee201a68440'
  'b7a0d1bd48f56dc1559b816e4aa3379709121da2699494723fdeeecbfd7ff021'
)
options=(!lto)

_archive="$_pkgname-$pkgver"

prepare() {
  cd "$_archive"

  patch --forward --strip=1 --input="$srcdir/fix-tab-config-test-with-desktop-notifications-feature.patch"

  cargo update
  cargo fetch --locked --target "$CARCH-unknown-linux-gnu"
}

build() {
  cd "$_archive"

  cargo build --frozen --release --features desktop-notifications
}

check() {
  cd "$srcdir"/$_pkgname-$pkgver

  cargo test --frozen --workspace --features desktop-notifications
}

package() {
  cd "$_archive"

  install -Dm755 -t "$pkgdir/usr/bin" "target/release/$_pkgname"

  install -Dm644 -t "$pkgdir/usr/share/licenses/$pkgname" LICENSE
  install -Dm644 -t "$pkgdir/usr/share/doc/$pkgname" ./*.md
  install -Dm644 -t "$pkgdir/usr/share/$_pkgname" "crates/$_pkgname/config.yml"
}
