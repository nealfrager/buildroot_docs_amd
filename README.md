# Buildroot

This page highlights AMD support for Buildroot as an alternative community Linux solution to Yocto.

Unofficial support for AMD architectures (Zynq, Zynq MPSoC, Kria SOMs, and Versal Adaptive SoC) are available. The Buildroot `configs/` directory provides a number of example `defconfig` files for common evaluation boards using AMD devices.

> [!WARNING]
> Buildroot is not WTS supported and should only be used by experienced users.
>
> However, Buildroot uses the same supported software as Yocto from the Xilinx repositories. Any issues with underlying software that can be reproduced using Yocto or Vitis are supported by WTS.
>
> Issues related to configuring Buildroot for custom hardware should be directed to the Buildroot community.

> [!NOTE]
> For Zynq and Zynq MPSoC platforms, the recommended supported boot flow uses FSBL + bootgen rather than the U-Boot SPL flow.

---

# Table of Contents

- [General Information](#general-information)
- [Supported AMD Evaluation Boards](#supported-amd-evaluation-boards)
  - [Zynq](#zynq)
  - [Zynq-UltraScale](#zynq-ultrascale)
  - [Kria SOM](#kria-som)
  - [Versal](#versal)
  - [Versal Gen 2](#versal-gen-2)
- [Getting Started with Buildroot](#getting-started-with-buildroot)
  - [Host System Requirements](#host-system-requirements)
  - [Build Steps](#build-steps)
- [Buildroot Custom Hardware Device Tree](#buildroot-custom-hardware-device-tree)
- [Reference Resources](#reference-resources)

---

# General Information

Official Buildroot release activity and version information can be found at:

- https://buildroot.org/

The following table correlates Buildroot LTS releases with AMD releases.

| AMD Release Tag | Buildroot LTS Release | Linux Kernel LTS |
|---|---|---|
| `xilinx_v2024.2` | `2025.02` | `6.6` |
| `xilinx_v2023.2` | `2024.02` | `6.1` |
| `xilinx_v2022.2` | `2023.02` | `5.15` |

To test newer AMD releases before the next Buildroot LTS release:

```bash
git clone https://gitlab.com/buildroot.org/buildroot.git
