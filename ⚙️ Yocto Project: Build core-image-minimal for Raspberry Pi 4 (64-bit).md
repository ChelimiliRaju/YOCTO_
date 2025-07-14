# Yocto Project: Build core-image-minimal for Raspberry Pi 4 (64-bit)

This guide provides detailed steps to build a minimal Linux image (core-image-minimal) using the Yocto Project (Kirkstone release) for Raspberry Pi 4 64-bit (raspberrypi4-64). Ideal for embedded CLI applications with minimal footprint.

---

## ⚖️ Table of Contents

1. About Yocto & core-image-minimal  
2. Host System Requirements  
3. Directory Setup  
4. Download Yocto & Layers  
5. Initialize Build Environment  
6. Configure local.conf  
7. Configure bblayers.conf  
8. Build the Image  
9. Output Artifacts  
10. Flash Image to SD Card  
11. First Boot on Raspberry Pi  
12. Optional Customizations  
13. Common Errors & Fixes  
14. References

---

## 1. About Yocto & core-image-minimal

- Yocto is a framework for creating custom Linux distributions for embedded systems.
- core-image-minimal is the smallest ready-to-use CLI image built using Yocto. It has:
  - BusyBox shell
  - Basic init system
  - Minimal system services
- Target device: Raspberry Pi 4 (64-bit CPU)

---

## 2. Host System Requirements

- OS: Ubuntu 20.04 / 22.04 (64-bit recommended)
- Minimum specs:
  - RAM: 8 GB
  - Disk: 100+ GB

Install required packages:

```bash
sudo apt update && sudo apt install gawk wget git-core diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm
```

---

## 3. Directory Setup

```bash
mkdir -p ~/Desktop/Yocto_Rc && cd ~/Desktop/Yocto_Rc
```

---

## 4. Download Yocto and Meta Layers

```bash
git clone -b kirkstone git://git.yoctoproject.org/poky poky-rpi
cd poky-rpi
git clone -b kirkstone https://git.yoctoproject.org/meta-raspberrypi
git clone -b kirkstone https://git.openembedded.org/meta-openembedded
<img width="1066" height="392" alt="image" src="https://github.com/user-attachments/assets/effb3b80-636d-4c4f-a01c-34d0f5cedcf2" />

```

- poky: main Yocto repository
- meta-raspberrypi: Raspberry Pi machine support
- meta-openembedded: additional packages and libraries

---

## 5. Initialize the Build Environment

```bash
source oe-init-build-env build
```

What it does:
- Creates a build directory (named build)
- Sets up bitbake environment
- Generates conf/local.conf and conf/bblayers.conf

---

## 6. Configure local.conf

Edit conf/local.conf:

```bash
MACHINE ?= "raspberrypi4-64"
IMAGE_FEATURES += "ssh-server-openssh"
ENABLE_UART = "1"
ENABLE_VC4GRAPHICS = "1"
PREFERRED_VERSION_linux-raspberrypi = "5.15%"
EXTRA_IMAGE_FEATURES += "debug-tweaks"
```

Keywords:
- MACHINE: hardware platform
- IMAGE_FEATURES: toggles features like ssh, debug tools
- EXTRA_IMAGE_FEATURES: enables additional debugging tools
- PREFERRED_VERSION: locks a specific version

---

## 7. Configure bblayers.conf

Add these layers to BBLAYERS:

```bash
BBLAYERS += " \
  ${TOPDIR}/../meta-raspberrypi \
  ${TOPDIR}/../meta-openembedded/meta-oe \
  ${TOPDIR}/../meta-openembedded/meta-python \
  ${TOPDIR}/../meta-openembedded/meta-networking \
"
```

Explanation:
- meta-oe: additional base packages
- meta-python: for python-based tools
- meta-networking: useful for net utilities

---

## 8. Build the Image

```bash
bitbake core-image-minimal
```

This command compiles all dependencies and generates the image.

Keywords:
- bitbake: Yocto’s build tool
- core-image-minimal: minimal rootfs target

---

## 9. Output Artifacts

After build:

Navigate to:

```bash
cd tmp/deploy/images/raspberrypi4-64/
```

You will find:
- core-image-minimal-raspberrypi4-64.rpi-sdimg → Full SD card image
- core-image-minimal-raspberrypi4-64.tar.bz2 → Root filesystem archive
- *.dtb → Device Tree Binaries
- Image → Kernel
- bootfiles/ → Bootloader (firmware)

---

## 10. Flash Image to SD Card

Use dd or balenaEtcher to flash:

```bash
sudo dd if=core-image-minimal-raspberrypi4-64.rpi-sdimg of=/dev/sdX bs=4M status=progress conv=fsync
```

Replace /dev/sdX with your SD card path.

---

## 11. First Boot on Raspberry Pi

- Insert the SD card
- Connect serial console or monitor + USB keyboard
- Boot to minimal shell
- Login prompt appears: root (no password required)

---

## 12. Optional Customizations

Add packages:

```bash
IMAGE_INSTALL:append = " nano htop vim i2c-tools"
```

Enable systemd:

```bash
DISTRO_FEATURES:append = " systemd"
VIRTUAL-RUNTIME_init_manager = "systemd"
DISTRO_FEATURES:remove = " sysvinit"
```

---

## 13. Common Errors & Fixes

| Problem                             | Solution                                         |
|------------------------------------|--------------------------------------------------|
| ed: command not found              | sudo apt install ed                              |
| fbc build failure (bc-native)      | Remove or patch bc-native or install ed          |
| Layer not found                    | Double-check paths in bblayers.conf              |
| Invalid MACHINE name               | Use correct name: raspberrypi4-64                |
| Permissions error                  | Never run bitbake as root                        |
| Build corruption                   | Clean and rebuild: bitbake -c cleansstate ...    |

---

## 14. References

- https://www.yoctoproject.org
- https://github.com/agherzan/meta-raspberrypi
- https://docs.yoctoproject.org/kirkstone/
