pkgname=st-mhl-git
_pkgname=st
pkgver=0.7.r41.g1f24bde
pkgrel=1
pkgdesc='Simple virtual terminal emulator for X with patches'
arch=('i686' 'x86_64')
url='https://st.suckless.org/'
license=('MIT')
options=('zipman')
depends=('libxft' 'xorg-fonts-misc')
makedepends=('ncurses' 'libxext' 'git')
optdepends=('dmenu: for unicode input'
            'iosevka-term-slab: default defined font' )

_patches=(
    "https://st.suckless.org/patches/fix_keyboard_input/st-fix-keyboard-input-20170621-b331da5.diff"
    "https://st.suckless.org/patches/alpha/st-alpha-20171221-0ac685f.diff"
    "https://st.suckless.org/patches/hidecursor/st-hidecursor-20170404-745c40f.diff"
    "https://st.suckless.org/patches/scrollback/st-scrollback-20170329-149c0d3.diff"
    "https://st.suckless.org/patches/vertcenter/st-vertcenter-20170601-5a10aca.diff"
)

source=("git://git.suckless.org/st"
        "config-mhl.h"
    "${_patches[@]}")
    
sha1sums=('SKIP'
          'SKIP'
          '8647a39bfdfd5a3e49ed13b5f1be57cf919459b4'
          'd49ca2170d5aac51a34ae4501d43eab92c98c41a'
          'b85e06f6190a55c2754fd47a1ff3f3dfd74722f6'
          '6a41683a04375f192d014cead227c749c595a721'
          'e850d9c17119ea213c62b1c3d8088f4a17c7a325'
         )

provides=("${_pkgname}")
conflicts=("${_pkgname}")

pkgver() {
  cd "${_pkgname}"
  git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
  local file
  pushd "${_pkgname}"

  # skip terminfo which conflicts with nsurses
  sed -i '/tic /,+1d' Makefile

  for file in "${_patches[@]}"; do
    if [[ "$file" == *.h ]]; then
      cp "$srcdir/$file" .
    elif [[ "$file" == *.diff || "$file" == *.patch ]]; then
      echo "Applying patch $(basename $file)..."
      patch -Np1 <"$srcdir/$(basename ${file})"
    fi
  done

  popd

  cp config-mhl.h "${_pkgname}/config.h"
}

build() {
  cd "${_pkgname}"
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd "${_pkgname}"
  make PREFIX=/usr DESTDIR="${pkgdir}" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  install -Dm644 README "${pkgdir}/usr/share/doc/${pkgname}/README"
}

# vim:ts=2 sw=2 et
