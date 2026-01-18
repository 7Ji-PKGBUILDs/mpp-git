# Maintainer: Mahmut Dikcizgi <boogiepop a~t gmx com>
# Maintainer: 7Ji <pugokushin@gmail.com>

pkgname='mpp-git'
pkgver=1.0.11.r4191.a9380ef3
pkgrel=1
pkgdesc='Rockchip VPU Media Process Platform (MPP) for hardware video decode latest revision from git'
arch=('x86_64' 'aarch64' 'armv7h')
url='https://github.com/hermanChen/mpp'
license=('Apache')
depends=('gcc-libs' 'coreutils' 'udev')
makedepends=('cmake' 'gitweb-dlagent')
provides=({rockchip-,}mpp="${pkgver}")
conflicts=({rockchip-,}mpp)
options=(!lto strip)
install='install'
_url="gitweb-dlagent://github.com/nyanmisaka/rk-mirrors.git#branch=jellyfin-mpp"
source=(
  $_url
  '60-mpp.rules'
  'install'
)
sha256sums=('SKIP'
  '05c4e6bf492d982db968e4f0a679634688c08c295f975a884cb2a4b12c65ab86'
  'e41004dc18f77d37b23f84464c4367c7ccf94d8e86b6f751437b685322e153d2'
)
DLAGENTS+=('gitweb-dlagent::/usr/bin/gitweb-dlagent sync %u')

build() {
  cd rk-mirrors
  cmake -S . -B build \
    -DCMAKE_BUILD_TYPE=RelWithDebInfo \
    -DCMAKE_INSTALL_PREFIX=${pkgdir}/usr \
    -DCMAKE_POLICY_VERSION_MINIMUM=3.5 \
    -DBUILD_TEST=OFF
  cmake --build build
}

pkgver() {
  cd rk-mirrors
  local _vers=$(sed -n 's/##[[:space:]]\+\([0-9.]\+\).*/\1/p' CHANGELOG.md | head -n 1)
  local _commits=$(gitweb-dlagent version ${_url} --pattern \{revision\})
  local _hash=$(gitweb-dlagent version ${_url} --pattern \{commit:.8s\})
  printf "%s.r%s.%s" $_vers $_commits $_hash
}

package() {
  cd rk-mirrors
  cmake --install build

  # mpp needs to access /dev/mpp_service /dev/rga /dev/dma_heap/ ad so on
  # access to those devices are +rw'ed to video groups with those rules
  install -m644 -Dt "${pkgdir}/usr/lib/udev/rules.d" ../60-mpp.rules
  install -m644 -Dt "${pkgdir}/usr/lib/udev/rules.d" ../60-mpp.rules
  # install -m644 -Dt "${pkgdir}/usr/lib" ${srcdir}/build/mpp/libmpp_ext.so
}
