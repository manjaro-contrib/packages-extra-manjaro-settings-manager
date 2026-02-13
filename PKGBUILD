# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Bernhard Landauer <bernhard[at]manjaro[dot]org>

pkgbase=manjaro-settings-manager
pkgname=(
  'manjaro-settings-manager'
#  'manjaro-settings-manager-kcm'
  'manjaro-settings-manager-notifier'
  'manjaro-settings-manager-knotifier'
)
pkgver=0.5.8
pkgrel=8
_commit=95b71db3ab1e4ed74c3c320bebdc7d7fbf98c58e
pkgdesc="Manjaro Linux System Settings Tool"
arch=('x86_64')
url="https://gitlab.manjaro.org/applications/manjaro-settings-manager"
license=('GPL-3.0-or-later')
depends=(
  'ckbcomp'
  'hwinfo<=26'
  'icu'
  'kauth5'
  'kcoreaddons5'
  'kitemmodels5'
  'qt5-base'
  'xdg-utils'
)
makedepends=(
  'extra-cmake-modules'
  'git'
  'kcmutils5'
  'kconfigwidgets5'
  'kdoctools5'
  'knotifications5'
  'qt5-tools'
)
checkdepends=('appstream')
source=(#"git+https://gitlab.manjaro.org/applications/manjaro-settings-manager.git#tag=$pkgver"
        "git+https://gitlab.manjaro.org/applications/manjaro-settings-manager.git#commit=$_commit")
sha256sums=('a1c5aee40456e2ba4dad89a1af5e7cc25e99b9e01bb16316d7c36e27812ed29a')

prepare() {
  cd "$pkgbase"
}

build() {
  cmake -B build -S "$pkgbase" \
    -DCMAKE_BUILD_TYPE='RelWithDebInfo' \
    -DCMAKE_INSTALL_PREFIX='/usr' \
    -DCMAKE_CXX_STANDARD='17' \
    -DCMAKE_CXX_STANDARD_REQUIRED='ON' \
    -DCMAKE_CXX_EXTENSIONS='OFF' \
    -DKDE_INSTALL_LIBDIR='lib' \
    -DKDE_INSTALL_USE_QT_SYS_PATHS='ON' \
    -DKDE_INSTALL_SYSCONFDIR='/etc' \
    -DCMAKE_POLICY_VERSION_MINIMUM='3.5'
  cmake --build build
}

check() {
  ctest --test-dir build --output-on-failure
}

package_manjaro-settings-manager() {
  optdepends=('manjaro-settings-manager-notifier: qt-based'
              'manjaro-settings-manager-knotifier: knotifications-based')
  conflicts=('kcm-msm')

  DESTDIR="$pkgdir" cmake --install build/

  rm -rf "$pkgdir"/usr/bin/msm_notifier
  rm -rf "$pkgdir"/usr/bin/msm_kde_notifier
  rm -rf "$pkgdir"/usr/lib/qt
  rm -rf "$pkgdir"/usr/share/systemsettings/categories/
  rm -rf "$pkgdir"/usr/share/kservices5
  rm -rf "$pkgdir"/usr/share/applications/msm_notifier_settings.desktop
  rm -rf "$pkgdir"/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf "$pkgdir"/etc/xdg
}

#package_manjaro-settings-manager-kcm() {
#  pkgdesc+=" (KCM for Plasma 5)"
#  depends=('kcmutils5' 'kconfigwidgets5' 'manjaro-settings-manager')
#  replaces=('kcm-msm')

#  DESTDIR="$pkgdir" cmake --install build

#  rm -rf "$pkgdir"/etc
#  rm -rf "$pkgdir"/usr/bin
#  rm -rf "$pkgdir"/usr/lib/kauth
#  rm -rf "$pkgdir"/usr/share/{applications,dbus-1,icons,polkit-1}
#}

package_manjaro-settings-manager-notifier() {
  pkgdesc+=" (Notifier)"
  depends=('manjaro-settings-manager')
  provides=('manjaro-settings-manager-kde-notifier')
  conflicts=('manjaro-settings-manager-kde-notifier')

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier
}

package_manjaro-settings-manager-knotifier() {
  pkgdesc+=" (Notifier for Plasma 5)"
  depends=('knotifications5' 'manjaro-settings-manager')
  conflicts=('manjaro-settings-manager-notifier')
  replaces=('manjaro-settings-manager-kde-notifier')

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier_kde
}
