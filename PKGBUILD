# Maintainer: fft

pkgname=p4-fusion-git
pkgver=v1.13.r14.g307aca8
pkgrel=1
pkgdesc='Perforce to Git conversion tool'
arch=('x86_64')
url='https://github.com/salesforce/p4-fusion'
depends=('openssl-1.1' 'pcre2')
makedepends=('git')
license=('BSD-3-Clause')
conflicts=(p4-fusion) # for the future, if anybody else will package it.
provides=(p4-fusion)
source=(
  "${pkgname}::git+https://github.com/salesforce/p4-fusion.git"
  "https://www.perforce.com/downloads/perforce/r24.2/bin.linux26x86_64/p4api-glibc2.12-openssl1.1.1.tgz"
  'p1.patch'
)

sha256sums=(
  'SKIP'
  'ee24e7250d87aab3f14a2cdedc384ed35393362d932e0e1a0380a55323020c9d'
  'b4400756b71b2f12993981b4eb717bcbb6776691f1dbc67d3f41df8a09a295d7'
)

pkgver() {
  cd "${pkgname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "${pkgname}"
  git clean -fxd
  git apply "${srcdir}/p1.patch"
  mkdir -p 'vendor/helix-core-api/linux/'
  cp -r '../p4api-2024.2.2675662/include/' '../p4api-2024.2.2675662/lib/' './vendor/helix-core-api/linux/'
}

build() {
  cd "${pkgname}"
  # Can't use system libgit2, because it is linked against libssl 3.0, while p4api static libs linked to openssl-1.1.
  # So compile static version of libgit2 (BUILD_SHARED_LIBS=OFF)
  PKG_CONFIG_PATH=/usr/lib/openssl-1.1/pkgconfig cmake -DBUILD_SHARED_LIBS=OFF -DREGEX_BACKEND=pcre2 ./
  cmake --build ./
}

package() {
  cd "${pkgname}"
  install -Dm755 'p4-fusion/p4-fusion' "${pkgdir}/usr/bin/p4-fusion"
  install -Dm644 "${srcdir}/${pkgname}/LICENSE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}
