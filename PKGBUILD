# $Id: PKGBUILD 218251 2014-07-28 16:10:39Z fyan $
# Maintainer: G. Amanakis 

pkgname=iproute2-connmark
pkgver=3.19.0
pkgrel=1
pkgdesc="IP Routing Utilities. Patches: correction of the TC library path so that m_xt.so works (iproute2-fhs.patch), support for act_connmark action (210-add-act_connmark.c taken from OpenWRT)."
arch=('i686' 'x86_64')
license=('GPL2')
url="http://www.linuxfoundation.org/collaborate/workgroups/networking/iproute2"
depends=('glibc' 'iptables' 'act_connmark')
makedepends=('linux-atm')
optdepends=('linux-atm: ATM support')
groups=('base')
provides=('iproute' 'iproute2')
conflicts=('iproute' 'iproute2')
replaces=('iproute' 'iproute2')
options=('staticlibs' '!makeflags')
backup=('etc/iproute2/ematch_map' 'etc/iproute2/rt_dsfield' 'etc/iproute2/rt_protos' \
	'etc/iproute2/rt_realms' 'etc/iproute2/rt_scopes' 'etc/iproute2/rt_tables')
source=(http://www.kernel.org/pub/linux/utils/net/iproute2/iproute2-$pkgver.tar.xz
        iproute2-fhs.patch
	unwanted-link-help.patch
	210-add-act_connmark.patch)
sha1sums=('109b076dc12d4af7c7b5299b0a66e0c4bd8e5e60'
          'b044ed081c645f59bd411c4102d77fe2af136d0d'
          '3b1335f4025f657f388fbf4e5a740871e3129c2a'
          '08fb6512c763e459a70befa24175f34408aa1ae5')

prepare() {
  mv $srcdir/iproute2-$pkgver $srcdir/$pkgname-$pkgver
  cd $srcdir/$pkgname-$pkgver

  # set correct fhs structure
  patch -Np1 -i "$srcdir/iproute2-fhs.patch"

  # allow operations on links called "h", "he", "hel", "help"
  patch -Np1 -i "$srcdir/unwanted-link-help.patch"

  # Taken from OpenWRT
  # https://dev.openwrt.org/browser/trunk/package/network/utils/iproute2/patches/210-add-act_connmark.patch?rev=41227
  patch -Np1 -i "$srcdir/210-add-act_connmark.patch"

  # do not treat warnings as errors
  sed -i 's/-Werror//' Makefile
}

build() {
  cd "$srcdir/$pkgname-$pkgver"

  ./configure
  make
}

package() {
  cd "$srcdir/$pkgname-$pkgver"

  make DESTDIR="$pkgdir" install

  # libnetlink isn't installed, install it FS#19385
  install -Dm644 include/libnetlink.h "$pkgdir/usr/include/libnetlink.h"
  install -Dm644 lib/libnetlink.a "$pkgdir/usr/lib/libnetlink.a"

  # usrmove
  cd "$pkgdir"
  mv usr/sbin usr/bin
}
