# Contributor: Thomas Baechler <thomas@archlinux.org>
# Maintainer: Jonathan Schmidt <j.schmidt@archlinux.us>

pkgname=nvidia-173xx-utils-glselect
pkgver=173.14.25
pkgrel=1
pkgdesc="NVIDIA drivers utilities and libraries with libgl compatibility through glselect."
arch=('i686' 'x86_64')
[ "$CARCH" = "i686"   ] && ARCH=x86
[ "$CARCH" = "x86_64" ] && ARCH=x86_64
url="http://www.nvidia.com/"
depends=('xorg-server')
optdepends=('gtk2: nvidia-settings' 'pkgconfig: nvidia-xconfig')
conflicts=('nvidia-173xx-utils')
provides=("nvidia-173xx-utils=$pkgver")
license=('custom')
install=nvidia.install
options=(!strip)
source=("http://download.nvidia.com/XFree86/Linux-${ARCH}/${pkgver}/NVIDIA-Linux-${ARCH}-${pkgver}-pkg0.run" 'glselect')
md5sums=('397bac51f760505ea57e863c1db9c572' '128787899ce0e0c08ae8e129a0663de4')
[ "$CARCH" = "x86_64" ] && md5sums=('a61b6c1627984f93af73eb446a8beb5e' '128787899ce0e0c08ae8e129a0663de4')

build() {
	cd $srcdir
	sh NVIDIA-Linux-${ARCH}-${pkgver}-pkg0.run --extract-only
	cd NVIDIA-Linux-${ARCH}-${pkgver}-pkg0/usr/

	mkdir -p $pkgdir/usr/{lib,bin,share/applications,share/pixmaps,share/man/man1}
	mkdir -p $pkgdir/usr/lib/xorg/modules/{extensions,drivers}
	mkdir -p $pkgdir/usr/share/licenses/nvidia-173xx/
	mkdir -p $pkgdir/usr/include/cuda
	mkdir -p $pkgdir/usr/lib/

	install -m644 include/cuda/cuda*.h $pkgdir/usr/include/cuda

	install lib/{libGLcore,libGL,libnvidia-cfg,libcuda,tls/libnvidia-tls}.so.${pkgver} \
	$pkgdir/usr/lib/ || return 1
	install -m644 share/man/man1/* $pkgdir/usr/share/man/man1/ || return 1
	rm $pkgdir/usr/share/man/man1/nvidia-installer.1.gz || return 1
	install X11R6/lib/libXv* $pkgdir/usr/lib/ || return 1
	install -m644 share/applications/nvidia-settings.desktop $pkgdir/usr/share/applications/ || return 1
	# fix nvidia .desktop file
	sed -e 's:__UTILS_PATH__:/usr/bin:' -e 's:__PIXMAP_PATH__:/usr/share/pixmaps:' -i $pkgdir/usr/share/applications/nvidia-settings.desktop
	install -m644 share/pixmaps/nvidia-settings.png $pkgdir/usr/share/pixmaps/ || return 1
	install X11R6/lib/modules/drivers/nvidia_drv.so $pkgdir/usr/lib/xorg/modules/drivers || return 1
	install X11R6/lib/modules/extensions/libglx.so.$pkgver $pkgdir/usr/lib/xorg/modules/extensions || return 1
	install -m755 bin/nvidia-{settings,xconfig,bug-report.sh} $pkgdir/usr/bin/ || return 1
	cd $pkgdir/usr/lib/
	ln -s libGLcore.so.$pkgver libGLcore.so.1 || return 1
	ln -s libnvidia-cfg.so.$pkgver libnvidia-cfg.so.1 || return 1
	ln -s libnvidia-tls.so.$pkgver libnvidia-tls.so.1 || return 1
	ln -s libcuda.so.$pkgver libcuda.so.1 || return 1
	ln -s libcuda.so.$pkgver libcuda.so || return 1
	ln -s libXvMCNVIDIA.so.$pkgver libXvMCNVIDIA_dynamic.so.1 || return 1

	install -m644 $srcdir/NVIDIA-Linux-${ARCH}-${pkgver}-pkg0/LICENSE $pkgdir/usr/share/licenses/nvidia-173xx/ || return 1
	ln -s nvidia-173xx $startdir/pkg/usr/share/licenses/nvidia-173xx-utils || return 1

	install -D -m644 $srcdir/NVIDIA-Linux-${ARCH}-${pkgver}-pkg0/usr/share/doc/README.txt $pkgdir/usr/share/doc/nvidia-173xx/README || return 1

	find $pkgdir/usr -type d -exec chmod 755 {} \;
	chmod 644 $pkgdir/usr/lib/libXvMCNVIDIA.a

	# Allow gl lib link switching with glselect
	install $srcdir/glselect $pkgdir/usr/bin/ || return 1
}
