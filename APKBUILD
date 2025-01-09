# Contributor: Clayton Craft <clayton@craftyguy.net>
# Contributor: Hugo Osvaldo Barrera <hugo@whynothugo.nl>
# Maintainer: Struan Robertson <contact@struanrobertson.co.uk>
pkgname=systemd-efistub
pkgver=257
pkgrel=0
pkgdesc="systemd's EFI boot stub and ukify"
url="https://systemd.io/"
# riscv64: I have no way to test this currently
arch="x86_64 x86 aarch64 armv7"
license="LGPL-2.1-or-later"
makedepends="
	bash
	coreutils
	gperf
	libcap-dev
	meson
	py3-elftools
	py3-jinja2
	util-linux-dev
"
source="
	systemd-$pkgver.tar.gz::https://github.com/systemd/systemd/archive/refs/tags/v$pkgver.tar.gz
	0001-patch-wchar_t-for-musl.patch
	"
options="!check"  # no tests
subpackages="ukify:ukify:noarch"
builddir="$srcdir/systemd-$pkgver"

build() {
	abuild-meson  \
		-Dsbat-distro="alpine" \
		-Dsbat-distro-summary="Alpine Linux" \
		-Dsbat-distro-pkgname="$pkgname" \
		-Dsbat-distro-version="$pkgver" \
		-Dsbat-distro-url="https://alpinelinux.org/" \
		-Dadm-group=false \
		-Danalyze=false \
		-Dbacklight=false \
		-Dbinfmt=false \
		-Dbootloader=enabled \
		-Dcompat-mutable-uid-boundaries=false \
		-Dcoredump=false \
		-Ddns-over-tls=false \
		-Defi=true \
		-Denvironment-d=false \
		-Dfexecve=false \
		-Dfirstboot=false \
		-Dfirst-boot-full-preset=false \
		-Dgshadow=false \
		-Dhibernate=false \
		-Dhomed=disabled \
		-Dhostnamed=false \
		-Dhwdb=false \
		-Didn=false \
		-Dima=false \
		-Dimportd=disabled \
		-Dinitrd=false \
		-Dkernel-install=false \
		-Dldconfig=false \
		-Dlocaled=false \
		-Dlogind=false \
		-Dmachined=false \
		-Dnetworkd=false \
		-Dnscd=false \
		-Dnss-myhostname=false \
		-Dnss-mymachines=disabled \
		-Dnss-resolve=disabled \
		-Dnss-systemd=false \
		-Doomd=false \
		-Dpolkit=disabled \
		-Dportabled=false \
		-Dpstore=false \
		-Dquotacheck=false \
		-Drandomseed=false \
		-Dremote=disabled \
		-Drepart=disabled \
		-Dresolve=false \
		-Drfkill=false \
		-Dsmack=false \
		-Dstoragetm=false \
		-Dsysext=false \
		-Dsysupdate=disabled \
		-Dsysusers=false \
		-Dtimedated=false \
		-Dtimesyncd=false \
		-Dtmpfiles=false \
		-Dtpm=false \
		-Dukify=disabled \
		-Durlify=false \
		-Duserdb=false \
		-Dutmp=false \
		-Dvconsole=false \
		-Dvmspawn=disabled \
		-Dwheel-group=false \
		-Dxdg-autostart=false \
		. output
	meson compile -C output systemd-boot
}

package() {
	mkdir -p "$pkgdir"/usr/lib/systemd/boot/efi

	find "$builddir/output/src/boot/" -name '*.stub' -exec \
		install -Dm 644 {} -t "$pkgdir"/usr/lib/systemd/boot/efi \;
}

ukify() {
	depends="binutils py3-pefile"

	install -Dm755 "$builddir"/src/ukify/ukify.py \
		"$subpkgdir"/usr/bin/ukify
}

sha512sums="
b8cd23ed1a5dff1894f33a831413f9805b2b7bafe93046f163aa4c1c8b929365785d0c04a4c758823624a7536d2a47c8fafae659dd41d4440ddace3d88bb1ff7  systemd-257.tar.gz
81e6f311d567ef8b1f8957f25f019e7fa995f640659381757d1abdd7295434810b79a09367d274c5755f1ed1f6d37d7d98e9c93645578daca34acdeca0ba3965  0001-patch-wchar_t-for-musl.patch
"
