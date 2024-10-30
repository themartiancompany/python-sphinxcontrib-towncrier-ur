# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Truocolo <truocolo@aol.com>

_os="$( \
  uname \
    -o)"
_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=sphinxcontrib-towncrier
pkgname="${_py}-${_pkg}"
pkgver=0.4.0a0
pkgrel=1
_pkgdesc=(
  "An RST directive for injecting a"
  "Towncrier-generated changelog draft"
  "containing fragments for the unreleased"
  "(next) project version."
)
pkgdesc="${_pkgdesc[*]}"
_http="https://github.com"
_ns="sphinx-contrib"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD'
)
arch=(
  'any'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  'towncrier'
)
makedepends=(
  "${_py}-build"
  "${_py}-installer"
  "${_py}-setuptools"
  "${_py}-setuptools-scm"
  "${_py}-wheel"
)
checkdepends=(
  "${_py}-pytest-xdist"
)
_pypi="https://files.pythonhosted.org/packages/source"
source=(
  # see below
  # "${url}/archive/refs/tags/v${pkgver}.tar.gz"
  "${_pypi}/${_pkg::1}/${_pkg}/${_pkg}-${pkgver}.tar.gz"
  0001-Update-build-system-dependencies.patch
)
sha512sums=(
  # checksum below seems to have changed
  # see ${url}/issues/95
  # 'a828a541ca8a6a492c990de79ed97bfcc082b27f0ab91fd811632bdabcfc64261ee4a172d098e236bf6a36caa640bc252583337050e92f3833aaa97be0da3d4e'
  'f534b8aeee2c94d980330ddeea81f93af6ebfbdeb178b26088e8f91425cad82b07916a0e95d8057a8775163c94e9d9be0279c8f3577afc0b52b974cd2f2d03f5'
  'e8c5f943e4ad8990a97a6aa8e493a1346cd9725a08c2b3e6ab0f3eb9371b9a63a55bad242060916f8a5ebfefe2a4c5c96b9a73291996fe3a4693c9b0920ca6b8'
)

prepare() {
  cd \
    "${_pkg}-${pkgver}"
  # remove requirement for
  # python-setuptools-scm-git-archive:
  # https://github.com/sphinx-contrib/sphinxcontrib-towncrier/pull/80
  patch \
    -Np1 \
    -i \
    ../0001-Update-build-system-dependencies.patch
  sed \
    -i \
    '/pytest_cov/d;/--cov/d' \
    pytest.ini
  # Do not treat warnings as errors
  sed \
    -i \
    '/^  error$/d' \
    pytest.ini
}

build() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m \
      build \
    -nw
}

check() {
  cd \
    "${_pkg}-${pkgver}"
  PYTHONPATH="$PWD"/src \
  pytest
}

package() {
  cd \
    "${_pkg}-${pkgver}"
  "${_py}" \
    -m installer \
    --destdir="${pkgdir}" \
    dist/*.whl
  install \
    -Dm644 \
    LICENSE \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}

# vim:set sw=2 sts=-1 et:
