## 📌 Yocto Project Architecture

### 1. Source Code
The base metadata and recipes used by the build system to define what to build and how to build it. This includes software layers, configuration files, and recipe files.

### 2. Terminal
A command-line interface where developers interact with the build system. Used to execute build commands, configure environment variables, and debug build processes.

### 3. Build System
The core part of the Yocto Project that orchestrates the entire build process. It includes:
- **BitBake**: Task scheduler and executor.
- **Build Engine**: Handles tasks like fetching, patching, compiling, and packaging.
- **Toolchain**: A set of cross-compilers and tools that generate binaries for the target hardware.

### 4. OpenEmbedded
A build framework used by Yocto Project that defines the core classes, functions, and metadata structure. It provides extensible support for embedded Linux development.

### 5. BitBake
The task executor tool in Yocto that parses metadata (recipes and classes), resolves dependencies, and runs defined build steps (e.g., `do_compile`, `do_install`, `do_rootfs`).

### 6. Output Folder (Binaries)
The directory where all generated binaries and images are placed after the build process. Includes kernel images, root filesystems, bootloaders, and SDKs.

### 7. Generate Images
The process of combining all compiled components into bootable system image files (`.wic`, `.sdimg`, `.iso`) that can be flashed onto embedded devices.

### 8. System Image(s)
Bootable operating system images ready to be deployed onto the target device. Includes everything needed to boot and run the OS on embedded hardware.

### 9. Target Hardware (SOM / Embedded Board)
The physical hardware (e.g., Raspberry Pi 4, or a System-on-Module) that runs the generated image. It boots from the image stored on external media like SD cards or eMMC.

### 10. Output Text (Console Log)
The runtime output displayed from the board’s serial or HDMI console. Useful for debugging, observing boot logs, and verifying system behavior.
