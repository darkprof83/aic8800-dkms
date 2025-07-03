# Maintainer: Franck Duriez <franck@duriez.info>

_pkgbase=aic8800
pkgname=aic8800-dkms
pkgver=1.0.7
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
  'ed75b617c3c3e15a640099600fc1d87f34e7a6fb0b32159439d02ae122986fc9bad679cfd27a938a3e9a9dd303e816bdb776424489acde9fe6f581f676b63ef1'
  '748daf3a5590c4348919d64df26fda2102b5c638bd461ce4eebeda9030157cee30c130ede971396b11cac8a8a069dc185e2e8345f42b918cc8de600a824d7571'
  '4cc6102432cee0f8d2c157fc8a09c23d02f61ca1e3540b1d89bed1e31d8a4e379e1472e68e402d5fe64574c885bba9378bd025877a45fd7cbe4ebcf7f202b925'
  'e6b2eb9b140c55587e188c6edea0887aa8d27fa4989e8f6601b39fe3604ddd62f0845be79b0069ef4afd6f851afe7d6ca423856451ae082bcf4158203dc0d2c1'
  '55cf0b8678afd14d6e3f960b1c633e64b79ff4593b620fbd38ff8859852f26908116fdf47a5395c1297ecb0bc30de4ea9fffdce4556a39cfc5b0744b9e3a0b0e'
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
