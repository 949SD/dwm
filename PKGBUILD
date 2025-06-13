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
options=('!debug')
makedepends=('cmake' 'unzip')
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
    echo 6.5."$(date -d "$___DATE" +'%Y%m%d')_$___COMMIT_COUNT"
    unset ___DATE
    unset ___DATE_TIME
    unset ___COMMIT_COUNT
}

build() {
    cd "$pkgname"
    . ./deps.sh
    mkdir -p ${VENDORDIR}
    curl -L ${NB_LINK} -o ${VENDORDIR}/noticeboard.zip
    ( cd ${VENDORDIR} && unzip noticeboard.zip )
    mv ${VENDORDIR}/noticeboard-* ${VENDORDIR}/noticeboard
    CFLAGS= CXXFLAGS= cmake -S ${VENDORDIR}/noticeboard --preset make -B ${VENDORDIR}/noticeboard/build -DCMAKE_BUILD_TYPE=Release
    CFLAGS= CXXFLAGS= cmake --build ${VENDORDIR}/noticeboard/build -- -j
    mkdir -p ${VENDORDIR}/noticeboard/out
    cmake --install ${VENDORDIR}/noticeboard/build --prefix ${VENDORDIR}/noticeboard/out
    DESTDIR="$pkgdir/" PREFIX="/usr" MODE=RELEASE ICONDIR=$PREFIX/share/pixmap NOVENDOR=true make -j
}

package() {
    cd "$pkgname"
    DESTDIR="$pkgdir/" PREFIX="/usr" MODE=RELEASE ICONDIR=$PREFIX/share/pixmap NOVENDOR=1 make install
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
