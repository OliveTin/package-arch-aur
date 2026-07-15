# package-arch-aur

This is the Arch Linux AUR package for OliveTin.

Find out more at https://olivetin.app

## Package layout

This package builds the OliveTin binary from the matching Git release tag, and
takes the prebuilt webui from the GitHub release archive
`OliveTin-linux-amd64.tar.gz` (so Node/npm is not needed at build time).

## Updating to a new release

1. Set `pkgver` to the new OliveTin release (for example `3000.17.0`) and reset
   `pkgrel` to `1` in `PKGBUILD`.
2. Refresh the release archive checksum:

   ```bash
   curl -sL "https://github.com/OliveTin/OliveTin/releases/download/${pkgver}/OliveTin-linux-amd64.tar.gz" \
     | sha256sum
   ```

   Put the resulting hash in the second `sha256sums` entry (the git tag source
   stays as `SKIP`).
3. Check whether `var/systemd/OliveTin.service` still uses
   `/usr/local/bin/OliveTin`. If that path or file moved, update
   `systemd-unit-usr-bin.patch` and its checksum.
4. Regenerate metadata:

   ```bash
   make srcinfo
   ```
5. Build/test on Arch if possible:

   ```bash
   make clean
   makepkg -f
   ```
6. Commit, push to GitHub (`origin`), then push to the AUR:

   ```bash
   git push origin master
   git push aur master
   ```
