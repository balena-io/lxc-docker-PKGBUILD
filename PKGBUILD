# Maintainer: Petros Angelatos <petrosagg@resin.io>

buildarch=18

pkgname='docker'
pkgver=0.7.5
pkgrel=1
pkgdesc='Pack, ship and run any application as a lightweight container'
arch=('arm armv6h')
url='http://www.docker.io/'
license=('Apache')
depends=('bridge-utils' 'iproute2' 'device-mapper' 'lxc' 'sqlite' 'systemd')
makedepends=('docker' 'patch' 'make')
options=('!strip')
optdepends=(
	'aufs3-util: AuFS backend support'
)
install=$pkgname.install
source=(
	"git+https://github.com/dotcloud/docker.git#tag=v$pkgver"
    "docker-arm-v$pkgver.patch"
    'docker.install'
    'docker.service'
)
md5sums=('SKIP'
         'e89c02ce9bb54d4bfe623607da67e86d'
         '64ebb2c81553442656ef5fea90d6568e'
         '3f7ccab915fb1942f06e18946c2811d2')

prepare() {
	cd "${srcdir}/docker"
	patch -p1 <"${srcdir}/docker-arm-v$pkgver.patch"
	make build
}

build() {
	cd "${srcdir}/docker"
	make binary
}

check() {
	cd "${srcdir}/docker"
	# make test
}

package() {
	cd "${srcdir}/docker"
	install -Dm755 "bundles/$pkgver/binary/docker-$pkgver" "$pkgdir/usr/bin/docker"
	# completion
	install -Dm644 "contrib/completion/bash/docker" "$pkgdir/usr/share/bash-completion/completions/docker"
	install -Dm644 "contrib/completion/zsh/_docker" "$pkgdir/usr/share/zsh/site-functions/_docker"
	# systemd
	install -Dm644 "contrib/init/systemd/docker.service" "$pkgdir/usr/lib/systemd/system/docker.service"
}
