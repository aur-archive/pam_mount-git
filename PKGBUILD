# Maintainer: Enric Morales <aur@enric.me>

pkgname=pam_mount-git
pkgver=20130606
pkgrel=1
pkgdesc="A PAM module that can mount volumes for a user session - Git Version"
arch=('i686' 'x86_64')
url="http://pam-mount.sourceforge.net/"
license=('GPL')
depends=('util-linux' 'libhx>=3.12.1' 'libxml2>=2.6' 'openssl>0.9.7' 'cryptsetup>=1.1.2')
makedepends=('git')
backup=('etc/security/pam_mount.conf.xml')
conflicts=('pam_mount')
provides=('pam_mount')
options=(!emptydirs !libtool)

_gitroot=git://pam-mount.git.sf.net/gitroot/pam-mount/pam-mount
_gitname=pam_mount

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [[ -d "$_gitname" ]]; then
    cd "$_gitname" && git pull origin
    msg "The local files are updated."
  else
    git clone "$_gitroot" "$_gitname"
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting build.."

  cd ${srcdir}/"$_gitname"
  patch -p1 <$srcdir/$pkgname-git.patch
  aclocal
  libtoolize
  automake --add-missing
  autoreconf
  ./configure \
	--prefix=/usr \
	--with-ssbindir=/usr/bin \
	--sbindir=/usr/bin \
	--with-slibdir=/usr/lib \
	--sysconfdir=/etc \
	--localstatedir=/var
  make
}

package() {
  cd "$_gitname"
  make DESTDIR="$pkgdir" install
}

