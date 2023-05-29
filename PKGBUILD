# Maintainer: David K david.dk949@gmail.com
_pkgname=dwm
pkgname="${_pkgname}-949sd"
pkgver="unknown"
pkgrel=0
pkgdesc="dynamic window manager"
arch=('x86_64')
url="https://github.com/dk949/$_pkgname"
license=('MIT')
depends=(
    'libx11'
    'libxcb'
    'libxft'
    'fontconfig'
)
optdepends=(
    'libxinerama: for multi-monitor use. only used when installed'
    'alsa-lib: for controling volume. only used when installed'
)
makedepends=()
provides=('dwm')
conflicts=(
    'dwm'
    'dwm-git'
)
source=("$pkgname::git+$url")
md5sums=() #autofill using updpkgsums
sha256sums=('SKIP')

pkgver() {
    ___DATE="$(git -C "$pkgname" log -1 --format='%cd' --date=format:'%F')"
    ___DATE_TIME="$___DATE 00:00"
    ___COMMIT_COUNT=$(git -C "$pkgname" rev-list --count HEAD --since="$___DATE_TIME")
    echo 6.2."$(date -d "$___DATE" +'%Y%m%d')_$___COMMIT_COUNT"
    unset ___DATE
    unset ___DATE_TIME
    unset ___COMMIT_COUNT
}

build() {
    cd "$pkgname"
    make -j
}

package() {
    cd "$pkgname"

    make DESTDIR="$pkgdir/" PREFIX="/usr" install
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
