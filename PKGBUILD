# Maintainer: Martchus <martchus@gmx.net>

# All my PKGBUILDs are managed at https://github.com/Martchus/PKGBUILDs where
# you also find the URL of a binary repository.

_name=libfilezilla
pkgname=mingw-w64-libfilezilla
pkgver=0.31.1
pkgrel=1
pkgdesc='Small and modern C++ library, offering some basic functionality to build high-performing, platform-independent programs (mingw-w64)'
arch=('any')
url='https://filezilla-project.org/'
license=('GPL')
depends=('mingw-w64-crt')
makedepends=('mingw-w64-configure' 'mingw-w64-gmp' 'mingw-w64-nettle' 'mingw-w64-gnutls')
options=(staticlibs !strip !buildflags !makeflags)
install=
source=("https://download.filezilla-project.org/libfilezilla/libfilezilla-$pkgver.tar.bz2")
md5sums=('a6e3157655e285410e6d12c7c76f0f26')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

prepare() {
  cd "${srcdir}/${_name}-${pkgver}"
  autoreconf -i
}

build() {
  export CXXFLAGS='-O0'
  for _arch in ${_architectures}; do
    mkdir -p "${srcdir}/${_name}-${pkgver}/build-${_arch}" && cd "${srcdir}/${_name}-${pkgver}/build-${_arch}"
    ${_arch}-configure --disable-shared
    # shared build doesn't work because 'fz::simple_event<fz::timer_event_type, unsigned long long>::type()'
    # is not exportet correctly
    make
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}/${_name}-${pkgver}/build-${_arch}"
    make DESTDIR="${pkgdir}" install
    find "${pkgdir}/usr/${_arch}" -name '*.exe' -exec ${_arch}-strip --strip-all {} \;
    find "${pkgdir}/usr/${_arch}" -name '*.dll' -exec ${_arch}-strip --strip-unneeded {} \;
    find "${pkgdir}/usr/${_arch}" -name '*.a'   -exec ${_arch}-strip -g {} \;
  done
}
