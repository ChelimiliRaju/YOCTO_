# weston

# 🖼️ Yocto Weston Build for Raspberry Pi 4 (64-bit)

This markdown documents the complete step-by-step process to build the `core-image-weston` image for Raspberry Pi 4 (64-bit) using the Yocto Project (Kirkstone release).

## 📁 Folder Structure Used
```bash
~/Desktop/yocto/poky-rpi
```

## 🧱 Build Environment Setup

### 1. Clone poky (Kirkstone)
```bash
git clone -b kirkstone git://git.yoctoproject.org/poky
cd poky
```

### 2. Clone Required Layers
```bash
git clone -b kirkstone git://git.openembedded.org/meta-openembedded

git clone -b kirkstone https://github.com/agherzan/meta-raspberrypi.git
```

### 3. Source Environment
```bash
source oe-init-build-env build
```

## ⚙️ Configuration

### 4. Configure bblayers.conf
Edit `build/conf/bblayers.conf`:
```bash
BBLAYERS ?= " \
  ${TOPDIR}/../poky/meta \
  ${TOPDIR}/../poky/meta-poky \
  ${TOPDIR}/../poky/meta-yocto-bsp \
  ${TOPDIR}/../meta-openembedded/meta-oe \
  ${TOPDIR}/../meta-raspberrypi \
"
```

### 5. Configure local.conf
Edit `build/conf/local.conf`:
```bash
MACHINE = "raspberrypi4-64"

# Weston specific features
DISTRO_FEATURES:append = " wayland x11 pam opengl "
IMAGE_INSTALL:append = " weston weston-examples "

# Common required settings
ENABLE_UART = "1"
GPU_MEM = "128"
```

## 🔨 Build Command
```bash
bitbake core-image-weston
```

This will build the Weston image and generate files like:
```
/tmp/deploy/images/raspberrypi4-64/core-image-weston-raspberrypi4-64.wic.bz2
```

You can flash this `.wic.bz2` file to an SD card.

---

## ✅ Additional Packages Compared to Minimal

The following are extra components included in Weston image compared to Minimal:

| Feature | Included By | Purpose |
|--------|-------------|---------|
| `weston` | `IMAGE_INSTALL` | Wayland compositor (GUI display) |
| `weston-examples` | `IMAGE_INSTALL` | Sample Weston applications |
| `wayland` | `DISTRO_FEATURES` | Wayland protocol support |
| `x11` | `DISTRO_FEATURES` | X11 compatibility |
| `opengl` | `DISTRO_FEATURES` | Hardware rendering support |
| `pam` | `DISTRO_FEATURES` | Pluggable Authentication Modules |

These additions provide GUI support on top of the base minimal image.

---

That completes the Weston build process for Raspberry Pi 4 using Yocto (kirkstone).
