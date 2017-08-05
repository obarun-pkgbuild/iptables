# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://projects.archlinux.org/svntogit/packages.git/tree/trunk?h=packages/iptables
# 						Maintainer: Ronald van Haren <ronald.archlinux.org>
# 						Contributor: Thomas Baechler <thomas@archlinux.org>

pkgname=iptables
pkgver=1.6.1
pkgrel=2
pkgdesc='Linux kernel packet control tool'
arch=('i686' 'x86_64')
license=(GPL2)
url='http://www.netfilter.org/projects/iptables/index.html'
depends=('glibc' 'bash' 'libnftnl' 'libpcap')
makedepends=('linux-api-headers')
optdepends=('iptables-runitserv: iptables runit service'
			'ip6tables-runitserv: ip6tables runit service')
source=(http://www.netfilter.org/projects/iptables/files/${pkgname}-${pkgver}.tar.bz2 \
        empty.rules
        simple_firewall.rules
        empty-filter.rules
        empty-mangle.rules
        empty-nat.rules
        empty-raw.rules
        empty-security.rules
        iptables-flush)
sha1sums=('b2592490ca7a6c2cd0f069e167a4337c86acdf91'
          '83b3363878e3660ce23b2ad325b53cbd6c796ecf'
          'f085a71f467e4d7cb2cf094d9369b0bcc4bab6ec'
          'd9f9f06b46b4187648e860afa0552335aafe3ce4'
          'c45b738b5ec4cfb11611b984c21a83b91a2d58f3'
          '1694d79b3e6e9d9d543f6a6e75fed06066c9a6c6'
          '7db53bb882f62f6c677cc8559cff83d8bae2ef73'
          'ebbd1424a1564fd45f455a81c61ce348f0a14c2e'
          'e7abda09c61142121b6695928d3b71ccd8fdf73a')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal

prepare() {
  cd "${pkgname}-${pkgver}"

  # use system one
  rm include/linux/types.h
}

build() {
  cd $pkgname-$pkgver

  ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --sbindir=/usr/bin \
    --libexecdir=/usr/lib/iptables \
    --with-xtlibdir=/usr/lib/iptables \
    --enable-bpf-compiler \
    --enable-devel \
    --enable-shared

  make
}

package() {
  cd "${pkgname}-${pkgver}"

  make DESTDIR="${pkgdir}" install

  cd "${srcdir}"
  install -D -m644 empty.rules "${pkgdir}"/etc/iptables/empty.rules
  install -D -m644 simple_firewall.rules "${pkgdir}"/etc/iptables/simple_firewall.rules

  install -d "$pkgdir"/var/lib/{iptables,ip6tables}
  install -m644 empty-{filter,mangle,nat,raw,security}.rules "${pkgdir}"/var/lib/iptables
  install -m644 empty-{filter,mangle,nat,raw,security}.rules "${pkgdir}"/var/lib/ip6tables

  install -Dm755 ${srcdir}/iptables-flush ${pkgdir}/usr/lib/iptables/scripts/iptables-flush
}

