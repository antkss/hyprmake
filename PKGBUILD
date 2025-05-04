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
  libxcursor
  libxfixes
  libxkbcommon
  libxrender
  mesa
  opengl-driver
  pango
  pixman
  polkit
  re2
  seatd
  systemd-libs
  tomlplusplus
  util-linux-libs
  wayland
  wayland-protocols
  xcb-proto
  xcb-util
  xcb-util-errors
  xcb-util-image
  xcb-util-keysyms
  xcb-util-renderutil
  xcb-util-wm
  xorg-xwayland

)
# depends+=(libdisplay-info.so)
makedepends=(
  cmake
  git
  # hyprland-protocols
  #patch
  #pkgconf
  xorgproto
)
# provides=("hyprland=mybuild")
conflicts=(hyprland hyprwayland-scanner hyprutils hyprpicker hyprland-qtutils hyprlang hyprgraphics aquamarine hyprcursor )
options=(!strip !docs !debug)

pkgver() {
	cd $srcdir/hyprland
    cat VERSION
  # cat props.json | jq -r .version
}
prepare() {
	cd $srcdir
	if [ -d hyprland ]; then
		echo "skipping hyprland"
	else
		git clone --recursive https://github.com/hyprwm/hyprland
		cd hyprland
		git checkout 788ae588979c2a1ff8a660f16e3c502ef5796755
	fi
	cd $srcdir
	echo "cloning all stuff"
	pack=(aquamarine  hyprgraphics  hyprland-qtutils  hyprpicker  hyprwayland-scanner hyprcursor  hyprlang hyprutils)
	for i in ${pack[@]}; do
	   if [ -d $i ]; then
			echo "skipping $i"
		else
			echo "cloning $i"
			git clone --recursive https://github.com/hyprwm/$i
	   fi
   done


}

build() {

  cd $srcdir/hyprland
	 meson setup build \
	   --prefix     /usr \
	   --libexecdir lib \
	   --sbindir    bin \
	   --buildtype  release \
	   --wrap-mode  nodownload \
	   --optimization  3 		\
	   -D          b_lto=true \
	   -D          b_pie=false \
		-D          debug=false \
		-D          default_library=static \
		-D          xwayland=enabled \
		-D          systemd=enabled 

	cd $srcdir/aquamarine
	  cmake --install-prefix $pkgdir/usr .
	  make 
	cd $srcdir/hyprcursor
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr -S . -B ./build
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`
	cd $srcdir/hyprlang
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr -S . -B ./build
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`
	cd $srcdir/hyprgraphics
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr -S . -B ./build || true
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`
	cd $srcdir/hyprutils
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr -S . -B ./build || true
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`
	cd $srcdir/hyprpicker
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=$pkgdir/usr -S . -B ./build  || true
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`
	cd $srcdir/hyprwayland-scanner
	cmake --no-warn-unused-cli -DCMAKE_BUILD_TYPE:STRING=Release -DCMAKE_INSTALL_PREFIX:PATH=/usr -S . -B ./build  || true
	cmake --build ./build --config Release --target all -j`nproc 2>/dev/null || getconf _NPROCESSORS_CONF`

	  # cp -r $pkgdir/../tmp/* $pkgdir/usr

}

package() {


  cd $srcdir/aquamarine
  make install
  cd $srcdir/hyprcursor
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprlang
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprgraphics
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprutils
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprpicker
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprwayland-scanner
  cmake --install build --prefix $pkgdir/usr
  cd $srcdir/hyprland
  meson install -C build \
  --destdir "$pkgdir" \
  --strip
  # --skip-subprojects hyprland-protocols\
  # make installheaders PREFIX="$pkgdir/usr"
  cd $srcdir/..
  rm "$pkgdir/usr/share/hypr/wall0.png"
  rm "$pkgdir/usr/share/hypr/wall1.png"
  cp ./.wall.png $pkgdir/usr/share/hypr/wall0.png
  cp ./.wall.png $pkgdir/usr/share/hypr/wall1.png
  cp ./.ori.png "$pkgdir/usr/share/hypr/wall2.png"
  chmod 777 "$pkgdir/usr/share/hypr/wall2.png"
}
