# Maintainer: ZekeSulastin <zekesulastin@gmail.com>

# This is the 3.6.14 Release Candidate 6
# http://www.hard-light.net/forums/index.php?topic=80754.0
# Things may or may not work properly; don't delete your
# 	regular fs2_open install just yet ;)

# This PKGBUILD only generates the engine binary.
# The retail Freespace 2 data is required to play the game.
# See http://www.hard-light.net/forums/index.php?topic=79545.0
# or http://www.hard-light.net/wiki/index.php/Fs2_open_on_Linux/Extracting_data_from_CD
# This data should be present and writable in ~/.fs2_open

pkgname=fs2_open_3614
pkgver=RC7
pkgrel=1
pkgdesc="An enhancement of the Freespace 2 engine - 3.6.14 release candidate"
url="http://scp.indiegames.us"
arch=('i686' 'x86_64')
license=('custom:freespace2')
depends=('libjpeg' 'libpng' 'libtheora' 'libvorbis' 'lua' 'mesa' 'openal' 'sdl')
install=fs2_open_3614.install
source=(http://scp.indiegames.us/builds/fs2_open_3_6_14_${pkgver}_src.tgz
		'osapi_unix.patch'
		http://scp.indiegames.us/builds/Shaders-3.6.14_Jan-23.zip)
md5sums=('95c99013d1773cfe48852d43cb67b237'
		'783d5ab68a0ce4d26ee415e8fefbc762'
		'31b92c6a89fdf9fb490fca970e349932')

build()
{
	cd "$srcdir/fs2_open_3_6_14_${pkgver}"

	# Changes default video settings for better mod compatability
	patch -Np0 -i "$srcdir/osapi_unix.patch"

	./autogen.sh --enable-speech --enable-inferno
	make
	cp "code/fs2_open_3.6.14" "$srcdir/fs2_open_3_6_14_${pkgver}/"

	# Build additional debug binary, for debugging the RC
	# You do NOT want to run the debug binary unless you need to 
	# 	make a debug log.
	make clean
	./autogen.sh --enable-speech --enable-inferno --enable-debug
	make
	cp "code/fs2_open_3.6.14_DEBUG" "$srcdir/fs2_open_3_6_14_${pkgver}/"

	# If building again from the same src directory, rm -rf /src before
	#   running makepkg again
}

package()
{
	cd "$srcdir/fs2_open_3_6_14_${pkgver}"

	install -D -m644 COPYING "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	install -D -m755 fs2_open_3.6.14 "$pkgdir/usr/bin/fs2_open_3.6.14"
	install -D -m755 fs2_open_3.6.14_DEBUG "$pkgdir/usr/bin/fs2_open_3.6.14_DEBUG"
	
	# Symlink to make the non-debug binary available under the package name
	ln -sf /usr/bin/fs2_open_3.6.14 "$pkgdir/usr/bin/fs2_open_3614"

	# New shaders to copy over at your leisure!
	install -m755 -d "$pkgdir/usr/share/fs2_open/shaders_3614/data/effects"
	install -m755 -d "$pkgdir/usr/share/fs2_open/shaders_3614/data/tables"
	
	install -m644 -D "$srcdir/data/effects/"* "$pkgdir/usr/share/fs2_open/shaders_3614/data/effects"
	install -m644 -D "$srcdir/data/tables/"* "$pkgdir/usr/share/fs2_open/shaders_3614/data/tables"
}
