# Maintainer: Gustavo Alvarez <sl1pkn07@gmail.com>
pkgname=yabause-git
pkgver=20130301
pkgrel=1
pkgdesc="A Sega Saturn emulator (GIT version)"
arch=('x86_64' 'i386')
url="http://yabause.org/"
license=('GPL')
conflicts=('yabause')
provides=('yabause')
depends=('mini18n-git' 'openal' 'sdl' 'freeglut')
makedepends=('git' 'cmake')

#Enable QT or GTK
#_port=gtk
_port=qt

[ "${_port}" = "qt" ] && depends+=('qt4')
[ "${_port}" = "gtk" ] && depends+=('gtkglext')

_gitroot="git://github.com/Guillaumito/yabause.git"
_gitname="yabause"

build() {
  cd "${srcdir}"
  msg "Connecting to GIT server...."

  if [ -d "${_gitname}" ]; then
    cd "${_gitname}" && git pull
    msg "The local files are updated."
  else
    git clone --depth=1 "${_gitroot}" "${_gitname}"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build..."

  rm -rf "${srcdir}/${_gitname}-build"
  cp -R "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"
  cd "${srcdir}/${_gitname}-build/${_gitname}"

  LDFLAGS+=",-z,noexecstack"
  cmake . -DCMAKE_INSTALL_PREFIX=/usr -DYAB_PORTS="${_port}" -DYAB_NETWORK=ON -DYAB_OPTIMIZED_DMA=ON -DYAB_PERKEYNAME=ON
  make
}

package() {
  cd "${srcdir}/${_gitname}-build/${_gitname}"
  make DESTDIR="${pkgdir}/" install
}
