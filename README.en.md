# Xiaomi BE7000 QWRT sysupgrade

[Русская версия](README.md)

This repository builds a `sysupgrade.bin` for a Xiaomi BE7000 that already runs
QWRT with one `rootfs` MTD partition and UBI volumes `kernel`, `rootfs`, and
`rootfs_data`. The image leaves the separate `overlay` MTD untouched.

Its project inputs come from
[timofey-maykov/be7000-openwrt](https://github.com/timofey-maykov/be7000-openwrt)
release 1.3.1 (`8b7c2fb`): build configuration, feeds, patches, and overlay
files. Exact source revisions are in [source.buildinfo](source.buildinfo). That
project builds the OpenWrt tree from the
[kravasuper/openwrt](https://github.com/kravasuper/openwrt) BE7000 port at
commit `790d036a`, then applies its project files. This repository adds the
QWRT profile to that build. It keeps the 1.3.1 5 GHz dual-radio mode, which
reserves 12 MiB of RAM.

The workflow builds only the sysupgrade image; it does not build a factory
image or publish a package feed or GitHub release. Download
`xiaomi-be7000-qwrt-squashfs-sysupgrade.bin` from a successful GitHub Actions
artifact and verify its SHA-256 using `sha256sums.txt`. The artifact is retained
for 14 days.

Upload the image through QWRT's firmware upgrade page and keep configuration
backup enabled. The upgrade replaces the contents of the 80 MiB `rootfs` MTD
partition and recreates `rootfs_data`; packages and application data outside the
configuration archive can be lost. Use this image only with the stated QWRT
layout. Do not use it on the stock two-slot layout or use a factory image.
