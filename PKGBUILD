# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>

pkgbase=manjaro-settings-manager
pkgname=('manjaro-settings-manager' 'manjaro-settings-manager-kcm' 
         'manjaro-settings-manager-notifier' 'manjaro-settings-manager-knotifier')
pkgver=0.5.7
_commit=cdf9c77eb1a7701bec7dce431f85b0976964a98f
pkgrel=4
pkgdesc="Manjaro Linux System Settings Tool"
arch=('x86_64')
url="https://gitlab.manjaro.org/applications/manjaro-settings-manager"
license=("GPL")
depends=('icu<71.2' 'qt5-base<5.15.5' 'hwinfo' 'kitemmodels' 'kauth'
         'kcoreaddons' 'ckbcomp' 'xdg-utils')
optdepends=('manjaro-settings-manager-notifier: qt-based'
            'manjaro-settings-manager-knotifier: knotifications-based')
makedepends=('extra-cmake-modules' 'kdoctools' 'qt5-tools' 'knotifications' 
             'kconfigwidgets' 'kcmutils')
conflicts=('kcm-msm')
source=(#"msm-$pkgver-$pkgrel.tar.gz::$url/-/archive/$pkgver/$pkgbase-$pkgver.tar.gz")
        "msm-$pkgver-$pkgrel-$_commit.tar.gz::$url/-/archive/$_commit/$pkgbase-$_commit.tar.gz")
sha256sums=('7364cb5be516b4a53e9f7ed039a863bf4620d2f2bd87150875e4e9f138195e22')

prepare() {
  mv ${pkgbase}-${_commit} ${pkgbase}-${pkgver}

  cd "$srcdir/${pkgbase}-${pkgver}"
  # patches here
}

build() {
  cmake -B build -S "${pkgbase}-${pkgver}" \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DLIB_INSTALL_DIR=lib \
    -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
    -DSYSCONF_INSTALL_DIR=/etc
  CXXFLAGS+="-std=gnu++11" make -C build
}

package_manjaro-settings-manager() {
  make -C build DESTDIR=${pkgdir} install

  rm -rf $pkgdir/usr/bin/msm_notifier
  rm -rf $pkgdir/usr/bin/msm_kde_notifier
  rm -rf $pkgdir/usr/lib/qt
  rm -rf $pkgdir/usr/share/systemsettings/categories/
  rm -rf $pkgdir/usr/share/kservices5
  rm -rf $pkgdir/usr/share/applications/msm_notifier_settings.desktop
  rm -rf $pkgdir/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf $pkgdir/etc/xdg
}

package_manjaro-settings-manager-kcm() {
  pkgdesc+=" (KCM for Plasma 5)"
  depends=('manjaro-settings-manager' 'kcmutils' 'kconfigwidgets')
  replaces=('kcm-msm')

  make -C build DESTDIR=${pkgdir} install

  rm -rf $pkgdir/etc  
  rm -rf $pkgdir/usr/bin
  rm -rf $pkgdir/usr/lib/kauth
  rm -rf $pkgdir/usr/share/{applications,dbus-1,icons,polkit-1}
}

package_manjaro-settings-manager-notifier() {
  pkgdesc+=" (Notifier)"
  depends=('manjaro-settings-manager')
  provides=('manjaro-settings-manager-kde-notifier')
  conflicts=('manjaro-settings-manager-kde-notifier')

  make -C build DESTDIR=${pkgdir} install

  rm -rf $pkgdir/etc/dbus-1
  rm -rf $pkgdir/etc/xdg/autostart/msm_kde_notifier.desktop
  rm -rf $pkgdir/usr/lib/
  rm -rf $pkgdir/usr/share/systemsettings/categories/
  rm -rf $pkgdir/usr/share/{kservices5,dbus-1,icons,polkit-1}
  rm -rf $pkgdir/usr/share/applications/manjaro*
  rm -rf $pkgdir/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf $pkgdir/usr/bin/manjaro*
  rm -rf $pkgdir/usr/bin/msm_kde_notifier
}

package_manjaro-settings-manager-knotifier() {
  pkgdesc+=" (Notifier for Plasma 5)"
  depends=('manjaro-settings-manager' 'knotifications')
  conflicts=('manjaro-settings-manager-notifier')
  replaces=('manjaro-settings-manager-kde-notifier')

  make -C build DESTDIR=${pkgdir} install

  rm -rf $pkgdir/etc/dbus-1
  rm -rf $pkgdir/etc/xdg/autostart/msm_notifier.desktop
  rm -rf $pkgdir/usr/lib/
  rm -rf $pkgdir/usr/share/systemsettings/categories/
  rm -rf $pkgdir/usr/share/{kservices5,dbus-1,icons,polkit-1}
  rm -rf $pkgdir/usr/share/applications/manjaro*
  rm -rf $pkgdir/usr/share/applications/msm_notifier_settings.desktop
  rm -rf $pkgdir/usr/bin/manjaro*
  rm -rf $pkgdir/usr/bin/msm_notifier
}
