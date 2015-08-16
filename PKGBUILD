# custom variables
_hkgname=Agda
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-agda
pkgver=2.3.0.1
pkgrel=2
pkgdesc="A dependently typed functional programming language and proof assistant"
url="http://wiki.portal.chalmers.se/agda/"
license=("OtherLicense")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc"
         "haskell-quickcheck"
         "haskell-haskeline"
         "haskell-haskell-src-exts"
         "haskell-mtl"
         "haskell-syb"
         "haskell-xhtml"
         "haskell-zlib"
         "hashable"
         "hashtables"
)
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
md5sums=('3caa2466ae4f925dd37320336e2e839c')

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
    sed -i "s/2.3.0/2.3.0.1/" src/data/emacs-mode/agda2-mode.el
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
