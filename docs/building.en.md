# QWRT build

[Русская версия](building.md)

GitHub Actions runs `.github/workflows/build.yml` on pushes to `main` or on a
manual run from **Actions → Build QWRT sysupgrade → Run workflow**. It uploads a
sysupgrade for QWRT's single-rootfs layout, the manifest, and SHA-256 as an
Actions artifact. The workflow does not build a factory image or publish feeds
or GitHub Releases.

This repository carries the integration layer from
[timofey-maykov/be7000-openwrt](https://github.com/timofey-maykov/be7000-openwrt):
`config.buildinfo`, pinned feeds, patches, overlay files, `awg-feed`, and the
LuCI theme. The workflow checks out the OpenWrt tree from the
[kravasuper/openwrt](https://github.com/kravasuper/openwrt) BE7000 port, branch
`xiaomi_be7000`, commit `790d036a`, as specified by the original project.

The `profiles/qwrt` profile replaces the DTS, UBI volume, and sysupgrade handler
for the installed QWRT layout. It does not select `rootfs_1`, change bootloader
flags, or attach the separate `overlay` MTD. The project's shared driver,
kernel-config, and package patches remain in the build; the QWRT DTS profile
includes the regulator fix from patch `005` so applying it does not depend on the
two-slot DTS.

After a successful run, download `xiaomi-be7000-qwrt-squashfs-sysupgrade.bin`
from the artifact and verify it against `sha256sums.txt`. Use it through QWRT's
normal upgrade page only on a device with this partition layout.
