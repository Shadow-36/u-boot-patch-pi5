# U-Boot for Raspberry Pi 5 — UQTR patch

**Course:** GEI-1089 — Systèmes d’exploitation embarqués  
**Author / adaptation:** Ahmed Khelifi, ing. — Université du Québec à Trois-Rivières (UQTR)  
**Target:** Raspberry Pi 5 (AArch64)  
**Upstream U-Boot base:** `10adf52ed29d31c72fa06c1d9ee311d47c537215` (October 2026 snapshot)

## Purpose

`RPi5-U-Boot-2026.10-UQTR.patch` adapts upstream U-Boot for our Raspberry Pi 5 embedded-Linux lab. It adds or adjusts:

- **RP1 Ethernet support:** RP1 device probing and clock/GPIO integration, with changes to the MACB network driver.
- **Local interaction:** HDMI video console and USB keyboard support, including Pi 5 startup diagnostics.
- **Board configuration:** Pi 5 settings in `rpi_arm64_defconfig`, including 16-bit video, PCI/USB initialization, and an interactive U-Boot prompt.

The resulting U-Boot is intended to let students load their own **Linux kernel, Device Tree and initramfs** over TFTP and boot them from the U-Boot shell. Those Linux artifacts and the student's `boot.scr` **are not included** in this patch.

## Apply and build

Install a suitable AArch64 cross-compiler and the usual U-Boot build dependencies first. Then run:

```bash
git clone https://github.com/u-boot/u-boot.git
cd u-boot
git checkout 10adf52ed29d31c72fa06c1d9ee311d47c537215

# Download the .patch from this repository, then use its actual path:
git apply --check /path/to/RPi5-U-Boot-2026.10-UQTR.patch
git apply /path/to/RPi5-U-Boot-2026.10-UQTR.patch

make O=build-rpi5 CROSS_COMPILE=aarch64-linux-gnu- rpi_arm64_defconfig
make O=build-rpi5 CROSS_COMPILE=aarch64-linux-gnu- -j"$(nproc)" u-boot.bin
```

**Output:** `build-rpi5/u-boot.bin`. Raspberry Pi firmware/boot-media setup and the Linux/TFTP assets must be provided separately.

> **Important:** Apply this patch to the pinned commit above, not an arbitrary U-Boot release. This is a teaching/research adaptation, not an official upstream Raspberry Pi 5 U-Boot release. The combined patch has not been independently hardware-retested as a single artifact; verify it on the lab equipment before distribution.

## Credits and licensing

Adaptation and lab integration: **Ahmed Khelifi, ing. (UQTR)**. The patch includes code based on existing U-Boot and RP1-related contributions. Original copyright notices, SPDX identifiers and third-party contributor credits remain in the source files. This README's authorship does not supersede those notices.
