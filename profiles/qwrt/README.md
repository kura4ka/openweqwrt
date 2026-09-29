# QWRT single-rootfs sysupgrade profile

This profile builds a `sysupgrade.bin` for Xiaomi BE7000 units currently running
QWRT with one `rootfs` MTD partition and UBI volumes `kernel`, `rootfs`, and
`rootfs_data`.

The image keeps the existing QWRT MTD map. It writes only `rootfs`, names the
SquashFS UBI volume `rootfs`, and leaves the separate `overlay` MTD unattached
and untouched. It does not use the project's two-slot boot flags or install
the updater that follows the project's shared GitHub latest release.

Package signature verification stays enabled. The image includes the public
key from the matching upstream build so the existing signed package feeds can
still be checked. This repository does not publish a package feed and never
stores or requests the corresponding private signing key.

Project inputs follow Timofey's v1.3.1 release, including the optional two-radio
5 GHz mode. That mode reserves a 12 MiB memory region for the dual-MAC radio
firmware, reducing memory available to Linux by 12 MiB. The regular single-radio
mode remains the default.

Upload the artifact file `xiaomi-be7000-qwrt-squashfs-sysupgrade.bin` through
QWRT's firmware upgrade page. Let QWRT keep its configuration backup enabled.
The upgrade replaces the UBI contents of the 80 MiB `rootfs` partition and
recreates `rootfs_data`; files that are not part of the saved configuration
archive, installed packages, and application data can be lost. Do not use a
factory image or a stock two-slot installation script for this profile.

The workflow also includes the build manifest and SHA-256 file. A successful
`sysupgrade -T` checks the image container and board metadata; it does not by
itself verify that the new kernel boots on a particular device.
