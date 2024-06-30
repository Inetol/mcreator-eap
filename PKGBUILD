# Maintainer: Ivan Gabaldon <aur[at]inetol.net>

pkgname=mcreator-eap
_pkgvermajor=2024.2
_pkgverbuild=24713
pkgver=$_pkgvermajor.$_pkgverbuild
pkgrel=1
pkgdesc='Make Minecraft Java Edition mods, Bedrock Edition Add-Ons, and data packs using visual graphical programming or integrated IDE (EAP release)'
arch=('x86_64')
url='https://mcreator.net'
license=('GPL-3.0-or-later WITH GPL-3.0-interface-exception')
noextract=("$pkgname-$pkgver.tar.gz")
source=("$pkgname-$pkgver.tar.gz::https://github.com/${pkgname//-eap}/${pkgname//-eap}/releases/download/$pkgver/MCreator.EAP.$pkgver.Linux.64bit.tar.gz"
        "${pkgname//-eap}.desktop")
b2sums=('dfaafd731e09c649d7552c05adc68645c2173b6dbb0e995eef535e2bead9cafe3583d12bddff0dc2592ee01c351122f04c9b93c8395a0a2fee740fbb92a81f3d'
        'c4227c9cb09a4c0db1fd368f4815d890da16b3bf08552d5bf3766f5ad9706444e5ed42cd591422ddd326e295faa6c302d2144f2c6963598df6cca14537d13248')

prepare() {
    PKGBUILD_JAVA='/usr/lib/jvm/$(archlinux-java status | awk '\''/java-21/{print $NF}'\'')/bin/java'

    mkdir -p "$pkgname-$pkgver/"
    bsdtar -xpf "$pkgname-$pkgver.tar.gz" --strip-components=1 -C "$pkgname-$pkgver/"

    # Convert
    cd "$pkgname-$pkgver/"

    cat >"mcreator.sh"<<EOF
#!/usr/bin/env bash
export CLASSPATH="./$pkgname/lib/mcreator.jar:./lib/*"

cd /opt/$pkgname/
$PKGBUILD_JAVA --add-opens=java.base/java.lang=ALL-UNNAMED net.mcreator.Launcher "\$1"
EOF

    cp "$srcdir/${pkgname//-eap}.desktop" "${pkgname//-eap}.desktop"

    rm -rf jdk/
}

package() {
    depends=('bash'
             'glibc'
             'hicolor-icon-theme'
             'java-runtime=21')

    install -d "$pkgdir/opt/$pkgname/"
    cp -a "$pkgname-$pkgver/." "$pkgdir/opt/$pkgname/"

    chmod 755 "$pkgdir/opt/$pkgname/${pkgname//-eap}.sh"

    install -d "$pkgdir/usr/bin/"
    ln -s "/opt/$pkgname/${pkgname//-eap}.sh" "$pkgdir/usr/bin/$pkgname"

    install -d "$pkgdir/usr/share/applications/"
    ln -s "/opt/$pkgname/${pkgname//-eap}.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"

    install -d "$pkgdir/usr/share/icons/hicolor/64x64/apps/"
    ln -s "/opt/$pkgname/icon.png" "$pkgdir/usr/share/icons/hicolor/64x64/apps/$pkgname.png"

    install -Dm644 "$pkgname-$pkgver/LICENSE.txt" -t "$pkgdir/usr/share/licenses/$pkgname/"
}
