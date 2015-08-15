# Maintainer:  veox <box 55 [shift-two] mail [dot] ru>

pkgname=cl-cffi-git
_clname=cl-cffi    # used in CL scope, not package scope
_pkgname=cffi

conflicts=('cl-cffi')
provides=('cl-cffi')

pkgver=20120722
pkgrel=1
pkgdesc="Common Foreign Function Interface for Common Lisp"
arch=('i686' 'x86_64')     # 32-bit only? needs gnu/stubs-32.h
url="http://common-lisp.net/project/cffi/"
license=('BSD')

# TODO: replace this segment with 'common-lisp' when all provide it.
# TODO: compiling 'tests' with 'make' on x86_64 will fail due to the way
#       tests/GNUmakefile is written: it issues '-m32' to gcc, which
#       (probably?) requires the 'gcc-multilib' package, without it, only
#       the 64-bit version can be compiled
if pacman -Qq clisp-new-clx &>/dev/null; then
    depends=('clisp-new-clx' 'cl-asdf' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
elif pacman -Qq clisp-gtk2 &>/dev/null; then
    depends=('clisp-gtk2' 'cl-asdf' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
elif pacman -Qq sbcl &>/dev/null; then
    depends=('libffi' 'sbcl' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
elif pacman -Qq clisp &>/dev/null; then
    depends=('libffi' 'clisp' 'cl-asdf' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
elif pacman -Qq cmucl &>/dev/null; then
    depends=('libffi' 'cmucl' 'cl-asdf' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
else
    depends=('libffi' 'sbcl' 'cl-alexandria' 'cl-babel' 'cl-trivial-features' 'libffi')
fi

makedepends=('git')

install=${pkgname}.install
md5sums=('1273768ac353f2c8387b1f39fc68a5cc')

source=("libffi.patch")
md5sums=('6f13776379715c7349a1c21f063a2362')

_gitroot="git://common-lisp.net/projects/cffi/cffi.git"
_gitname="cffi"

build() {
    
    cat << EOM

	WARNING!

	You are building a package using a snapshot from a repository. The
        resulting package may be unusable or pose a security risk, since
        the install script does not check source file hashes. Do not continue
        if this is undesirable.

EOM
    

    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/examples
    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/grovel
    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/src
    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/tests
    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/uffi-compat
    install -d ${pkgdir}/usr/share/common-lisp/source/${_clname}/libffi
    install -d ${pkgdir}/usr/share/common-lisp/systems
    install -d ${pkgdir}/usr/share/licenses/${pkgname}

    ### Git checkout
    cd "$srcdir"
    msg "Connecting to GIT server...."
    
    if [ -d $_gitname ] ; then
	cd $_gitname && git pull origin
	msg "The local files are updated."
    else
	git clone $_gitroot $_gitname
    fi
    cd ${srcdir}/${_gitname}

    patch -p1 < ${srcdir}/libffi.patch

    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/examples \
        ${srcdir}/${_gitname}/examples/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/grovel \
        ${srcdir}/${_gitname}/grovel/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/src \
        ${srcdir}/${_gitname}/src/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/tests \
        ${srcdir}/${_gitname}/tests/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/libffi \
        ${srcdir}/${_gitname}/libffi/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname}/uffi-compat \
        ${srcdir}/${_gitname}/uffi-compat/*
    install -m 644 -t ${pkgdir}/usr/share/common-lisp/source/${_clname} \
        ${srcdir}/${_gitname}/*.asd
    
    install -m 644 ${srcdir}/${_gitname}/COPYRIGHT \
        ${pkgdir}/usr/share/licenses/${pkgname}
    
    cd ${pkgdir}/usr/share/common-lisp/systems
    ln -s ../source/${_clname}/${_pkgname}.asd .
    ln -s ../source/${_clname}/${_pkgname}-examples.asd .
    ln -s ../source/${_clname}/${_pkgname}-grovel.asd .
    ln -s ../source/${_clname}/${_clname}-tests.asd .
    ln -s ../source/${_clname}/${_clname}-libffi.asd .
    ln -s ../source/${_clname}/${_pkgname}-uffi-compat.asd .
}

# vim:set ts=2 sw=4 et nospell:
