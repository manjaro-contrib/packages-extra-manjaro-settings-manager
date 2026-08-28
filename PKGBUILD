# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Bernhard Landauer <bernhard[at]manjaro[dot]org>

pkgbase=manjaro-settings-manager
pkgname=(
  'manjaro-settings-manager-qt6'
  # 'manjaro-settings-manager-kcm-qt6'
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
_commit=fdf755184c1bc31823ead6f442a1f8372f13d49e  # branch/qt6
source=("git+https://gitlab.manjaro.org/applications/manjaro-settings-manager.git#commit=${_commit}")
sha256sums=('c1f112df8d939fadba16511f0fa5cfc51ca952ed06fb7d1f5d670ae657117d1b')

prepare() {
  cd "$pkgbase"

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
    'manjaro-settings-manager-notifier-qt6: Kernel release notifications'
    'manjaro-settings-manager-kstatus-notifier: Kernel release notifications (Plasma 6)'
  )
  provides=('manjaro-settings-manager')
  conflicts=(
    'kcm-msm'
    'manjaro-settings-manager'
  )

  DESTDIR="$pkgdir" cmake --install build

  rm -f "$pkgdir"/usr/bin/msm_notifier
  rm -f "$pkgdir"/usr/bin/msm_kde_notifier
  rm -rf "$pkgdir"/usr/lib/plugins/
  rm -rf "$pkgdir"/usr/share/systemsettings/
  rm -rf "$pkgdir"/etc/

  rm -rf "$pkgdir"/usr/share/applications/
  install -Dm644 "$pkgbase/src/msm/$pkgbase.desktop" -t \
    "$pkgdir"/usr/share/applications/
}

package_manjaro-settings-manager-kcm-qt6() {
  pkgdesc+=" (KCM for Plasma 6)"
  depends=(
    'kcmutils'
    'kconfigwidgets'
    'manjaro-settings-manager-qt6'
  )
  provides=('manjaro-settings-manager-kcm')
  conflicts=(
    'kcm-msm'
    'manjaro-settings-manager-kcm'
  )
  replaces=('kcm-msm')

  cd "$pkgbase"

  ## FIXME Find proper install directory for modules
  for module in kernel language_packages mhwd; do
    install -Dm755 "src/modules/${module}/msm_${module}.so" -t "$pkgdir/usr/lib/qt6/"
    install -Dm644 "src/modules/${module}/msm_${module}.desktop" -t \
      "$pkgdir/usr/share/applications/"
  done

  install -Dm644 src/manjaro-category.desktop -t \
    "$pkgdir/usr/share/systemsettings/categories/"
}

package_manjaro-settings-manager-notifier-qt6() {
  pkgdesc+=" (Notifier)"
  depends=('manjaro-settings-manager-qt6')
  provides=('manjaro-settings-manager-notifier')
  conflicts=(
    'manjaro-settings-manager-kde-notifier'
    'manjaro-settings-manager-notifier'
  )

  DESTDIR="$pkgdir" cmake --install build/src/notifier/notifier
}

package_manjaro-settings-manager-kstatus-notifier() {
  pkgdesc+=" (Notifier for Plasma 6)"
  depends=(
    'kstatusnotifieritem'
    'manjaro-settings-manager-qt6'
  )
  provides=('manjaro-settings-manager-notifier')
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
