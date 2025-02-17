# Mantainer: Randomly Chosen <ngmoviedo at gmx dot com>
# Adapted from https://daveparrish.net/posts/2019-11-16-Better-AppImage-PKGBUILD-template.html

_pkgname=altus

pkgname="${_pkgname}"-appimage
pkgver="4.4.3"
pkgrel=1
pkgdesc="Electron-based WhatsApp client with themes and multiple account support"
arch=('x86_64')
url="https://github.com/amanharwara/altus"
license=('GPL3')
depends=('zlib')
options=('!strip')

_appimage="Altus-${pkgver}-x86_64.AppImage"
source=("${_appimage}::https://github.com/amanharwara/altus/releases/download/${pkgver}/${_appimage}")
md5sums=('SKIP')

noextract=("${_appimage}")

prepare() {
    chmod +x "${_appimage}"
    ./"${_appimage}" --appimage-extract
}

build() {
    # Adjust .desktop so it will work outside of AppImage container
    sed -i -E "s|Exec=AppRun|Exec=env DESKTOPINTEGRATION=false /usr/bin/${_pkgname}|"\
        "squashfs-root/${_pkgname}.desktop"
    # Fix permissions; .AppImage permissions are 700 for all directories
    chmod -R a-x+rX squashfs-root/usr
}

package() {
    # AppImage
    install -Dm755 "${srcdir}/${_appimage}" "${pkgdir}/opt/${pkgname}/${pkgname}.AppImage"

    # Desktop file
    install -Dm644 "${srcdir}/squashfs-root/${_pkgname}.desktop"\
            "${pkgdir}/usr/share/applications/${_pkgname}.desktop"

    # Icon images
    install -dm755 "${pkgdir}/usr/share/"
    cp -a "${srcdir}/squashfs-root/usr/share/icons" "${pkgdir}/usr/share/"

    # Symlink executable
    install -dm755 "${pkgdir}/usr/bin"
    ln -s "/opt/${pkgname}/${pkgname}.AppImage" "${pkgdir}/usr/bin/${_pkgname}"
}
