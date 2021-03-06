pkgname=iscan
pkgver=2.30.1
pkgrel=1
pkgdesc="EPSON Image Scan! front-end for scanners and all-in-ones"
arch=('x86_64')
url="http://download.ebz.epson.net/dsc/search/01/search/?OSC=LX"
license=('GPL2' 'custom:AVASYSPL')
depends=('gtk2' 'sane' 'libstdc++5')
makedepends=('gettext' 'gimp')
optdepends=('iscan-data: Image Scan! data files required for some devices')
source=("http://support.epson.net/linux/src/scanner/iscan/${pkgname}_${pkgver}-1.tar.gz"
        "libpng15.patch"
        "epkowa.conf")
sha256sums=('79aa9a9f81a7146beb22a382bbaf840fbce61b1bdb82660e541946a471e48426'
            '1a75b8df945a813a297dfd6e3dabae3bc8b51898f23af31640091e31b901f0ba'
            '8e9e90fa50f1bd476b13766b19f100470c41dd253dc0605fbb1d0ac346a0beff')
install="${pkgname}.install"
backup=("etc/sane.d/epkowa.conf")

prepare() {
  cd "${pkgname}-${pkgver}"
  # patch for building iscan against libpng15 by giovanni
  patch -Np0 -i "../libpng15.patch"
  # add fix for CXX ABI different than 1002
  ln -s libesmod-x86_64.c2.so non-free/libesmod-x86_64.so
}

build() {
  cd "${pkgname}-${pkgver}"
  export LDFLAGS="${LDFLAGS} -ldl -lpng16"
  ./configure --prefix=/usr \
              --sbindir=/usr/bin \
              --enable-dependency-reduction \
              --enable-frontend \
              --enable-jpeg \
              --enable-tiff \
              --enable-png \
              --enable-gimp
  make
}

package() {
  cd "${pkgname}-${pkgver}"

  # install files
  make DESTDIR="${pkgdir}" install

  # install sane configuration files
  install -m 755 -d "${pkgdir}/etc/sane.d"
  install -m 644 -D "backend/epkowa.conf" "${pkgdir}/etc/sane.d/"
  install -m 755 -d "${pkgdir}/etc/sane.d/dll.d"
  install -m 644 "${srcdir}/epkowa.conf" "${pkgdir}/etc/sane.d/dll.d/"

  # install desktop file
  install -m 755 -d "${pkgdir}/usr/share/applications"
  install -m 644 "${pkgname}.desktop" "${pkgdir}/usr/share/applications/"

  # install license
  install -m 755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m 644 "non-free/AVASYSPL.en.txt" "${pkgdir}/usr/share/licenses/${pkgname}/AVASYSPL"

  # install plugin for GIMP
  install -m 755 -d "${pkgdir}/usr/lib/gimp/2.0/plug-ins"
  ln -s "/usr/bin/iscan" "${pkgdir}/usr/lib/gimp/2.0/plug-ins/"
}

