# Maintainer: FabioLolix
# Maintainer: éclairevoyant
# Contributor: ThatOneCalculator <kainoa at t1c dot dev>

pkgname=hyprland-git
pkgver=0.46.0
pkgrel=1
pkgdesc="A dynamic tiling Wayland compositor based on wlroots that doesn't sacrifice on its looks."
arch=(x86_64)
license=(BSD)
depends=(
  hyprgraphics
  cairo
  gcc-libs
  glib2
  glibc
  glslang
  # libdisplay-info
  aquamarine
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
  # libdisplay-info.so
  hyprwayland-scanner
  hyprutils
)
# depends+=(libdisplay-info.so)
makedepends=(
 cmake
 git
 jq
 meson
 ninja
 pkgconf
 xorgproto
 hyprwayland-scanner
 hyprutils
 libliftoff
 seatd
 tomlplusplus
 xcb-util-errors
 xorg-xinput
 xorg-xwayland
 hyprcursor
)
provides=("hyprland=mybuild")
conflicts=(hyprland)
options=(!strip !docs !debug)

pkgver() {
    cd hyprland
    cat VERSION

  # cat props.json | jq -r .version
}
prepare() {
    if [ -d hyprland ]; then
	cd hyprland
	git pull origin main
    else 
	git clone --recursive https://github.com/hyprwm/hyprland
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

}

package() {

  cd $srcdir/hyprland
  meson install -C build \
  --destdir "$pkgdir" \
  --strip
  # --skip-subprojects hyprland-protocols\
  # make installheaders PREFIX="$pkgdir/usr"
  rm "$pkgdir/usr/share/hypr/wall0.png"
  rm "$pkgdir/usr/share/hypr/wall1.png"
  cp ../../.wall.png $pkgdir/usr/share/hypr/wall0.png
  cp ../../.wall.png $pkgdir/usr/share/hypr/wall1.png
  cp ../../.ori.png "$pkgdir/usr/share/hypr/wall2.png"
  chmod 777 "$pkgdir/usr/share/hypr/wall2.png"
}
