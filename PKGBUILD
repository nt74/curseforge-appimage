# Maintainer: Nikos Toutountzoglou <nikos.toutou@gmail.com>
pkgname=curseforge-appimage
pkgver=0.218.1
pkgrel=2
pkgdesc="Download and manage your addons, CC and mods with the CurseForge app"
arch=('x86_64')
url="https://download.curseforge.com/"
license=('MIT')
depends=('fuse2')
provides=("curseforge-appimage=$pkgver")
conflicts=('curseforge-appimage' 'curseforge')
source=("$pkgname-$pkgver.zip::https://curseforge.overwolf.com/downloads/curseforge-latest-linux.zip")
sha256sums=('fe442d92ba3e2342c9ef945dd5f331c0e647e0c2b131fe6900e11c6ad2a731e1')
options=(!strip) # necessary otherwise the AppImage file in the package is truncated
_image="CurseForge.AppImage"

prepare() {
	cd "$srcdir"
	mv CurseForge-${pkgver}*.AppImage "$_image"
	chmod +x "$_image"
	./"$_image" --appimage-extract
	sed -i -e "s/AppRun/\/usr\/bin\/curseforge/" "$srcdir/squashfs-root/curseforge.desktop"
  cat > curseforge.sh <<EOF
#!/bin/sh
/opt/curseforge/curseforge.AppImage "\$@"
EOF
}

package() {
	install -Dm755 "$srcdir/$_image" "$pkgdir/opt/curseforge/curseforge.AppImage"
	install -Dm755 "$srcdir/curseforge.sh" "$pkgdir/usr/bin/curseforge"
	install -dm755 "$pkgdir/usr/share/"
	cp -r --no-preserve=mode,ownership "$srcdir/squashfs-root/usr/share/icons" "$pkgdir/usr/share/"
	install -Dm644 "$srcdir/squashfs-root/curseforge.desktop" "$pkgdir/usr/share/applications/curseforge.desktop"
}
