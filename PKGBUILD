# Maintainer: Petros Angelatos <petrosagg@resin.io>

buildarch=18

pkgname='lxc-docker'
pkgver=0.6.4
pkgrel=2
pkgdesc="Docker - the Linux container runtime"
arch=('arm armv6h')
url="https://github.com/dotcloud/docker"
license=('Apache License 2.0')
makedepends=('go>=1.1.2')
depends=('lxc' 'aufs3-util')
provides=('lxc-docker')
options=('!strip')
source=("${pkgname}-${pkgver}.tar.gz::https://codeload.github.com/dotcloud/docker/tar.gz/v${pkgver}"
        'docker-arm-v0.6.4.patch')

md5sums=('c70bb4f56ef1fee069964a5f8c7c26f9'
         '06f0e60138fb808f9c3f76290b2fcb90')

prepare() {
	mkdir -p "${srcdir}/go/src/github.com/dotcloud/"
	mv "${srcdir}/docker-${pkgver}" "${srcdir}/go/src/github.com/dotcloud/docker"
	cd "${srcdir}/go/src/github.com/dotcloud/docker"
	patch -p1 <"${srcdir}/docker-arm-v0.6.4.patch"
}

build() {
	cd "${srcdir}/go/src/github.com/dotcloud/docker"
	GOPATH="${srcdir}/go:${srcdir}/go/src/github.com/dotcloud/docker/vendor"
	LDFLAGS="-X main.VERSION $pkgver -d -w"
	GOARCH=arm go build -v -o "${srcdir}/docker" -ldflags "$LDFLAGS" ./docker
}

package() {
	install -D -m 755 "$srcdir/docker" "$pkgdir/usr/bin/docker"
}
