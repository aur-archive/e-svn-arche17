# Maintainer: ultraviolet <ultravioletnanokitty@gmail.com>
# Contributor: Ronald van Haren <ronald.archlinux.org>

pkgname=e-svn-arche17
pkgver=latest
pkgrel=1
pkgdesc="Enlightenment window manager DR17 (aka e17), based on the arche17 project's PKGBUILD"
arch=('i686' 'x86_64')
groups=('e17-svn-arche17')
url="http://www.enlightenment.org"
license=('BSD')
depends=('e_dbus-svn' 'edje-svn' 'efreet-svn' 'alsa-lib' 'pm-utils' 
	'pam' 'eeze-svn')  
makedepends=('subversion')
conflicts=('e' 'e-svn')
provides=('e' 'e-svn')
backup=('etc/enlightenment/sysactions.conf')
options=('!libtool')
source=('e-applications.menu')
sha1sums=('e08cc63cb8a188a06705b42d03e032b9fcfa7ee5')

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/e"
_svnmod="e"

build() {
   cd $srcdir

msg "Connecting to $_svntrunk SVN server...."
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up)
  else
    svn co $_svntrunk --config-dir ./ $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  cp -r $_svnmod $_svnmod-build
  cd $_svnmod-build
  
  ./autogen.sh --prefix=/usr --sysconfdir=/etc --enable-pam
  make
}
         
package() {
  cd $srcdir/$_svnmod-build

  make DESTDIR=$pkgdir install

# install license files
  if [ -e $srcdir/$_svnmod-build/COPYING ]; then
    install -Dm644 $srcdir/$_svnmod-build/COPYING \
        $pkgdir/usr/share/licenses/$pkgname/COPYING
  fi

  if [ -e $srcdir/$_svnmod-build/COPYING-PLAIN ]; then
    install -Dm644 $srcdir/$_svnmod-build/COPYING-PLAIN \
        $pkgdir/usr/share/licenses/$pkgname/COPYING-PLAIN
  fi

  # install a default applications.menu file (mostly copy from gnome-menus)
  install -Dm644 $srcdir/e-applications.menu \
	$pkgdir/etc/xdg/menus/e-applications.menu

  rm -r $srcdir/$_svnmod-build
}


