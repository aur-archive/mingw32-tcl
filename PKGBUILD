pkgname=mingw32-tcl
pkgver=8.5.11
pkgrel=2
pkgdesc="The Tcl scripting language (mingw32)"
arch=('any')
url="http://www.tcl.tk"
license=('custom')
makedepends=('mingw32-gcc')
depends=('mingw32-runtime')
options=('!strip' '!buildflags' '!emptydirs')
source=("http://downloads.sourceforge.net/sourceforge/tcl/tcl${pkgver}-src.tar.gz")
md5sums=('b01a9691c83990b3db0ce62d1012ca67')

build() {
  cd "${srcdir}/tcl${pkgver}/win"
  unset LDFLAGS
  mkdir -p build && cd build
  ../configure \
    --host=i486-mingw32 \
    --prefix=/usr/i486-mingw32 \
    --enable-threads
  
  # don't let Makefile install tzdata and msgs
  sed -i "s/install-tzdata install-msgs//" Makefile
  
  make
}

package() {
  cd "${srcdir}/tcl${pkgver}/win/build"
  make INSTALL_ROOT=${pkgdir} all install-binaries install-libraries install-private-headers
  ln -sf tclsh85.exe "${pkgdir}/usr/i486-mingw32/bin/tclsh.exe"

  # copy tzdata and msgs manually
  mkdir -p "${pkgdir}/usr/i486-mingw32/lib/tcl85"
  cp -r "../../library/tzdata" "${pkgdir}/usr/i486-mingw32/lib/tcl85"
  cp -r "../../library/msgs" "${pkgdir}/usr/i486-mingw32/lib/tcl85"

  chmod -R 644 "${pkgdir}/usr/i486-mingw32/lib/tcl85/tzdata"
  chmod -R +X  "${pkgdir}/usr/i486-mingw32/lib/tcl85/tzdata"
  chmod -R 644 "${pkgdir}/usr/i486-mingw32/lib/tcl85/msgs"
  chmod -R +X  "${pkgdir}/usr/i486-mingw32/lib/tcl85/msgs"
  
  # remove buildroot traces / fixes #3602
  sed -i \
  -e "s,^TCL_BUILD_LIB_SPEC='-L.*/win,TCL_BUILD_LIB_SPEC='-L/usr/i486-mingw32/lib," \
  -e "s,^TCL_SRC_DIR='.*',TCL_SRC_DIR='/usr/i486-mingw32/include'," \
  -e "s,^TCL_BUILD_STUB_LIB_SPEC='-L.*/win,TCL_BUILD_STUB_LIB_SPEC='-L/usr/i486-mingw32/lib," \
  -e "s,^TCL_BUILD_STUB_LIB_PATH='.*/win,TCL_BUILD_STUB_LIB_PATH='/usr/i486-mingw32/lib," \
  -e "s,^TCL_CC_SEARCH_FLAGS='\(.*\)',TCL_CC_SEARCH_FLAGS='\1:/usr/i486-mingw32/lib'," \
  -e "s,^TCL_LD_SEARCH_FLAGS='\(.*\)',TCL_LD_SEARCH_FLAGS='\1:/usr/i486-mingw32/lib'," \
  "${pkgdir}/usr/i486-mingw32/lib/tclConfig.sh"
  
  i486-mingw32-strip $pkgdir/usr/i486-mingw32/bin/*.exe
  i486-mingw32-strip -x $pkgdir/usr/i486-mingw32/bin/*.dll
  i486-mingw32-strip -g $pkgdir/usr/i486-mingw32/lib/*.a
  i486-mingw32-strip -x $pkgdir/usr/i486-mingw32/lib/dde1.3/*.dll
  i486-mingw32-strip -x $pkgdir/usr/i486-mingw32/lib/reg1.2/*.dll
}
