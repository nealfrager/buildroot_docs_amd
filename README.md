# buildroot_docs_amd
Buildroot documentation for use with AMD (Xilinx) products

This page highlights AMD support for Buildroot as an alternative community
Linux solution to Yocto.  Unofficial support for AMD architectures (Zynq, Zynq
MPSoC, Kria SOMs and Versal Adaptive SoC) are available. The Buildroot configs
directory provides a number of example defconfig files for common evaluation
boards which use AMD devices.

Table of Contents
    General Information
    Supported AMD Evaluation Boards
        Zynq
        Zynq UltraScale+
        Kria SOM
        Versal
        Versal Gen 2
    Getting Started with Buildroot
        Host System Requirements
        Build Steps
    Buildroot Custom Hardware Device Tree
        ZCU102 example with custom PL bitstream
        VEK280 example with custom pld.pdi
        VEK385 example with custom pld.pdi
    Reference Resources

General Information

Official Buildroot release activity and version information can be found at
https://buildroot.org/download.html

The following table correlates Buildroot LTS releases to the corresponding AMD
releases. Because Buildroot LTS releases happen in February of each year, they
will always be based on the prior year’s AMD Adaptive .2 release.  For example,
Buildroot 2025.02 is based on the xilinx_v2024.2 AMD Adaptive SoC and FPGA
release.

AMD Release Tag     Buildroot LTS Release     Linux Kernel LTS
xilinx_v2025.2      2026.02                   6.12
xilinx_v2024.2      2025.02                   6.6
xilinx_v2023.2      2024.02                   6.1
xilinx_v2022.2      2023.02                   5.15

Note: To test AMD release xilinx_v2025.2 before the Buildroot February 2026 LTS
release, it is possible to clone the Buildroot master branch which already
includes AMD release xilinx_v2025.2.

git clone https://gitlab.com/buildroot.org/buildroot.git

Supported AMD Evaluation Boards

Zynq
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zc702_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zc706_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_microzed_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynq_zed_defconfig

Zynq UltraScale+
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu102_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu104_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_zcu106_defconfig

Kria SOM
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kd240_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kr260_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/zynqmp_kria_kv260_defconfig

Versal
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vck190_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vek280_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vpk120_defconfig
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal_vpk180_defconfig

Versal Gen2
  https://gitlab.com/buildroot.org/buildroot/-/blob/master/configs/versal2_vek385_defconfig

Getting Started with Buildroot

This page provide a step by step guide to configuring buildroot to generate an
embedded Linux system using the AMD Linux release.  Now that many defconfig
examples have been added to buildroot mainline, the latest AMD software will
now build and run on a variety of evaluation boards with very little effort.

Host System Requirements

Buildroot documents the requirements for the host Linux development machine:
https://buildroot.org/downloads/manual/manual.html#requirement
