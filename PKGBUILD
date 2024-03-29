# Maintainer: Alexander F. Rødseth <xyproto@archlinux.org>
# Contributor: Eli Schwartz <eschwartz@archlinux.org>
# Contributor: Lex Black <autumn-wind@web.de>
# Contributor: Michael Jakl <jakl.michael@gmail.com>
# Contributor: devmotion <nospam-archlinux.org@devmotion.de>
# Contributor: Valentin Churavy <v.churavy@gmail.com>

pkgbase=julia
pkgname=(julia julia-docs)
epoch=2
pkgver=1.2.0.rc1
pkgrel=1
arch=('x86_64')
pkgdesc='High-level, high-performance, dynamic programming language'
url='https://julialang.org/'
license=('MIT')
depends=('cblas' 'fftw' 'hicolor-icon-theme' 'libgit2' 'libunwind' 'libutf8proc'
         'openblas' 'suitesparse')
makedepends=('cmake' 'gcc-fortran' 'gmp' 'python2')
source=("https://github.com/JuliaLang/$pkgbase/releases/download/v${pkgver/.rc/-rc}/$pkgbase-${pkgver/.rc/-rc}-full.tar.gz"{,.asc}
        'https://github.com/JuliaLang/julia/pull/29540/commits/0c442318196389d653ee21eba65d8c4f7beb72a0.patch'
        'Make.user')
sha256sums=('e301421b869c6ecea8c3ae06bfdddf67843d16e694973b4958924914249afa46'
            'SKIP'
            'b7374fcd5a579fc59d6988795fc0c3cf411a89205942c691a5b3003793ae6c52'
            '9381af45e329f874241eec5a5d85e70a7945433ab9ee82215e28a6085783df88')
# Julia (Binary signing key) <buildbot@julialang.org>
validpgpkeys=('3673DF529D9049477F76B37566E3C7DC03D6E495')

prepare() {
  cd $pkgbase-${pkgver/.rc/-rc}

  # add and use option to build with system cblas
  patch -p1 --no-backup-if-mismatch -i ../0c442318196389d653ee21eba65d8c4f7beb72a0.patch

  msg2 'Configuring the build...'
  cp -f ../Make.user Make.user
}

build() {
  env CFLAGS="$CFLAGS -w" CXXFLAGS="$CXXFLAGS -w" make -C "$pkgbase-${pkgver/.rc/-rc}"
}

#check() {
# cd "$pkgbase-${pkgver/.rc/-rc}/test"
#
 # this is the make testall target, plus the --skip option from
 # travis/appveyor/circleci (one test fails with DNS resolution errors)
# ../julia --check-bounds=yes --startup-file=no ./runtests.jl all --skip Sockets
# find ../stdlib \( -name \*.cov -o -name \*.mem \) -delete
# rm -r depot/compiled
#}

package_julia() {
  backup=('etc/julia/startup.jl')
  optdepends=('gnuplot: If using the Gaston Package from julia')

  make -C "$pkgbase-${pkgver/.rc/-rc}" DESTDIR="$pkgdir" install

  # Documentation is in the julia-docs package.
  # Man pages in /usr/share/julia/doc/man are duplicate.
  rm -rf "$pkgdir/usr/share/"{doc,julia/doc}

  install -Dm644 "$pkgbase-${pkgver/.rc/-rc}/LICENSE.md" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.md"
  rm $pkgdir/usr/share/icons/hicolor/icon-theme.cache
}

package_julia-docs() {
  pkgdesc='Documentation and examples for Julia'
  depends=('julia')

  install -d "$pkgdir/usr/share/doc"
  cp -r "$pkgbase-${pkgver/.rc/-rc}/doc" "$pkgdir/usr/share/doc/$pkgbase"
  rm -rf "$pkgdir/usr/share/doc/julia/man"
  install -Dm644 "$pkgbase-${pkgver/.rc/-rc}/LICENSE.md" \
    "$pkgdir/usr/share/licenses/$pkgname/LICENSE.md"
}

# getver: julialang.org/downloads
# vim: ts=2 sw=2 et:
