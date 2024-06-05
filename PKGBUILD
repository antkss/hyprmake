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
# source=(
#   "git+https://github.com/hyprwm/Hyprland.git"
#   "git+https://gitlab.freedesktop.org/wlroots/wlroots.git"
#   "git+https://github.com/hyprwm/hyprland-protocols.git"
#   "git+https://github.com/canihavesomecoffee/udis86.git"
#   "git+https://github.com/wolfpld/tracy.git")
# b2sums=(
#   'SKIP'
#   'SKIP'
#   'SKIP'
#   'SKIP'
#   'SKIP'
# )
#
pkgver() {
 if [ -d hyprland-npi ]; then
   cd hyprland-npi
 elif [ -d hyprland-source ]; then
   cd hyprland-source
 elif [ -d hyprland ]; then
   cd hyprland
 else
   echo "tried all !!!! errors"
 fi
  cat props.json | jq -r .version
}
prepare() {
  # cd hyprland
  echo "lmao"
  # make all
  # git submodule init
  # git config submodule.wlroots.url "$srcdir/wlroots"
  # git config submodule.subprojects/hyprland-protocols.url "$srcdir/hyprland-protocols"
  # git config submodule.subprojects/udis86.url "$srcdir/udis86"
  # git config submodule.subprojects/tracy.url "$srcdir/tracy"
  # git -c protocol.file.allow=always submodule update
  #
  # if [[ -z "$(git config --get user.name)" ]]; then
  #   git config user.name local && git config user.email '<>' && git config commit.gpgsign false
  # fi
  # # Pick pull requests from github using `pick_mr <pull request number>`.
  #
  # git -C subprojects/wlroots reset --hard
  # patch -d subprojects/wlroots -Np1 < subprojects/packagefiles/wlroots-meson-build.patch
}
#
# pkgver() {
#   git -C Hyprland describe --long --tags | sed 's/^v//;s/\([^-]*-\)g/r\1/;s/-/./g'
# }
#
build() {
  if [ -d hyprland ]; then
    cd hyprland
    git pull 
  fi
  make release
}

package() {

 if [ -d hyprland-npi ]; then
   cd hyprland-npi
 elif [ -d hyprland-source ]; then
   cd hyprland-source
 elif [ -d hyprland ]; then
   cd hyprland
 else
   echo "tried all !!!! errors"
 fi
# if [ -z $(grep "cmake --install ./build --prefix \$\(PREFIX\)" Makefile) ]; then
#   sed -i 's/cmake --install .\/build/cmake --install .\/build --prefix $(PREFIX)/g' Makefile
# else 
#   echo "file is already patched"
# fi

make install PREFIX="$pkgdir/usr"
make installheaders PREFIX="$pkgdir/usr"
# strip -v $pkgdir/usr/bin/Hyprland
# strip -v $pkgdir/usr/bin/hyprpm
# strip -v $pkgdir/usr/bin/hyprctl
# strip -v $pkgdir/usr/lib/libwlroots.so.13032
cp /home/as/.images/ori.png "$pkgdir/usr/share/hyprland/wall2.png"
chmod 777 "$pkgdir/usr/share/hyprland/wall2.png"
cd $pkgdir

  # meson install -C build \
  #   --destdir "$pkgdir" \
  #
  # # rm -rf "$pkgdir/usr/include/hyprland/wlroots/wlr"
  # # ln -sf . "$pkgdir/usr/include/hyprlandwlroots/wlr"
  # # resolve conflicts with system wlr
  # rm -f "$pkgdir/usr/lib/libwlroots.so"
  # rm -rf "$pkgdir/usr/lib/pkgconfig"
  # # FIXME: remove after xdg-desktop-portal-hyprland disowns hyprland-portals.conf
  # rm -rf "$pkgdir/usr/share/xdg-desktop-portal"
  #
  # # license
  # install -Dm0644 -t "$pkgdir/usr/share/licenses/${pkgname}" LICENSE
}
# vi: et ts=2 sw=2
