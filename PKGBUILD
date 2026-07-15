# Maintainer: James Read <contact@jread.com>
pkgname=olivetin
pkgver=3000.17.0
pkgrel=1
pkgdesc="Give safe and simple access to predefined shell commands from a web interface."
arch=('x86_64')
provides=("olivetin")
conflicts=("olivetin-bin")
url="https://www.olivetin.app"
license=('AGPL-3.0-only')
backup=('etc/OliveTin/config.yaml')
makedepends=(
	'go'
	'git'
)
source=("$pkgname-$pkgver::git+https://github.com/OliveTin/OliveTin.git#tag=${pkgver}"
	"https://github.com/OliveTin/OliveTin/releases/download/${pkgver}/OliveTin-linux-amd64.tar.gz"
	"systemd-unit-usr-bin.patch")
noextract=()
sha256sums=('SKIP'
	'eebf32945ce8446dfbf7497c5befdee84f5144ae39dbb4680fcb8eec9ec2500a'
	'3f5759da171ae5221402b8a2e20101b89ef778e3c269aad1bb7007d18a1ce653')
validpgpkeys=()

prepare() {
	cd "$srcdir/${pkgname}-${pkgver}"
	patch -p1 < "$srcdir/systemd-unit-usr-bin.patch"
}

build() {
	cd "$srcdir/${pkgname}-${pkgver}/service"

	export CGO_ENABLED=0
	go build \
		-trimpath \
		-ldflags "-s -w -X main.version=${pkgver}" \
		-o ../OliveTin \
		.
}

check() {
	cd "$srcdir/${pkgname}-${pkgver}"
}

package() {
	cd "$srcdir/${pkgname}-${pkgver}"

	install -Dm755 OliveTin "$pkgdir/usr/bin/OliveTin"

	install -Dm644 config.yaml "$pkgdir/etc/OliveTin/config.yaml"
	install -Dm644 var/systemd/OliveTin.service "$pkgdir/usr/lib/systemd/system/OliveTin.service"
	install -Dm644 var/manpage/OliveTin.1.gz "$pkgdir/usr/share/man/man1/OliveTin.1.gz"

	# Prebuilt webui from the matching release archive
	mkdir -p "$pkgdir/var/www/olivetin"
	cp -a "$srcdir/OliveTin-linux-amd64/webui/." "$pkgdir/var/www/olivetin/"

	if [[ -d var/entities ]]; then
		mkdir -p "$pkgdir/etc/OliveTin/entities"
		cp -a var/entities/. "$pkgdir/etc/OliveTin/entities/"
	fi
}
