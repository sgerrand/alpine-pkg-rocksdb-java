# Contributor: Sasha Gerrand <alpine-pkgs@sgerrand.com>
# Maintainer: Sasha Gerrand <alpine-pkgs@sgerrand.com>
pkgname=rocksdb-java
_pkgname=rocksdbjni
pkgver=5.12.4
pkgrel=0
pkgdesc="A persistent key-value store for fast storage environments"
url="http://rocksdb.org/"
arch="all"
license="BSD"
depends="openjdk8-jre-base"
depends_dev="bzip2-dev lz4-dev snappy-dev zlib-dev zstd-dev"
makedepends="$depends_dev bash openjdk8 perl linux-headers"
install=""
subpackages="$pkgname-dev $pkgname-doc $pkgname-native"
source="$pkgname-$pkgver.tar.gz::https://github.com/facebook/rocksdb/archive/v$pkgver.tar.gz"
builddir="$srcdir/rocksdb-$pkgver"

build() {
	cd "$builddir"

	DEBUG_LEVEL=0 JAVA_HOME="/usr/lib/jvm/default-jvm" \
		PATH="$PATH:/usr/lib/jvm/default-jvm/bin" \
		make rocksdbjava || return 1
}

package() {
	depends="$depends $pkgname-native"

	cd "$builddir"

	install -m644 -D "$builddir"/java/target/$_pkgname-*.jar \
		"$pkgdir"/usr/share/java/$_pkgname-$pkgver.jar || return 1

	for file in COPYING LICENSE.leveldb
	do
    install -Dm644 $file "$pkgdir"/usr/share/licenses/$pkgname/$file || return 1
	done

  mkdir -p "$pkgdir"/usr/share/doc
	cp -a java/target/apidocs "$pkgdir"/usr/share/doc/$pkgname

	for file in README.md HISTORY.md
	do
		install -Dm644 $file "$pkgdir"/usr/share/doc/$pkgname/$file || return 1
	done
}

dev() {
  mkdir "$subpkgdir"

  default_dev
}

doc() {
  default_doc
}

native() {
	local soname="librocksdbjni-linux64.so"

	install -m756 -D "$builddir"/java/target/$soname \
		"$subpkgdir"/usr/lib/$soname.$pkgver || return 3
	ln -sf $soname.$pkgver "$subpkgdir"/usr/lib/$soname
}

sha512sums="661e30a9fd2a83f2d8dbcc9f27e8ae2e83f384bb7f4f61dc41d89e85d93edc1032b70ab97063e0e3c0bda591612214793d1b075c6dcfeda2e0a32acb6e9d8689  rocksdb-java-5.12.4.tar.gz"
