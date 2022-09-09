# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>

pkgbase=manjaro-settings-manager
pkgname=('manjaro-settings-manager' 'manjaro-settings-manager-kcm' 
         'manjaro-settings-manager-notifier' 'manjaro-settings-manager-knotifier')
pkgver=0.5.7
_commit=cdf9c77eb1a7701bec7dce431f85b0976964a98f
pkgrel=10
pkgdesc="Manjaro Linux System Settings Tool"
arch=('x86_64')
url="https://gitlab.manjaro.org/applications/manjaro-settings-manager"
license=("GPL")
depends=('icu<71.2' 'qt5-base<5.15.7' 'hwinfo' 'kitemmodels' 'kauth'
         'kcoreaddons' 'ckbcomp' 'xdg-utils')
makedepends=('extra-cmake-modules' 'kdoctools' 'qt5-tools' 'knotifications' 
             'kconfigwidgets' 'kcmutils')
checkdepends=('appstream')
source=(
#        "$url/-/archive/$pkgver/$pkgbase-$pkgver.tar.gz"
        "$url/-/archive/$_commit/$pkgbase-$_commit.tar.gz"
        'hideRemoveButton.patch'
        'installPackagesCN.patch'
        'installRemoveKernelCN.patch'
        'rateRUTranslation.patch'
        )
sha256sums=('7364cb5be516b4a53e9f7ed039a863bf4620d2f2bd87150875e4e9f138195e22'
            '4ac7c0120dcc4272cc8ba9e3bfd7a7c329b8fae8bfd71ee89f961b1529f14d44'
            '2200c3617148e916a77e669fcd4916795f4486448d0750b86e10fe235651b3e3'
            '8ceac84432cdaef87ebab4eded03e9d90c190e173efa7de76d60e61880082326'
            'e3dae932a552f5cab3cc376e0bd1926a47483d4282b58f87894f57441d865365')

prepare() {
  mv ${pkgbase}-${_commit} ${pkgbase}-${pkgver}

  cd "$srcdir/${pkgbase}-${pkgver}"

  # https://gitlab.manjaro.org/applications/manjaro-settings-manager/-/issues/204
  patch -Np1 -i ../hideRemoveButton.patch

  # https://gitlab.manjaro.org/applications/manjaro-settings-manager/-/issues/212
  patch -Np1 -i ../installPackagesCN.patch

  # https://gitlab.manjaro.org/applications/manjaro-settings-manager/-/issues/167
  patch -Np1 -i ../installRemoveKernelCN.patch

  # https://gitlab.manjaro.org/applications/manjaro-settings-manager/-/issues/168
  patch -Np1 -i ../rateRUTranslation.patch
}

build() {
  cmake -B build -S "${pkgbase}-${pkgver}" \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DKDE_INSTALL_LIBDIR=lib \
    -DKDE_INSTALL_USE_QT_SYS_PATHS=ON \
    -DKDE_INSTALL_SYSCONFDIR=/etc
  CXXFLAGS+="-std=gnu++11" cmake --build build
}

check() {
  ctest --test-dir build --output-on-failure
}

package_manjaro-settings-manager() {
  optdepends=('manjaro-settings-manager-notifier: qt-based'
              'manjaro-settings-manager-knotifier: knotifications-based')
  conflicts=('kcm-msm')

  DESTDIR="$pkgdir" cmake --install build/

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

  DESTDIR="$pkgdir" cmake --install build

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

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier
}

package_manjaro-settings-manager-knotifier() {
  pkgdesc+=" (Notifier for Plasma 5)"
  depends=('manjaro-settings-manager' 'knotifications')
  conflicts=('manjaro-settings-manager-notifier')
  replaces=('manjaro-settings-manager-kde-notifier')

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier_kde
}
