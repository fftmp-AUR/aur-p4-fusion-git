# Maintainer: fft

pkgname=p4-fusion-git
pkgver=v1.13.r14.g307aca8
pkgrel=2
pkgdesc='Perforce to Git conversion tool'
arch=('x86_64')
url='https://github.com/salesforce/p4-fusion'
depends=('libgit2' 'openssl')
makedepends=('git')
license=('BSD-3-Clause')
conflicts=(p4-fusion) # for the future, if anybody else will package it.
provides=(p4-fusion)
source=(
  "${pkgname}::git+https://github.com/salesforce/p4-fusion.git"
  "https://www.perforce.com/downloads/perforce/r24.2/bin.linux26x86_64/p4api-glibc2.12-openssl3.tgz"
  'p1.patch::https://github.com/fftmp-forked/p4-fusion/commit/2e51b94c0baf55ef6b5bbe42cc13c39d61573dff.patch'
  'p2.patch::https://github.com/fftmp-forked/p4-fusion/commit/4b0d8bb01afeafd06ee412fabc2f753107b8837a.patch'
  'p3.patch::https://github.com/fftmp-forked/p4-fusion/commit/c95d73496b14c6d9938f3a80473f096942e757ed.patch'
  'p4.patch::https://github.com/fftmp-forked/p4-fusion/commit/75fd3580202373de9645819a2d0460ab2a4625c6.patch'
)

sha256sums=(
  'SKIP'
  'add5a540481f9084583735a624153fe1e7dd8a665ecd9ef5103e1865977dbde6'
  '77fb9d59a2869674d7af82ecc9d5fe1e8d5380b7d9d51d7373346f9d0d118b71'
  '7273eac268ab8e3507359ec747551e9391eeadbfe2c9a6bb2619c15f112e8b41'
  '2c3c14f6eb71c788decd4a4c31878ceabb2dadadc44542c6bd18e4f749947fb0'
  '5a87aed0b07b0a6e9088810e8dd44a9591059d0c12b7eb516737aa790b59fba8'
)

pkgver() {
  cd "${pkgname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  cd "${pkgname}"
  git clean -fxd
  git apply "${srcdir}/p1.patch"
  git apply "${srcdir}/p2.patch"
  git apply "${srcdir}/p3.patch"
  git apply "${srcdir}/p4.patch"
  mkdir -p 'vendor/helix-core-api/linux/'
  cp -r '../p4api-2024.2.2697822/include/' '../p4api-2024.2.2697822/lib/' './vendor/helix-core-api/linux/'
}

build() {
  cd "${pkgname}"
  cmake ./
  cmake --build ./
}

package() {
  cd "${pkgname}"
  install -Dm755 'p4-fusion/p4-fusion' "${pkgdir}/usr/bin/p4-fusion"
  install -Dm644 "${srcdir}/${pkgname}/LICENSE.txt" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.txt"
}
