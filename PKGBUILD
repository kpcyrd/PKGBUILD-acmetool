# Maintainer: Mikael Eriksson <mikael_eriksson@miffe.org>
# Contributor: kpcyrd <git@rxv.cc>

pkgname=acmetool
pkgver=0.0.58
pkgrel=1

pkgdesc="Acmetool is an easy-to-use command line tool for automatically acquiring certificates from ACME servers (such as Let's Encrypt)"
url="https://github.com/hlandau/acme"
arch=(i686 x86_64 armv7h)
license=('MIT')

depends=(glibc)
makedepends=(go)

source=(acmetool-$pkgver.tar.gz::https://github.com/hlandau/acme/archive/v$pkgver.tar.gz
        acmetool.service
        acmetool.timer
        acmetool.tmpfile)
sha512sums=('c8cb7e721800947e6efb14f00a41309e02714b46c8735a8d09815aca6f9be8b42f032ac9cdafb75f9d9f41119c007fb91b5c34341b2649f697fb7c486b9c51aa'
            '4cf37f14ab92efd159255cc4c3475646017cd4f015a3830d3e1e123d50e19667d0409600635fa5b78d31f1e13d8dacf8e1ebe3df25572f7cbbd26af58db45880'
            '60dc78a7404101606f6a3e1107c01ca84ab46e391b3085f98695ecdfe01470d38efabcda4c3b0e631d3ff82741c8c565f7f29cec3d40cad9c1eae090efeddfb9'
            'fe35fd770bc6798383bd76c03a7e036e2a09962bc3eca030b2130775f1b694876489d0adeeb4080fdab1821b0f15a3116a005ba56e3392ae5454fb9a43a7bfea')

prepare() {
    cd acme-$pkgver

    mkdir -p .gopath/src/github.com/hlandau
    ln -sf "$PWD" .gopath/src/github.com/hlandau/acmetool
}

build() {
    cd acme-$pkgver/cmd/acmetool
    # defunkt: https://github.com/hlandau/acme/issues/234
    # workaround: git config --global http.https://gopkg.in.followRedirects true
    GOPATH="$PWD/../../.gopath" go get -v -d
    GOPATH="$PWD/../../.gopath" go build -v -ldflags "-X github.com/hlandau/acme/hooks.DefaultPath=/etc/acme/hooks"
}

package() {
    cd acme-$pkgver

    install -Dm 755 cmd/acmetool/acmetool "$pkgdir/usr/bin/acmetool"
    install -Dm 644 -t "$pkgdir/usr/lib/systemd/system" \
                        ../acmetool.service \
                        ../acmetool.timer
    install -Dm 644 -t "$pkgdir/usr/lib/tmpfiles.d" ../acmetool.tmpfile
    #install -Dm 644 -t "$pkgdir/usr/share/licenses/$pkgname" LICENSE
    install -Dm 644 -t "$pkgdir/usr/share/doc/$pkgname" README.md _doc/*
}
