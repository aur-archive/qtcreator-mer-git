# Contributor: Riccardo Ferrazzo <f.riccardo87@ gmail com>
# Based on qtcreator-git package

pkgname=qtcreator-mer-git
pkgver=20121214
pkgrel=1
pkgdesc="Qt Creator is a new cross-platform integrated development environment (IDE) tailored to the needs of Qt developers."
arch=('i686' 'x86_64')
url="http://www.qtsoftware.com/products/developer-tools"
license=('LGPL')
depends=('qt>=4.7' 'qt-private-headers')
makedepends=('git')
optdepends=('qt-doc: for the integrated Qt documentation'
'gdb: for the debugger')
provides=('qtcreator')
conflicts=('qtcreator')
options=(docs)
source=('qtcreator.desktop')
md5sums=('380736ffef157a0e2c67edaee8fa5de6')

_gitroot="git://gitorious.org/+mer-qt-creator/qt-creator/mer-qt-creator.git"
_gitname="mer-qt-creator"

build() {
    msg "Connecting to GIT server"

    if [ -d ${srcdir}/$_gitname ] ; then
        cd ${srcdir}/$_gitname && git pull origin || return 1
        msg "The local files are updated."
    else
        git clone $_gitroot $_gitname || return 1
    fi

    msg "GIT checkout done or server timeout"

    cd "${srcdir}/${_gitname}" || return 1

    git checkout mer

    msg "Starting make in quiet mode"

    if [ -d ${srcdir}/build ]
    then
        cd ${srcdir}/build
    else
        mkdir ${srcdir}/build
        cd ${srcdir}/build
    fi
    mkdir -p share/doc/qtcreator
    touch share/doc/qtcreator/qtcreator.qch

    # to enable the qml designer you need to add QT_PRIVATE_HEADERS=/path/to/your/qt/source/include to the qmake command
    qmake QT_PRIVATE_HEADERS=/usr/include ${srcdir}/${_gitname}/qtcreator.pro \
    && make --quiet \
    && make INSTALL_ROOT="${pkgdir}/usr" install || return 1
    install -Dm644 ${srcdir}/qtcreator.desktop ${pkgdir}/usr/share/applications/qtcreator.desktop || return 1
    install -Dm644 ${srcdir}/$_gitname/LGPL_EXCEPTION.TXT ${pkgdir}/usr/share/licenses/qtcreator/LGPL_EXCEPTION.TXT || return 1
}


