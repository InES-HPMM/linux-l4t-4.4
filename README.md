# linux-l4t-4.4

This is the top-level repository for TX1/TX2 **kernel** and **Device Tree**. The Nvidia Source URLs can be found in the *README.md* files of the submodules.

## Clone with Submodules

Use the following command to clone this repository and its submodules:

```shell
$ git clone --recursive <URL>
```

## Already Cloned but no Submodules?

```shell
$ git submodule update --init --recursive
```

## Building the Kernel

```shell
# Go to the kernel-4.4 directory
$ cd kernel/kernel-4.4

# Set the environment variables
$ export ARCH=arm64
$ export CROSS_COMPILE=<aarch64_toolchain_prefix>

# TX1
$ make tegra21_hdmi2csi_defconfig
# TX2
$ make tegra18_defconfig

# Build the kernel, modules and dtb files
$ make -j8
```

## Device Tree

The Device Tree source files of the Linux kernel are located in the *hardware* directory:

**TX1**:

`hardware/nvidia/platform/t210/jetson/kernel-dts/`

**TX2**:

`hardware/nvidia/platform/t18x/quill/kernel-dts/`

For compiling the Device Trees, go to the kernel directory and run make:

```shell
$ cd kernel/kernel-4.4
$ make dtbs
```

### Update Device-Tree

**CAUTION**:

Do **not** use `extlinux.conf` to define the generated `.dtb` file because *u-boot* makes changes on-the-fly to the Device Tree Blob. Otherwise, the screen might flicker or the kernel stops during the boot process.

Instread, use the flash script `flash.sh` from `jetpack`:

* [Extlinux: Jetson DTB](https://elinux.org/Jetson/TX2_DTB)

**TX1**:
1. Replace the existing `.dtb` file: `JetPack/3.1/64_TX1/Linux_for_Tegra_64_tx1/kernel/dtb/tegra210-jetson-tx1-p2597-2180-a01-devkit.dtb` with `tegra210-jetson-tx1-p2597-2180-a01-devkit-hdmi2csi.dtb`
2. Flash the DTB partition: `$ sudo ./flash.sh -r -k DTB jetson-tx1 mmcblk0p1`

**TX2**:
1. Replace the existing `.dtb` file: `JetPack/3.1/64_TX2/Linux_for_Tegra_64_tx2/kernel/dtb/tegra186-quill-p3310-1000-c03-00-base.dtb`
2. Flash the DTB partition: `$ sudo ./flash.sh -r -k kernel-dtb jetson-tx2 mmcblk0p1`
