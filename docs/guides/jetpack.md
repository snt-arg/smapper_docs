# Jetson AGX Orin: Flashing Guide for e-con Cameras

This guide details the process of flashing an NVIDIA Jetson AGX Orin development
kit with a custom kernel to support the `e-CAM200_CUOAGX` camera array from e-con Systems.

The official e-con Systems kernel patches are built for JetPack 6.0. This guide uses JetPack 6.2, which provides performance boost for cameras.
We will download the necessary NVIDIA sources, apply the e-con Systems patch, build the kernel, and flash it to the device's NVMe SSD.

## 1. Setup Host Environment 🛠️

First, we'll set up the development environment on your x86_64 host PC running Ubuntu.
All the compilation and flashing operations will happen here.

### 1.1 Export Environment Variables

Open a terminal and run the following commands. These variables will simplify the commands in the subsequent steps.

!!! warning "Keep This Terminal Open"

    All subsequent commands must be run in this same terminal session. If you close it, you'll need to export these variables again.

```bash
# Top-level directory for all our files
export TOP_DIR=$HOME/Downloads/jetpack_orin

# Path to the unpacked e-con Systems driver package
export RELEASE_PACK_DIR=$TOP_DIR/e-CAM200_CUOAGX_JETSON_AGX_ORIN_L4T36.3.0_15-May-2024_R02_RC1

# Path to the unpacked Jetson Linux Driver Package
export L4T_DIR=$TOP_DIR/Linux_for_Tegra

# Path to the target root filesystem
export LDK_ROOTFS_DIR=$TOP_DIR/Linux_for_Tegra/rootfs

# Architecture and cross-compiler toolchain settings
export ARCH=arm64
export CROSS_COMPILE=$TOP_DIR/tool_chain/aarch64--glibc--stable-2022.08-1/bin/aarch64-buildroot-linux-gnu-

# Paths for kernel source and build output
export NVIDIA_SRC=$TOP_DIR/kernel_sources/Linux_for_Tegra/source
export TEGRA_KERNEL_OUT=$NVIDIA_SRC/out/nvidia-linux-header
export KERNEL_HEADERS=$NVIDIA_SRC/kernel/kernel-jammy-src

# Path where kernel modules will be installed
export INSTALL_MOD_PATH=$LDK_ROOTFS_DIR
```

### 1.2 Download Required Files

Now, let's create the directories and download all the necessary packages from NVIDIA.
Bash

```bash
# Create the main directories
mkdir -p $TOP_DIR/tool_chain
mkdir -p $TOP_DIR/kernel_sources


# Links for JetPack 6.0 (L4T r36.3.0)
export TOOL_CHAIN_LINK="https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/toolchain/aarch64--glibc--stable-2022.08-1.tar.bz2"
export L4T_DRIVER_PKG_LINK="https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/release/jetson_linux_r36.3.0_aarch64.tbz2"
export L4T_ROOTFS_LINK="https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/release/tegra_linux_sample-root-filesystem_r36.3.0_aarch64.tbz2"
export DRIVER_PKG_SOURCE_LINK="https://developer.nvidia.com/downloads/embedded/l4t/r36_release_v3.0/sources/public_sources.tbz2"
```

```bash
# Download everything using curl
echo "Downloading required packages..."
curl -L -o $TOP_DIR/tool_chain/glibc.tar.bz2 $TOOL_CHAIN_LINK
curl -L -o $TOP_DIR/Jetson_Linux.tbz2 $L4T_DRIVER_PKG_LINK
curl -L -o $TOP_DIR/Sample_rootfs.tbz2 $L4T_ROOTFS_LINK
curl -L -o $TOP_DIR/kernel_sources/public_sources.tbz2 $DRIVER_PKG_SOURCE_LINK
echo "Downloads complete!"
```

!!! note "e-con Systems Driver Package"

    This guide assumes you have already obtained the camera driver package
    (e.g., e-CAM200_CUOAGX_JETSON_AGX_ORIN_L4T36.3.0_15-May-2024_R02_RC1.tar.gz) from e-con Systems.
    You must download it and place it in the $TOP_DIR directory.

    Example:

    ```bash
    mv ~/Downloads/JP6.0_L4T36.3.0/e-CAM200_CUOAGX_JETSON_AGX_ORIN_L4T36.3.0_15-May-2024_R02_RC1.tar.gz $TOP_DIR
    ```

### 1.3 Extract Files

Unpack all the downloaded archives.

```bash
# Unzip the e-con Systems driver package
# Note: The filename might differ. Adjust if necessary.
cd $TOP_DIR
tar -xaf e- e-CAM200_CUOAGX_JETSON_AGX_ORIN_L4T36.3.0_15-May-2024_R02_RC1.tar.gz

# Extract the toolchain
cd $TOP_DIR/tool_chain
tar -xf glibc.tar.bz2

# Extract Jetson Linux Driver Package and RootFS
cd $TOP_DIR
tar xf Jetson_Linux.tbz2
sudo tar xpf Sample_rootfs.tbz2 -C $LDK_ROOTFS_DIR

# Extract public kernel sources
cd $TOP_DIR/kernel_sources
tar xf public_sources.tbz2

cd $NVIDIA_SRC
tar xf kernel_src.tbz2
tar xf kernel_oot_modules_src.tbz2
tar xf nvidia_kernel_display_driver_source.tbz2
```

### 1.4 Install Required Dependencies

```bash
sudo apt-get update && \
sudo apt-get install qemu-user-static build-essential \
bc lbzip2 flex openssl libssl-dev
```

### 1.5 Prepare Package Ready for Flashing

```bash
cd $L4T_DIR
sudo ./tools/l4t_flash_prerequisites.sh
sudo ./apply_binaries.sh
```

## 2. Patching the Kernel 🩹

With the sources extracted, we apply several patches from the e-con Systems package.
This process involves modifying the driver module, the device tree, and NVIDIA's out-of-tree components.

```bash
# Navigate to the main source directory for all patching operations
cd $NVIDIA_SRC
```

1.  Apply Module Patch: This patch sets up the camera sensor driver to be built out-of-tree.

    ```bash
    cd $NVIDIA_SRC
    mkdir -p sensor_driver
    patch -d sensor_driver -p1 -i $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_module.patch
    ```

2.  Apply Device Tree Patch: Next, we modify and apply the patch for the Device Tree Blob (DTB).

    ```bash
    sed -i 's/vcc-supply/\/\/vcc-supply/g' $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_dtb.patch

    patch -p1 -i $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_dtb.patch
    ```

3.  Apply NVIDIA OOT Patches: Finally, we patch NVIDIA's out-of-tree (OOT) source files. These `sed` commands are necessary workarounds to ensure the patch applies cleanly.

    ```bash

    sed '/@@ -2224,6 +2227,26 @@ static long tegra_channel_default_ioctl/,/tegra_channel_close/d' \
    $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_oot.patch > tempfile && \
    mv $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_oot.patch $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_oot.patch.backup && \
    mv tempfile $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_oot.patch

    patch -p1 -i $RELEASE_PACK_DIR/Kernel/e-CAM200_CUOAGX_JETSON_ORIN_L4T36.3.0_oot.patch
    ```

    !!! warning "Potential Makefile Patch Failure"

        If the last patch command fails with an error like `1 out of 3 hunks FAILED -- saving
        rejects to file Makefile`, you must manually add the following text to the end of the Makefile located in `$NVIDIA_SRC`.

        ```bash
        sensor-driver:
            @if [ ! -d “$(MAKEFILE_DIR)/sensor_driver/$(SENSOR_DRIVER)” ]; then \
                echo “Directory sensor_driver is not found, exiting..”; \
                false; \
            fi

            @echo “================================================================================”
            @echo “make $(MAKECMDGOALS) – sensor driver …”
            @echo “================================================================================”

            @if [ -d “$(MAKEFILE_DIR)/sensor_driver/PWM_MCU” ] && \
            [ -d “$(MAKEFILE_DIR)/sensor_driver/$(SENSOR_DRIVER)” ]; then \
                $(MAKE) -j $(NPROC) ARCH=arm64 \
                    CROSS_COMPILE=$(CROSS_COMPILE) \
                    -C $(NVIDIA_HEADERS) \
                    M=$(MAKEFILE_DIR)/sensor_driver/PWM_MCU \
                    srctree.nvconftest=$(NVIDIA_CONFTEST) \
                    $(MAKECMDGOALS); \
              $(MAKE) -j $(NPROC) ARCH=arm64 \
                  CROSS_COMPILE=$(CROSS_COMPILE) \
                  -C $(NVIDIA_HEADERS) \
                  M=$(MAKEFILE_DIR)/sensor_driver/$(SENSOR_DRIVER) \
                  KBUILD_EXTRA_SYMBOLS=$(MAKEFILE_DIR)/sensor_driver/PWM_MCU/Module.symvers \
                  srctree.nvconftest=$(NVIDIA_CONFTEST) \
                  $(MAKECMDGOALS); \
            else \
                $(MAKE) -j $(NPROC) ARCH=arm64 \
                    CROSS_COMPILE=$(CROSS_COMPILE) \
                    -C $(NVIDIA_HEADERS) \
                    M=$(MAKEFILE_DIR)/sensor_driver/$(SENSOR_DRIVER) \
                    srctree.nvconftest=$(NVIDIA_CONFTEST) \
                    $(MAKECMDGOALS); \
            fi
        ```

4.  Proper support for camera streaming

    ```bash
    sed '238,238d' $NVIDIA_SRC/nvidia-oot/drivers/media/platform/tegra/camera/vi/vi5_fops.c > tempfile && mv tempfile $NVIDIA_SRC/nvidia-oot/drivers/media/platform/tegra/camera/vi/vi5_fops.c
    sed '228i#endif' $NVIDIA_SRC/nvidia-oot/drivers/media/platform/tegra/camera/vi/vi5_fops.c > tempfile && mv tempfile $NVIDIA_SRC/nvidia-oot/drivers/media/platform/tegra/camera/vi/vi5_fops.c
    ```

## 3. Building Kernel 👨‍💻

Now, we will compile the kernel, modules, and device tree from the main source directory.

```bash
# Ensure you are in the main source directory
cd $NVIDIA_SRC

make -C kernel
make modules
sudo -E make modules_install
make dtbs
```

## 4. Flash the Jetson ⚡

The final step is to put the Jetson in recovery mode and flash the entire system.
