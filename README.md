# ncos-quicksettings-applet

Packaging for [Quick Settings](https://github.com/DrakRosmann/cosmic-ext-applet-quick-settings)
(`cosmic-ext-applet-quick-settings`) — a COSMIC panel applet with Wi-Fi,
Bluetooth, volume, brightness and more in one place — as the
`ncos-quicksettings-applet` pacman package for the
[ncos repo](https://github.com/exothermic88/ncos-repo).

This repo holds only the PKGBUILD and a patch: the source is downloaded from a
pinned upstream commit (`_commit`), since upstream's only tag (v0.1.0) is far
behind its main branch.

## Patches

- `tooltip-under-popup.patch` — hides a panel tooltip when the popup opens,
  and drops a tooltip whose delayed show lands after the popup opened (showing
  it would close the popup).

## Build

```sh
makepkg -srf
```

## Update to a newer upstream commit

1. Set `_commit` to the new full commit hash.
2. Set `pkgver` to `<Cargo.toml version>.r<commits since v0.1.0>.g<short hash>`
   (e.g. `git describe --long --tags` → `v0.1.0-21-g08aa376`) and reset `pkgrel=1`.
3. Check the patch still applies (drop it if upstream fixed this).
4. `updpkgsums`
5. `makepkg -srf`

## Publish to the ncos repo

```sh
cp ncos-quicksettings-applet-<ver>-<rel>-x86_64.pkg.tar.zst ~/claude_projects/ncos-repo/x86_64/
cd ~/claude_projects/ncos-repo
./rename   # repo-add + db rename
git add . && git commit -m "Add ncos-quicksettings-applet <ver>-<rel>"
./push
```

Do not ship the `-debug` package.
