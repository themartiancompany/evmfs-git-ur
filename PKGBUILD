# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer:  Truocolo <truocolo@aol.com>
# Maintainer:  Pellegrino Prevete <pellegrinoprevete@gmail.com>

_os="$( \
  uname \
    -o)"
_git='true'
_offline='false'
_proj="hip"
_pkg=evmfs
_pkgname="${_pkg}"
pkgname="${_pkgname}-git"
pkgver=0.0.0.0.0.0.0.0.0.1.1.1.1.1.1.r57.g33d43c8
pkgrel=1
_pkgdesc=(
  "Ethereum Virtual Machine network file system."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  any
)
_branch="master"
_tarname="${_pkg}-${_branch}"
_gl="gitlab.com"
_gh="github.com"
_host="https://${_gh}"
_ns='themartiancompany'
_local="file://${HOME}/${_pkg}"
url="${_host}/${_ns}/${_pkg}"
_gh_api="https://api.${_gh}/repos/${_ns}/${_pkg}"
license=(
  'AGPL3'
)
depends=(
  "evm-contracts-tools"
  "evm-wallet"
  "encoding-tools"
  "libcrash-bash"
  "libcrash-js"
)
[[ "${_os}" == 'GNU/Linux' ]] && \
  depends+=(
  )
makedepends=(
  'make'
  'solidity-compiler'
)
checkdepends=(
  shellcheck
)
optdepends=(
)
[[ "${_os}" == 'Android' ]] && \
  optdepends+=(
  )
provides=(
  "${_pkgname}=${pkgver}"
)
conflicts=(
  "${_pkgname}"
)
groups=(
 "${_proj}"
 "${_proj}-git"
)
_url="${url}"
if [[ "${_offline}" == true ]]; then
  _url="${_local}"
fi
if [[ "${_git}" == true ]]; then
  makedepends+=(
    git
  )
  _src="${_tarname}::git+${_url}#branch=${_branch}"
elif [[ "${_git}" == false ]]; then
  makedepends+=(
    curl
    jq
  )
  _src="${_tarname}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
fi
source=(
  "${_src}"
)
sha256sums=(
  SKIP
)

_nth() {
  local \
    _str="${1}" \
    _n="${2}"
  echo \
    "${_str}" | \
    awk \
      -F '+' \
      '{print $'"${_n}"'}'
}

_jq_pkgver() {
  local \
    _version \
    _rev \
    _commit
  _version="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].name')"
  _version_commit="$( \
    curl \
      --silent \
      "${_gh_api}/tags" | \
      jq \
        '.[0].commit.sha')"
  _rev="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        'map(.sha == '${_version_commit}' ) | index(true)')"
  _commit="$( \
    curl \
      --silent \
      "${_gh_api}/commits" | \
      jq \
        '.[0].sha')"
  printf \
    "%s.r%s.g%s" \
    "${_version}" \
    "${_rev}" \
    "${_commit}"
}

_parse_ver() {
  local \
    _pkgver="${1}" \
    _out="" \
    _ver \
    _rev \
    _commit
  _ver="$( \
    _nth \
      "${_pkgver}" \
      "1")"
  _rev="$( \
    _nth \
      "${_pkgver}" \
      "2")"
  _commit="$( \
    _nth \
      "${_pkgver}" \
      "3")"
  _out=${_ver}
  [[ "${_rev}" != "" ]] && \
    _out+=".r${_rev}"
  [[ "${_commit}" != "" ]] && \
    _out+=".${_commit}"
  echo \
    "${_out}"
}

_git_pkgver() {
  local \
    _pkgver
  _pkgver="$( \
    git \
      describe \
      --tags \
      --long | \
      sed \
        's/-/+/g')"
  _parse_ver \
    "${_pkgver}"
}

pkgver() {
  cd \
    "${_tarname}"
  if [[ "${_git}" == true ]]; then
    _git_pkgver
  elif [[ "${_git}" == false ]]; then
    _jq_pkgver
  fi
}

build() {
  cd \
    "${_tarname}"
  make \
    all
}

package() {
  cd \
    "${_tarname}"
  make \
    DESTDIR="${pkgdir}" \
    PREFIX="/usr" \
    install
}

# vim: ft=sh syn=sh et
