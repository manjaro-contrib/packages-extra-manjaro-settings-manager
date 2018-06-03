# Maintainer : Ramon Buldo <ramon@manjaro.org>

pkgbase=manjaro-settings-manager
pkgname=('manjaro-settings-manager' 'manjaro-settings-manager-kcm' 
         'manjaro-settings-manager-notifier' 'manjaro-settings-manager-knotifier')
pkgver=0.5.4
pkgrel=14
pkgdesc="Manjaro Linux System Settings Tool"
arch=('i686' 'x86_64')
url="https://gitlab.manjaro.org/applications/manjaro-settings-manager"
license=("GPL")
depends=('icu>=61.1' 'qt5-base>=5.9.1' 'hwinfo' 'kitemmodels' 'kauth' 
         'kcoreaddons' 'ckbcomp' 'xdg-utils')
optdepends=('manjaro-settings-manager-notifier: qt-based'
            'manjaro-settings-manager-knotifier: knotifications-based')
makedepends=('extra-cmake-modules' 'kdoctools' 'qt5-tools' 'knotifications' 
             'kconfigwidgets' 'kcmutils')
conflicts=('kcm-msm')
source=("msm-$pkgver-$pkgrel.tar.gz::$url/-/archive/$pkgver/$pkgname-pkgver.tar.gz"
        "icu61.patch::$url/commit/b49453649ba5e8bf39ed4361807f6be9e1dac3cd.diff"
        "140.patch::$url/commit/b329c132e1043e0462e0ccb39cc0eecc186d2cfb.diff"
        "pacman51.patch::$url/commit/d60f1db3f9a2fbadf8c330ae072a4f2b69e3d253.diff")
sha256sums=('1666bb12849be0d020c31ced9866de9b0ee03f16914b425511094fa772dfbb0b'
            '8bf306d12fae9994f2037ea29720a8d88c7513f73772ccb3d637138f3053ac3a'
            'fee9a14a8438d315edd5d625ca1647fcf334a303e053f1d1c278422426926e48'
            '34d3b92ef5f1c8c86060a6f65b2fc1dbed738e7800fa61a2579263d5991ff652')

prepare() {
  cd "$srcdir/${pkgbase}-${pkgver}"
  patch -p1 -i "$srcdir/icu61.patch"
  patch -p1 -i "$srcdir/140.patch"
  patch -p1 -i "$srcdir/pacman51.patch"
}

build() {
  cd "$srcdir/${pkgbase}-${pkgver}"
  mkdir -p build
  cd build
  cmake ../ \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DLIB_INSTALL_DIR=lib \
    -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
    -DSYSCONF_INSTALL_DIR=/etc
  CXXFLAGS+="-std=gnu++98" make
}

package_manjaro-settings-manager() {
  cd "$srcdir/${pkgbase}-${pkgver}/build"
  make DESTDIR=${pkgdir} install 
  rm -rf $pkgdir/usr/bin/msm_notifier
  rm -rf $pkgdir/usr/bin/msm_kde_notifier
  rm -rf $pkgdir/usr/lib/qt
  rm -rf $pkgdir/usr/share/kservices5
  rm -rf $pkgdir/usr/share/applications/msm_notifier_settings.desktop
  rm -rf $pkgdir/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf $pkgdir/etc/xdg
}

package_manjaro-settings-manager-kcm() {
  pkgdesc="Manjaro Linux System Settings Tool (KCM for Plasma 5)"
  depends=('manjaro-settings-manager' 'kcmutils' 'kconfigwidgets')
  replaces=('kcm-msm')
  cd "$srcdir/${pkgbase}-${pkgver}/build"
  make DESTDIR=${pkgdir} install
  rm -rf $pkgdir/etc  
  rm -rf $pkgdir/usr/bin
  rm -rf $pkgdir/usr/lib/kauth
  rm -rf $pkgdir/usr/share/{applications,dbus-1,icons,polkit-1}
}

package_manjaro-settings-manager-notifier() {
  pkgdesc="Manjaro Linux System Settings Tool (Notifier)"
  depends=('manjaro-settings-manager')
  provides=('manjaro-settings-manager-kde-notifier')
  conflicts=('manjaro-settings-manager-kde-notifier')
  cd "$srcdir/${pkgbase}-${pkgver}/build"
  make DESTDIR=${pkgdir} install
  rm -rf $pkgdir/etc/dbus-1
  rm -rf $pkgdir/etc/xdg/autostart/msm_kde_notifier.desktop
  rm -rf $pkgdir/usr/lib/
  rm -rf $pkgdir/usr/share/{kservices5,dbus-1,icons,polkit-1}
  rm -rf $pkgdir/usr/share/applications/manjaro*
  rm -rf $pkgdir/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf $pkgdir/usr/bin/manjaro*
  rm -rf $pkgdir/usr/bin/msm_kde_notifier
}

package_manjaro-settings-manager-knotifier() {
  pkgdesc="Manjaro Linux System Settings Tool (Notifier for Plasma 5)"
  depends=('manjaro-settings-manager' 'knotifications')
  conflicts=('manjaro-settings-manager-notifier')
  replaces=('manjaro-settings-manager-kde-notifier')
  cd "$srcdir/${pkgbase}-${pkgver}/build"
  make DESTDIR=${pkgdir} install
  rm -rf $pkgdir/etc/dbus-1
  rm -rf $pkgdir/etc/xdg/autostart/msm_notifier.desktop
  rm -rf $pkgdir/usr/lib/
  rm -rf $pkgdir/usr/share/{kservices5,dbus-1,icons,polkit-1}
  rm -rf $pkgdir/usr/share/applications/manjaro*
  rm -rf $pkgdir/usr/share/applications/msm_notifier_settings.desktop
  rm -rf $pkgdir/usr/bin/manjaro*
  rm -rf $pkgdir/usr/bin/msm_notifier
} 