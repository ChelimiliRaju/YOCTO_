## 📌 Yocto Project Architecture
![WhatsApp Image 2025-07-14 at 11 52 12_0f8484cb](https://github.com/user-attachments/assets/c24573da-54c3-4b34-a867-ebe0bd7545f8)

# Yocto Project Architecture – Explained

This document explains the complete architecture of the Yocto Project based on the architecture diagram. Each section has been explained in detail with color codes, flow, and definitions.

---

## ⚙️ Input Sections (Left Side)

### 1. User Configuration
- Comes from: `conf/local.conf`
- Defines user-level settings:
  - `MACHINE`, `DISTRO`, `IMAGE_FEATURES`, `BB_NUMBER_THREADS`, etc.

### 2. Metadata (.bb + patches)
- BitBake recipes `.bb`, `.bbclass`, `.conf`
- Describe how to fetch, configure, build, install, and package software
- May include patches via `SRC_URI`

### 3. Machine (BSP) Configuration
- Board Support Package config
- Located in layers like `meta-raspberrypi`
- Defines device tree, kernel, u-boot, etc.

### 4. Policy Configuration
- Found in distro files like `poky.conf`
- Controls build policies such as:
  - Init system: `systemd` or `sysvinit`
  - Filesystem types: `.ext4`, `.wic`, `.cpio`
  - Package formats: `.rpm`, `.deb`, `.ipk`

---

## 👤 Source Mirror(s)

### Upstream Project Releases
- Public project releases (e.g., GNU tools, Linux kernel)

### Local Projects
- In-house applications or custom modules

### SCMs (optional)
- Pull sources from Git, SVN, etc.
- Managed by `SRC_URI` in recipes

---

## 🔧 Build System (Blue Blocks = Process Steps)

### 1. Source Fetching
- BitBake fetches source from upstream/local/SCM

### 2. Patch Application
- Applies patches from metadata if specified

### 3. Configuration / Compile / Autotools
- Configures software using `autoconf`, `cmake`, etc.
- Cross-compiles with toolchain for target arch

### 4. Output Analysis
- Determines package dependencies and splits output

### 5. Package Generation
- Generates binary packages:
  - `.rpm`, `.deb`, `.ipk`
  - Tools: rpm-native, dpkg-native, opkg-native

### 6. QA Tests
- Ensures built artifacts are correct and complete
- Validates libraries, file ownership, symlinks, etc.

---

## 📦 Output Artifacts

### Package Feeds (Green Block)
- Directory of generated `.rpm/.deb/.ipk`
- Used by rootfs generation or external feed

### Image Generation (Blue Block)
- Combines packages into image formats:
  - `.wic`, `.ext4`, `.cpio.gz`, `.tar.bz2`

### SDK Generation (Blue Block)
- Generates Software Development Kit
- Contains cross-toolchain, sysroots, headers

---

## 🔴 Final Outputs (Red Blocks)

### Images
- Flashable system images
- Example: `core-image-minimal-raspberrypi4-64.wic`

### Application Development SDK
- Built using:
  ```sh
  bitbake -c populate_sdk core-image-minimal
  ```
- Includes GCC cross-compiler, libc, include headers

---


---

## Summary
The Yocto Project builds Linux images by combining metadata, policies, and source code through BitBake. The architecture shows how source is fetched, patched, built, and then packaged into flashable system images and SDKs.

