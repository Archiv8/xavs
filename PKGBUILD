#!/bin/bash

# Disable various shellcheck rules that produce false positives in this file.
# Repository rules should be added to the .shellcheckrc file located in the
# repository root directory, see https://github.com/koalaman/shellcheck/wiki
# and https://archiv8.github.io for further information.
# shellcheck disable=SC2034,SC2154
# [Query]: Upstream seems unmaintained as of 2011-08-21 
# [ToDo]:  Add files: User documentation
# [ToDo]:  Add files: Tooling
# [FixMe]: Namcap warnings and errors
# [FixMe]: Does not build
#          /usr/bin/ld: cannot find -lxavs: No such file or directory
#          collect2: error: ld returned 1 exit status
#          make: *** [Makefile:73: xavs] Error 1

# Maintainer: Ross Clark <archiv8@artisteducator.com>
# Contributor: Ross Clark <archiv8@artisteducator.com>

pkgname=xavs
pkgver=0.1.55
pkgrel=1
pkgdesc="XAVS is to implement high quality encoder and decoder of the Audio Video Standard of China (AVS)."
arch=(i686 x86_64 arm)
url="http://xavs.sourceforge.net/"
license=(
    "GPL"
)
# depends=()
makedepends=(
    # Arch Linux dependencies
    "yasm"
)
#options=(!strip)

source=(
    "${pkgname}-${pkgver}.tar.gz::https://github.com/OpenMandrivaAssociation/xavs/raw/master/xavs-${pkgver}.tar.xz" # pkgname=xavs, pkgver=0.1.55
    https://github.com/OpenMandrivaAssociation/xavs/raw/master/xavs-0.1.55-dont-strip-symbols.patch
    https://github.com/pld-linux/xavs/raw/master/xavs-dynamic-xavs.patch
    xavs-dup-asm.patch
    xavs-x32-yasm.patch
)
sha512sums=(
    "6fcc38c103a9fa1ced4c6d4723b8c345dc6c631b0aac499cb42fc95323f18bf175c96d8c2e0de372352bc9338a8b486d156789765931367d594ab4d9c7e86857"
    "36bba684126d9ac9b6c8770fce06a8d4d1871da3c2b83cc80f961f63264c544b16eec05ddcb5c25046c2e0b1224339ead503450bf62992d3c8e98222eb0f2514"
    "fde60023b04ba97d16c469d59e7569705afa20e0e43d9ad99ff43c41e9aea40906b166a3b82e653ffb3674bb03a060dbf97dc0164a4d0586f3c59c88600fd866"
    "0964385247c1fbcee157bdd56616c1dae54663679f73d7033619f6fda359b30e2120ee666d46f81ca8c353c336ff5e2d9c23343ab0d4a17be9eacac0f4695386"
    "dcd7c8f131b7ddf5519334211a974f9ecde0177d77bc10884ed7763a4bf9129aa137fe2dd4af135929d80805917fba588294a5d0d05c992eeac41273aaf42a5f"
)

prepare() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    patch -Np1 -i ${srcdir}/xavs-dynamic-xavs.patch
    patch -Np1 -i ${srcdir}/xavs-0.1.55-dont-strip-symbols.patch
    patch -Np1 -i ${srcdir}/xavs-dup-asm.patch
    patch -Np1 -i ${srcdir}/xavs-x32-yasm.patch
    sed -i -e "s|$(CC) -o $@ $(OBJCLI) $(LDFLAGS) -L. -lxavs|$(CC) -o $@ $(OBJCLI) -L. -lxavs $(LDFLAGS)|" Makefile
    sed -i -e "s|xavs$(EXE): $(OBJCLI) $(SONAME)|xavs$(EXE): $(OBJCLI) $(SONAME) libxavs.a|" Makefile
}

build() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    ./configure --enable-shared --disable-asm --prefix=/usr
    make
}

package() {
    cd "$srcdir/${pkgname}-${pkgver}"
    make DESTDIR="${pkgdir}" install
    install -m 644 libxavs.a ${pkgdir}/usr/lib/libxavs.a
}
