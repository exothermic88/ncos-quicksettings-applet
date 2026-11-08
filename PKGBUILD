# Maintainer: nic <nicolasciampa2001@gmail.com>
# Personal-repo PKGBUILD: builds the upstream "Quick Settings" COSMIC applet
# from a pinned commit (upstream's only tag, v0.1.0, is far behind), plus a
# local tooltip fix. Bump _commit/pkgver + updpkgsums to update.
pkgname=ncos-quicksettings-applet
_upstream=cosmic-ext-applet-quick-settings
_appid=io.github.DrakRosmann.CosmicExtAppletQuickSettings
_commit=08aa376f10f7207b050c2e7a9642d20ec52bd439
pkgver=0.1.1.r21.g08aa376
pkgrel=1
pkgdesc="COSMIC panel applet with Wi-Fi, Bluetooth, volume, brightness and more in one place (Quick Settings)"
arch=('x86_64' 'aarch64')
url="https://github.com/DrakRosmann/cosmic-ext-applet-quick-settings"
license=('GPL-3.0-only')
# wayland: libwayland-client is dlopen'd at runtime. The rest are programs the
# applet runs: wpctl (wireplumber), pw-dump (pipewire), pactl (libpulse),
# curl, loginctl/systemctl (systemd).
depends=('libxkbcommon' 'wayland' 'wireplumber' 'pipewire' 'libpulse' 'curl' 'systemd')
makedepends=('cargo' 'git' 'pkgconf') # git: libcosmic is a cargo git dependency
provides=('cosmic-ext-applet-quick-settings')
conflicts=('cosmic-ext-applet-quick-settings')
options=('!lto') # cargo already does its own LTO; avoid makepkg injecting -flto
source=("$_upstream-$_commit.tar.gz::$url/archive/$_commit.tar.gz"
        'tooltip-under-popup.patch')
sha256sums=('ad697dd75c6784b9dd92b3a0f8d08abedeb35a2c5408aa1c5435c90981d23a2f'
            '06abd9d71b01533410415e4152eb620c383fd22e16793febc81a62b31fc95ff7')

prepare() {
    cd "$_upstream-$_commit"
    # Drop a tooltip that would open over (and so close) the popup.
    patch -Np1 -i ../tooltip-under-popup.patch
    cargo fetch --locked
}

build() {
    cd "$_upstream-$_commit"
    cargo build --release --frozen
}

package() {
    cd "$_upstream-$_commit"
    # Mirrors upstream install.sh with prefix=/usr.
    install -Dm755 "target/release/$_upstream" "$pkgdir/usr/bin/$_upstream"
    install -dm755 "$pkgdir/usr/share/applications"
    sed "s|@bindir@|/usr/bin|" "data/$_appid.desktop.in" \
        > "$pkgdir/usr/share/applications/$_appid.desktop"
    install -Dm644 "data/$_appid.metainfo.xml" \
        "$pkgdir/usr/share/metainfo/$_appid.metainfo.xml"
    install -Dm644 "data/icons/scalable/apps/$_appid.svg" \
        "$pkgdir/usr/share/icons/hicolor/scalable/apps/$_appid.svg"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
