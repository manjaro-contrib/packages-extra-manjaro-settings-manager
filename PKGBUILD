# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Bernhard Landauer <bernhard[at]manjaro[dot]org>

pkgbase=manjaro-settings-manager
pkgname=(
  'manjaro-settings-manager-qt6'
  'manjaro-settings-manager-kcm-qt6'
  'manjaro-settings-manager-notifier-qt6'
  'manjaro-settings-manager-kstatus-notifier'
)
pkgver=0.6.0
pkgrel=1
pkgdesc="Manjaro Linux System Settings Tool"
arch=('x86_64')
url="https://gitlab.manjaro.org/applications/manjaro-settings-manager"
license=('GPL-3.0-or-later')
depends=(
  'ckbcomp'
  'hicolor-icon-theme'
  'hwinfo'
  'icu'
  'kauth'
  'kcoreaddons'
  'kitemmodels'
  'qt6-base'
  'xdg-utils'
  'xorg-xset'
)
makedepends=(
  'extra-cmake-modules'
  'git'
  'kcmutils'
  'kconfigwidgets'
  'kdoctools'
  'kstatusnotifieritem'
  'qt6-tools'
)
checkdepends=(
  'appstream'
  'desktop-file-utils'
)
options=('!lto')
_commit=43e192cbd6c2213a6a7a64d3a803f074884cffe8  # branch/qt6
source=("git+https://gitlab.manjaro.org/applications/manjaro-settings-manager.git#commit=${_commit}")
sha256sums=('94530e3fdb4d1160eb70d13ad86c474766ea71eaf1d51a25e10ae08e0bd55b16')

prepare() {
  cd "$pkgbase"

  # Bump PROJECT_VERSION
  sed -i "s/0.5.8/$pkgver/g" CMakeLists.txt
}

build() {
  local cmake_options=(
    -B build
    -S "$pkgbase"
    -D CMAKE_BUILD_TYPE='RelWithDebInfo'
    -D CMAKE_INSTALL_PREFIX='/usr'
  )
  cmake "${cmake_options[@]}"
  cmake --build build
}

check() {
  local ctest_flags=(
    --test-dir build
    --output-on-failure
    --parallel $(nproc)
  )
  ctest "${ctest_flags[@]}"

  cd "$pkgbase"
  desktop-file-validate "src/msm/$pkgbase.desktop"
  desktop-file-validate src/notifier/notifier/msm_notifier_settings.desktop
  desktop-file-validate src/notifier/notifier_kde/msm_kde_notifier_settings.desktop
}

package_manjaro-settings-manager-qt6() {
  optdepends=(
    'manjaro-settings-manager-notifier-qt6: Qt based'
    'manjaro-settings-manager-kstatus: KStatusNotifierItem based'
  )
  provides=('manjaro-settings-manager')
  conflicts=(
    'kcm-msm'
    'manjaro-settings-manager'
  )

  DESTDIR="$pkgdir" cmake --install build

  rm -f "$pkgdir"/usr/bin/msm_notifier
  rm -f "$pkgdir"/usr/bin/msm_kde_notifier
  rm -rf "$pkgdir"/usr/lib/qt/
  rm -rf "$pkgdir"/usr/share/systemsettings/
  rm -f "$pkgdir"/usr/share/applications/msm_notifier_settings.desktop
  rm -f "$pkgdir"/usr/share/applications/msm_kde_notifier_settings.desktop
  rm -rf "$pkgdir"/etc/
}

package_manjaro-settings-manager-kcm-qt6() {
  pkgdesc+=" (KCM for Plasma 6)"
  depends=(
    'kcmutils'
    'kconfigwidgets'
    'manjaro-settings-manager-qt6'
  )
  provides=('manjaro-settings-manager-kcm')
  conflicts=('manjaro-settings-manager-kcm')
  replaces=('kcm-msm')

  DESTDIR="$pkgdir" cmake --install build

  rm -rf "$pkgdir"/etc/
  rm -rf "$pkgdir"/usr/bin/
  rm -rf "$pkgdir"/usr/lib/{kf6,plugins}/
  rm -rf "$pkgdir"/usr/share/{applications,dbus-1,icons,polkit-1}/
  rm -rf "$pkgdir"/usr/share/{dbus-1,icons,polkit-1}/
}

package_manjaro-settings-manager-notifier-qt6() {
  pkgdesc+=" (Notifier)"
  depends=('manjaro-settings-manager-qt6')
  provides=(
    'manjaro-settings-manager-kde-notifier'
    'manjaro-settings-manager-notifier'
  )
  conflicts=(
    'manjaro-settings-manager-kde-notifier'
    'manjaro-settings-manager-notifier'
  )

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier
}

package_manjaro-settings-manager-kstatus-notifier() {
  pkgdesc+=" (KStatus Notifier for Plasma 6)"
  depends=(
    'kstatusnotifieritem'
    'manjaro-settings-manager-qt6'
  )
  conflicts=(
    'manjaro-settings-manager-notifier'
    'manjaro-settings-manager-knotifier'
  )
  replaces=(
    'manjaro-settings-manager-kde-notifier'
    'manjaro-settings-manager-knotifier'
  )

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier_kde
}
