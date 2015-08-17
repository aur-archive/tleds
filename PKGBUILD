# Submitter: Tom Wizetek <tom@wizetek.com>
# Maintainer: Krischan <neutrino@project-insanity.org>
pkgname=tleds
pkgver=1.05beta10
_pkgverdir=1.05b
pkgrel=4
pkgdesc="Blinks keyboard LEDs indicating incoming and outgoing network packets"
arch=('any')
url="http://web.archive.org/web/20120314130140/http://users.tkk.fi/~jlohikos/tleds/"
license=('GPL')
#source=(http://users.tkk.fi/~jlohikos/tleds/public/${pkgname}-${pkgver}.tgz)
source=(http://archive.debian.org/debian/pool/main/t/tleds/${pkgname}_${pkgver}.orig.tar.gz)
depends=('glibc')
md5sums=('9372325d0383b7ea38e463dae1f1de78')

build() {
  cd "${srcdir}/${pkgname}-${_pkgverdir}"
  make GCCOPTS="-DKERNEL2_1 -D_GNU_SOURCE -O3 -Wall" tleds
}

package() {
  cd "${srcdir}/${pkgname}-${_pkgverdir}"
  install -D -m 755 tleds ${pkgdir}/usr/bin/tleds
  install -D -m 644 tleds.1 ${pkgdir}/usr/man/man1/tleds.1
  install -D -m 644 README ${pkgdir}/usr/share/doc/tleds/README
  install -D -m 644 Changes ${pkgdir}/usr/share/doc/tleds/Changes

# Contributed by xpixelz
  cat > tleds.service << EoF
[Unit]
Description=Blinks keyboard LEDs indicating incoming and outgoing network packets
After=network.target

[Service]
Type=forking
PIDFile=/run/tleds.pid
ExecStart=/usr/bin/tleds -q -c -d 25 eth0
Restart=on-abort

[Install]
WantedBy=multi-user.target
EoF
  install -D -m 644 "tleds.service" "$pkgdir/usr/lib/systemd/system/tleds.service"

  msg ""
  msg "Start manually as root or add to /etc/rc.local"
  msg "   /usr/bin/tleds -q -c -d 25 eth0"
  msg ""
  msg "...or start/enable the systemd service:"
  msg "   systemctl start tleds.service"
  msg "   systemctl enable tleds.service"
  msg ""
  msg "Note that the 'xtleds' binary, which depends on the X libraries,"
  msg "is not included in this package."
  msg ""
}
