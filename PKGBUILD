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
  meson setup build \
    --prefix     /usr \
    --libexecdir lib \
    --sbindir    bin \
    --buildtype  release \
    --wrap-mode  nodownload \
    --optimization  3 		\
    -D          b_lto=true \
    -D          b_pie=false \
    -D          default_library=shared \
    -D          xwayland=enabled \
    -D          systemd=enabled \
    -Dcc.link_args='-z norelro -fno-stack-protector'

}

package() {

    cd $srcdir/hyprland
    meson install -C build \
    --destdir "$pkgdir" \
    --skip-subprojects hyprland-protocols
    make installheaders PREFIX="$pkgdir/usr"
    cp ../../.ori.png "$pkgdir/usr/share/hyprland/wall2.png"
    chmod 777 "$pkgdir/usr/share/hyprland/wall2.png"
}
