# Contributor: Otakar Haška <ota.haska@seznam.cz> 

pkgname=scribus-ece-15
pkgver=baf572f
pkgrel=1
pkgdesc="A desktop publishing program - Everybody Can Enhance"
arch=(i686 x86_64)
license=('GPL')
url="http://www.scribus-ece.info"
install=${pkgname}.install
depends=('libcups>=1.2.10' 'lcms>=1.17' 'qt4>=4.3.0' \
	'ghostscript>=8.60' 'libart-lgpl>=2.3.19' 'python>=2.5' \
       	'libxml2>=2.6.27' 'cairo' 'desktop-file-utils' 'freetype2>=2.3.0' \
	'libtiff>=3.6.0' 'libjpeg' 'podofo')
makedepends=('subversion' 'cmake' 'gcc>=4.0' 'make')
# options=(!libtool force !makeflags)
replaces=()
source=('scribus-ece-15.desktop')

_gitroot=('git://scribus-ece.info/ScribusECE')
_gitname=('ece15')

pkgver() {
  cd $srcdir/ScribusECE
  git describe --always | sed 's|-|.|g'
}


build() {
  cd $startdir/src
  msg "Connecting to git server...."
  [ ! -d "$srcdir/ScribusECE-build" ] || rm -rf "$srcdir/ScribusECE-build"
  
  if [[ -d ScribusECE ]]; then
   cd ScribusECE || return 1
   git pull origin || return 1
    else
   git clone -b $_gitname  $_gitroot || return 1
     fi
  msg " checkout done."

    msg "Starting make..."
    cd "$srcdir"
    #rm -r "$srcdir/$_gitname-build"
    cp -r $srcdir/ScribusECE ScribusECE-build
    cd ScribusECE-build/Scribus
    
export CMAKE_PREFIX_PATH=/usr/local
export CMAKE_LIBRARY_PATH=/usr/local/lib
export CMAKE_INCLUDE_PATH=/usr/local/include
echo $CMAKE_PREFIX_PATH

#  cmake ../$_svnmod -DCMAKE_INSTALL_PREFIX:PATH=/opt/scribus-svn -DWANT_SYSTEM_CAIRO=1 \
#    -DCMAKE_LIBRARY_PATH:PATH=/usr/local/lib -DCMAKE_INCLUDE_PATH:PATH=/usr/local/include -DCMAKE_PREFIX_PATH:PATH=/usr/local

  cmake ../Scribus -DCMAKE_INSTALL_PREFIX:PATH=/opt/scribus-ece15 \
		    -DCMAKE_SKIP_RPATH:BOOL=YES \
		    -DWANT_GRAPHICSMAGICK=1 \
		    -DWANT_SYSTEM_CAIRO=1 \
		    -DCMAKE_INCLUDE_PATH=/usr/include/python2.7

  sed -i 's|-Wl,--as-needed||' scribus/CMakeFiles/scribus.dir/link.txt
  make || return 1
}


package() {
cd $srcdir/ScribusECE-build/Scribus
  make DESTDIR=$pkgdir install || return 1

  install -Dm644 ${startdir}/scribus-ece-15.desktop \
    ${pkgdir}/usr/share/applications/scribus-ece-15.desktop

}

md5sums=('d955f9e27b10a9c614d366232318cf6c')
