# QWRT build

[Русская версия](building.md)

GitHub Actions runs `.github/workflows/build.yml` on pushes to `main` or on a
manual run from **Actions → Build QWRT sysupgrade → Run workflow**. It uploads a
sysupgrade for QWRT's single-rootfs layout, the manifest, and SHA-256 as an
Actions artifact. The workflow does not build a factory image or publish feeds
or GitHub Releases.

This repository carries version 1.3.1 (`8b7c2fb3372a548fd3562ee4f77d2ce9691ba1f3`)
integration files from
[timofey-maykov/be7000-openwrt](https://github.com/timofey-maykov/be7000-openwrt):
`config.buildinfo`, pinned feeds, patches, overlay files, `awg-feed`, and the
LuCI theme. The workflow checks out the OpenWrt tree from the
[kravasuper/openwrt](https://github.com/kravasuper/openwrt) BE7000 port, branch
`xiaomi_be7000`, commit `790d036a178c0f695c2f60d490d194091b35e595`, as
specified by the original project. Both source revisions are recorded in
`source.buildinfo`.

The `profiles/qwrt` profile supplies the DTS, UBI volume, and sysupgrade handler
for the installed QWRT layout. Upstream patches `001`, `003`, and `005` are
skipped: they carry boot arguments/RF defaults for another layout, select a
slot, and add the regulator fix, which is already in the QWRT DTS. The profile
uses its own DTS, UBI-volume, and sysupgrade patches. They write only to MTD
`rootfs`; they do not select `rootfs_1`, change bootloader flags, or attach the
separate `overlay` MTD.

Project release 1.3.1 adds a two-radio 5 GHz mode. The build includes ath12k
patches `308`–`310`, DTS patches `006`–`008`, the board data, and the switching
service. Patch `007` runs after the QWRT DTS patch adds the RF GPIO hogs that it
converts to a controllable pinctrl state. DTS reserves 12 MiB for dual-MAC radio
firmware; single-radio mode remains the default. Patch `009` keeps the usual
`openwrt-` image filename prefix.

Package signature checks remain enabled, but the image omits the upstream
updater and this workflow does not publish a feed. It builds a QWRT-layout image,
not the upstream two-slot update flow.

Before building, the workflow fixes the hunk count in patch `307`, already
present in the pinned Krava port (`790d036a`): its header claims 7/6 lines while
the actual hunk contains 6/5. The correction lets mac80211 apply its existing
patch queue before the new `308`–`310` patches.

After a successful run, download `xiaomi-be7000-qwrt-squashfs-sysupgrade.bin`
from the artifact and verify it against `sha256sums.txt`. Use it through QWRT's
normal upgrade page only on a device with this partition layout.
