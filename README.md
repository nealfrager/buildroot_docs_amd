# Buildroot

> Source: [AMD Adaptive Computing Wiki – Buildroot](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/2804187327/Buildroot)

This page highlights AMD support for Buildroot as an alternative community Linux solution to Yocto. Unofficial support for AMD architectures (Zynq, Zynq MPSoC, Kria SOMs and Versal Adaptive SoC) are available. The Buildroot configs directory provides a number of example `defconfig` files for common evaluation boards which use AMD devices.

Buildroot is not WTS supported and should only be used by experienced users. However, Buildroot does use the same supported software as Yocto from the `https://www.github.com/Xilinx` repository. Any issues with this underlying software which can be duplicated using Yocto or Vitis are supported by WTS. But any issues with configuring Buildroot for custom hardware should be addressed to the Buildroot community.

One exception to the above is the Zynq and Zynq MPSoC U-Boot Secondary Program Loader (SPL). While it is included with U-Boot and Buildroot, the WTS supported flow for the Zynq and Zynq MPSoC families is the First Stage Bootloader (FSBL). Experienced users are welcome to give the U-Boot SPL a try as it is [documented here](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18842574). However, if technical support is needed, the Xilinx recommendation is to build the `boot.bin` using the FSBL and `bootgen`. It is possible to use Yocto or Vitis for building a hardware specific `boot.bin` while still using Buildroot for building the Linux kernel and file system.

---

# Table of Contents

- [General Information](#general-information)
- [Supported AMD Evaluation Boards](#supported-amd-evaluation-boards)
  - [Zynq](#zynq)
  - [Zynq UltraScale+](#zynq-ultrascale)
  - [Kria SOM](#kria-som)
  - [Versal](#versal)
  - [Versal Gen 2](#versal-gen-2)
- [Getting Started with Buildroot](#getting-started-with-buildroot)
  - [Host System Requirements](#host-system-requirements)
  - [Build Steps](#build-steps)
- [Buildroot Custom Hardware Device Tree](#buildroot-custom-hardware-device-tree)
  - [ZCU102 Example with Custom PL Bitstream](#zcu102-example-with-custom-pl-bitstream)
  - [VEK280 Example with Custom-pldpdi](#vek280-example-with-custom-pldpdi)
  - [VEK385 Example with Custom-pldpdi](#vek385-example-with-custom-pldpdi)
- [Reference Resources](#reference-resources)

---

# General Information

Official Buildroot release activity and version information can be found at:

- [Releases - Buildroot](https://buildroot.org/download.html)

The following table correlates Buildroot LTS releases to the corresponding AMD releases.

| AMD Release Tag | Buildroot LTS Release | Linux Kernel LTS |
| --- | --- | --- |
| `xilinx_v2024.2` | `2025.02` | `6.6` |
| `xilinx_v2023.2` | `2024.02` | `6.1` |
| `xilinx_v2022.2` | `2023.02` | `5.15` |

Starting with the `2025.02` release, Buildroot LTS releases are maintained for 3 years and every other year will have an LTS release. Because Buildroot LTS releases happen in February, they will always be based on the prior year’s AMD Adaptive `.2` release. For example, Buildroot `2025.02` is based on the `xilinx_v2024.2` AMD release.

To test the latest AMD release before the next Buildroot February LTS release, it is possible to clone the Buildroot master branch which already includes the latest AMD release.

```bash
git clone https://gitlab.com/buildroot.org/buildroot.git
```

---

# Supported AMD Evaluation Boards

## Zynq

- [`zynq_zc702_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zc702_defconfig)
- [`zynq_zc706_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zc706_defconfig)
- [`zynq_microzed_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_microzed_defconfig)
- [`zynq_zed_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zed_defconfig)

## Zynq UltraScale+

- [`zynqmp_zcu102_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu102_defconfig)
- [`zynqmp_zcu104_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu104_defconfig)
- [`zynqmp_zcu106_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu106_defconfig)

## Kria SOM

- [`zynqmp_kria_kd240_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kd240_defconfig)
- [`zynqmp_kria_kr260_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kr260_defconfig)
- [`zynqmp_kria_kv260_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kv260_defconfig)

.## Versal

- [`versal_vck190_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vck190_defconfig)
- [`versal_vek280_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vek280_defconfig)
- [`versal_vpk120_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vpk120_defconfig)
- [`versal_vpk180_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vpk180_defconfig)

## Versal Gen 2

- [`versal2_vek385_defconfig`](https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal2_vek385_defconfig)

---

# Getting Started with Buildroot

This page provides a step-by-step guide to configuring Buildroot to generate an embedded Linux system using the AMD Linux release. Now that many `defconfig` examples have been added to Buildroot mainline, the latest AMD software will build and run on a variety of evaluation boards with very little effort.

## Host System Requirements

Buildroot documents the requirements for the host Linux development machine here:

- [Buildroot Manual](https://buildroot.org/downloads/manual/manual.html)

## Build Steps

### 1. Download the Latest LTS Version of Buildroot

- [Buildroot - Making Embedded Linux Easy](https://buildroot.org/)

### 2. Review the Buildroot Directory Structure

Buildroot includes a well-organized directory structure documented in the official documentation.

### 3. Inspect the Following Directories

#### `configs`

Contains build examples for AMD evaluation boards that can be used as templates for creating custom target board configurations.

#### `board`

Contains a `readme.txt` file and open-source build scripts for each AMD evaluation board family.

The `post-image.sh` script operates on the `genimage.cfg` file for creation of the SD card image for booting.

Example board directories:

```text
board/zynq
board/zynqmp
board/zynqmp/kria
board/versal
board/versal2
```

### 4. Configure Buildroot for an AMD Evaluation Board

Example for the AMD ZCU102:

```bash
make zynqmp_zcu102_defconfig
```

### 5. Build the Target Images

```bash
make
```

### 6. Inspect the Output Directories

#### `output/build`

Contains the sources of all packages that were built.

#### `output/images`

Contains all target images that will run on the target board, including `sdcard.img`.

Example layout:

```text
output/images/
+-- atf-uboot.ub
+-- bl31.bin
+-- boot.bin
+-- boot.vfat
+-- Image
+-- rootfs.ext2
+-- rootfs.ext4 -> rootfs.ext2
+-- sdcard.img
+-- system.dtb -> zynqmp-zcu102-rev1.0.dtb
+-- u-boot.itb
`-- zynqmp-zcu102-rev1.0.dtb
```

### 7. Image the SD Card for Booting

```bash
dd if=output/images/sdcard.img of=/dev/sdX
```

Where `sdX` is the device node of the SD card on the host.

### 8. Configure the Target Evaluation Board for SD Card Boot Mode

Set the board boot switches appropriately for SD card boot.

### 9. Login Credentials

Default root password:

```text
root
```

Example login:

```text
Welcome to Buildroot
buildroot login:
```

---

# Buildroot Custom Hardware Device Tree

Buildroot expects custom hardware specifications to already be in Device Tree format. To create a device tree for custom hardware, it is recommended to use the supported Software Hardware Exchange Loop which is the same flow used by Yocto.

---

# ZCU102 Example with Custom PL Bitstream

## 1. SDTGen

Convert Vivado XSA file (`system.xsa`) to system device tree files.

```bash
sdtgen -xsa zcu102.xsa -dir sdt_out -board_dts zynqmp-zcu102-rev1.0
```

## 2. Lopper

Strip out `zynqmp_linux.dts` and `pl.dtsi` files from system device tree files.

```bash
export LOPPER_DTC_FLAGS="-b 0 -@"
git clone -b xilinx_v2025.2 https://github.com/Xilinx/lopper.git
mkdir -p ./lopper_out
lopper -f --enhanced ./sdt_out/system-top.dts ./lopper_out/system.dts -- xlnx_overlay_dt cortexa53-zynqmp full
lopper -f --enhanced -O ./lopper_out -i ./lopper/lopper/lops/lop-a53-imux.dts ./lopper_out/system.dts zynqmp_linux.dts -- gen_domain_dts psu_cortexa53_0 linux_dt
```

## 3. Buildroot

Change `zynqmp_zcu102_defconfig` to use the Lopper-generated `zynqmp_linux.dts` instead of the in-tree DTS.

## 4. Create External DTS Directory

```bash
mkdir -p ./buildroot_dts
mkdir -p ./buildroot_dts/xilinx
cp ./lopper_out/zynqmp_linux.dts ./buildroot_dts/xilinx
```

## 5. Modify Buildroot Defconfig

Modify:

```text
<buildroot_dir>/configs/zynqmp_zcu102_defconfig
```

Add or update:

```diff
+BR2_LINUX_KERNEL_CUSTOM_DTS_PATH="./buildroot_dts"
-BR2_LINUX_KERNEL_INTREE_DTS_NAME="xilinx/zynqmp-zcu102-rev1.0"
```

## 6. Rebuild Buildroot

```bash
make
```

## 7. Image the SD Card

```bash
dd if=output/images/sdcard.img of=/dev/sdX
```

Where `sdX` is the device node of the SD card on the host.

## 8. Boot the Board from SD Card

Configure the ZCU102 boot mode switches for SD card boot and power on the board.  Login as root.

```text
root
```

```text
Welcome to Buildroot
buildroot login:
```

## 9. Load the FPGA Bitstream at Runtime

After Linux boots, load the FPGA bitstream using `fpgautil`:

```bash
fpgautil -b /lib/firmware/xilinx/zcu102.bit.bin -o /lib/firmware/xilinx/pl.dtbo
```

---

# VEK280 Example with Custom pld.pdi

## 1. SDTGen

```bash
sdtgen -xsa vek280.xsa -dir sdt_out -board_dts versal-vek280-revb
```

## 2. Lopper

```bash
export LOPPER_DTC_FLAGS="-b 0 -@"
git clone -b xilinx_v2025.2 https://github.com/Xilinx/lopper.git
mkdir -p ./lopper_out
lopper -f --enhanced ./sdt_out/system-top.dts ./lopper_out/system.dts -- xlnx_overlay_dt cortexa72-versal full
lopper -f --enhanced -O ./lopper_out -i ./lopper/lopper/lops/lop-a72-imux.dts ./lopper_out/system.dts versal_linux.dts -- gen_domain_dts psv_cortexa72_0 linux_dt
```

## 3. Buildroot

Change `versal_vek280_defconfig` to use the Lopper-generated `versal_linux.dts`.

## 4. Create External DTS Directory

```bash
mkdir -p ./buildroot_dts
mkdir -p ./buildroot_dts/xilinx
cp ./lopper_out/versal_linux.dts ./buildroot_dts/xilinx
```

## 5. Modify Buildroot Defconfig

Modify:

```text
<buildroot_dir>/configs/versal_vek280_defconfig
```

Update:

```diff
+BR2_LINUX_KERNEL_CUSTOM_DTS_PATH="./buildroot_dts"
-BR2_LINUX_KERNEL_INTREE_DTS_NAME="xilinx/versal-vek280-revB"
```

## 6. Rebuild Buildroot

```bash
make
```

## 7. Image the SD Card

```bash
dd if=output/images/sdcard.img of=/dev/sdX
```

Where `sdX` is the device node of the SD card on the host.

## 8. Boot the Board from SD Card

Configure the VEK280 boot mode switches for SD card boot and power on the board.  Login as root.

```text
root
```

```text
Welcome to Buildroot
buildroot login:
```

## 9. Load the PLD Image at Runtime

After Linux boots, load the programmable logic image using `fpgautil`:

```bash
fpgautil -b /lib/firmware/xilinx/vek280_pld.pdi -o /lib/firmware/xilinx/pl.dtbo
```

---

# VEK385 Example with Custom pld.pdi

## 1. SDTGen

```bash
sdtgen -xsa vek385.xsa -dir sdt_out -board_dts versal2-vek385-revb
```

## 2. Lopper

```bash
export LOPPER_DTC_FLAGS="-b 0 -@"
git clone -b xilinx_v2025.2 https://github.com/Xilinx/lopper.git
mkdir -p ./lopper_out
lopper -f --enhanced ./sdt_out/system-top.dts ./lopper_out/system.dts -- xlnx_overlay_dt cortexa78-versal2 full
lopper -f --enhanced -O ./lopper_out -i ./lopper/lopper/lops/lop-a78-imux.dts ./lopper_out/system.dts versal2_linux.dts -- gen_domain_dts cortexa78_0 linux_dt
```

## 3. Buildroot

Change `versal2_vek385_defconfig` to use the Lopper-generated `versal2_linux.dts`.

## 4. Create External DTS Directory

```bash
mkdir -p ./buildroot_dts
mkdir -p ./buildroot_dts/xilinx
cp ./lopper_out/versal2_linux.dts ./buildroot_dts/xilinx
```

## 5. Modify Buildroot Defconfig

Modify:

```text
<buildroot_dir>/configs/versal2_vek385_defconfig
```

Update:

```diff
+BR2_LINUX_KERNEL_DTS_SUPPORT=y
+BR2_LINUX_KERNEL_CUSTOM_DTS_PATH="./buildroot_dts"
```

## 6. Rebuild Buildroot

```bash
make
```

## 7. Image the SD Card

```bash
dd if=output/images/sdcard.img of=/dev/sdX
```

Where `sdX` is the device node of the SD card on the host.

## 8. Boot the Board from SD Card

Configure the VEK385 boot mode switches for SD card boot and power on the board.  Login as root.

```text
root
```

```text
Welcome to Buildroot
buildroot login:
```

## 9. Load the PLD Image at Runtime

After Linux boots, load the programmable logic image using `fpgautil`:

```bash
fpgautil -b /lib/firmware/xilinx/vek385_pld.pdi -o /lib/firmware/xilinx/pl.dtbo
```

---

# Reference Resources

- [Buildroot User Manual](https://buildroot.org/downloads/manual/manual.html)
- [Buildroot Official Website](https://buildroot.org/)
- [Xilinx GitHub](https://github.com/Xilinx)
- [Lopper Repository](https://github.com/Xilinx/lopper)