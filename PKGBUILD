# Maintainer: Andy Kluger <AndyKluger@gmail.com>
# Contributor: Markus Weimar <mail@markusweimar.de>
pkgname=ttf-iosevka-custom-git
pkgver=r1708.060a4f2a
pkgrel=1
pkgdesc='A slender monospace sans-serif and slab-serif typeface inspired by Pragmata Pro, M+ and PF DIN Mono.'
arch=('any')
url='https://be5invis.github.io/Iosevka/'
license=('custom:OFL')
makedepends=('afdko' 'git' 'nodejs' 'npm' 'otfcc' 'ttfautohint')
depends=('fontconfig' 'xorg-font-utils')
conflicts=('ttf-iosevka-custom')
provides=('ttf-iosevka-custom')
source=(
  'git+https://github.com/be5invis/Iosevka'
  'private-build-plans.toml'
)
sha256sums=(
  'SKIP'
  'SKIP'
)

pkgver() {
  cd Iosevka
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  buildplans="$HOME/.config/iosevka/private-build-plans.toml"
  if [[ -f "$buildplans" ]]; then
    cp "$buildplans" Iosevka/
  else
    echo ">>> $buildplans not found, using private-build-plans.toml"
    cp private-build-plans.toml Iosevka/private-build-plans.toml
  fi

  # fixes for leading-1000
  sed -i 's/^cap = [0-9]*/cap = 700/' Iosevka/params/parameters.toml
  sed -i 's/^xheight = [0-9]*/xheight = 510/' Iosevka/params/parameters.toml
  sed -i 's/^symbolMid = [0-9]*/symbolMid = 300/' Iosevka/params/parameters.toml
  sed -i 's/^parenSize = [0-9]*/parenSize = 950/' Iosevka/params/parameters.toml
}

build() {
  cd Iosevka
  npm install
  npm update
  npm run build -- ttf::iosevka-custom
  npm run build -- ttf::iosevka-fixed-custom
  npm run build -- ttf::iosevka-term-custom
}

package() {
  install -d "${pkgdir}/usr/share/fonts/TTF"
  install -m644 Iosevka/dist/*/ttf/*.ttf "${pkgdir}/usr/share/fonts/TTF/"
  install -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 Iosevka/LICENSE.md "${pkgdir}/usr/share/licenses/${pkgname}/"
}
