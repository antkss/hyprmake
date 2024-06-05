# Maintainer: FabioLolix
# Maintainer: éclairevoyant
# Contributor: ThatOneCalculator <kainoa at t1c dot dev>

pkgname=hyprland-test
pkgver=0.40.0
pkgrel=1
pkgdesc="A dynamic tiling Wayland compositor based on wlroots that doesn't sacrifice on its looks."
arch=(x86_64)
license=(BSD)
depends=(
  cairo
  gcc-libs
  glib2
  glibc
  glslang
  libdisplay-info
  libdrm
  libglvnd
  libinput
  libliftoff
  libx11
  libxcb
  libxcomposite
  libxfixes
  libxkbcommon
  libxrender
  opengl-driver
  pango
  pixman
  polkit
  seatd
  systemd-libs
  tomlplusplus
  wayland
  wayland-protocols
  xcb-proto
  xcb-util
  xcb-util-errors
  xcb-util-keysyms
  xcb-util-renderutil
  xcb-util-wm
  xorg-xinput
  xorg-xwayland
  hyprlang
  hyprcursor
)
depends+=(libdisplay-info.so)
makedepends=(
  cmake
  gdb
  git
  jq
  meson
  ninja
  pkgconf
  xorgproto
)
provides=("hyprland=mybuild")
conflicts=(hyprland)
options=(strip !docs !debug)

pkgver() {
    cd hyprland

  cat props.json | jq -r .version
}
prepare() {
    if [ -d hyprland ]; then
	cd hyprland
	git pull 
    else 
	git clone --recursive https://github.com/antkss/hyprland
    fi
}

build() {
    cd hyprland
    if [ -d build ]; then
	cd build 
    else
	mkdir build
	cd build
    fi
    cmake .. 
    make
}

package() {

 # if [ -d hyprland-npi ]; then
 #   cd hyprland-npi
 # elif [ -d hyprland-source ]; then
 #   cd hyprland-source
 # elif [ -d hyprland ]; then
 #   cd hyprland
 # else
 #   echo "tried all !!!! errors"
 # fi
cd $srcdir/hyprland
# if [ -z $(grep "cmake --install ./build --prefix \$\(PREFIX\)" Makefile) ]; then
#   sed -i 's/cmake --install .\/build/cmake --install .\/build --prefix $(PREFIX)/g' Makefile
# else 
#   echo "file is already patched"
# fi

cmake --install ./build --prefix "$pkgdir/usr"
make installheaders PREFIX="$pkgdir/usr"
# strip -v $pkgdir/usr/bin/Hyprland
# strip -v $pkgdir/usr/bin/hyprpm
# strip -v $pkgdir/usr/bin/hyprctl
# strip -v $pkgdir/usr/lib/libwlroots.so.13032
cp ../../.ori.png "$pkgdir/usr/share/hyprland/wall2.png"
chmod 777 "$pkgdir/usr/share/hyprland/wall2.png"
}
