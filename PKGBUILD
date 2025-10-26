# Maintainer: Franck Duriez <franck@duriez.info>

_pkgbase=aic8800
pkgname=aic8800-dkms
pkgver=1.0.8
pkgrel=1

pkgdesc="Kernel modules for BrosTrend AX300 WiFi 6"
arch=('any')
url="https://linux.brostrend.com/troubleshooting/source-code/"
license=('GPL-2.0-only')
depends=('udev' 'dkms')
makedepends=('dos2unix')
conflicts=("${_pkgbase}")
source=(
  "https://linux.brostrend.com/${_pkgbase}-dkms.deb"
  '0001-Make-CONFIG_RFTEST-n-valid.patch'
  '0002-Fix-DKMS-config.patch'
  '0003-Fix-kernel-logs.patch'
  '0005-Add-Archer-TX1U-nano.patch'
)
sha512sums=(
  'ec9c36ddd97682a45f58c556917d89f0715fb594ffc454aaa8559d111d9e70a9b9c7ebb3170f9f23143d9eccea0dc979a79499b558b3205cc790aaa15af95afb'
  '748daf3a5590c4348919d64df26fda2102b5c638bd461ce4eebeda9030157cee30c130ede971396b11cac8a8a069dc185e2e8345f42b918cc8de600a824d7571'
  '4cc6102432cee0f8d2c157fc8a09c23d02f61ca1e3540b1d89bed1e31d8a4e379e1472e68e402d5fe64574c885bba9378bd025877a45fd7cbe4ebcf7f202b925'
  'e6b2eb9b140c55587e188c6edea0887aa8d27fa4989e8f6601b39fe3604ddd62f0845be79b0069ef4afd6f851afe7d6ca423856451ae082bcf4158203dc0d2c1'
  '3962cd74d32bc8142046fe6c56eff952327b713f38d337915e696609ce027a4dba7c795cef537f01fae929b7c60f9a4b968c58cf5d4a4e59ab61f4c937bea0be'
)

prepare() {
  cd "${srcdir}"
  tar xvzf "data.tar.gz"
  cd "usr/src/${_pkgbase}-${pkgver}"
  dos2unix */*.{c,h}
  patch -Np1 -i ../../../0001-Make-CONFIG_RFTEST-n-valid.patch -d .
  patch -Np1 -i ../../../0002-Fix-DKMS-config.patch -d .
  patch -Np1 -i ../../../0003-Fix-kernel-logs.patch -d .
  patch -Np1 -i ../../../0005-Add-Archer-TX1U-nano.patch -d .
}

package() {
  # Copy udev rules
  install -dm 755 "${pkgdir}/usr/lib/udev/rules.d"
  install -m 644 "${srcdir}/lib/udev/rules.d/aic.rules" "${pkgdir}/usr/lib/udev/rules.d/99-aic-modeswitch.rules"

  # Copy device firmware
  install -dm 755 "${pkgdir}/usr/lib/firmware"
  cp -dr --no-preserve=ownership "${srcdir}/lib/firmware/${_pkgbase}DC" "${pkgdir}/usr/lib/firmware"

  # Copy source and dkms config
  install -dm 755 "${pkgdir}/usr/src"
  cp -dr --no-preserve=ownership "${srcdir}/usr/src/${_pkgbase}-${pkgver}" "${pkgdir}/usr/src"

  # Prune unwanted files
  rm -f "${pkgdir}/usr/src/${_pkgbase}-${pkgver}/driverctl"
}
