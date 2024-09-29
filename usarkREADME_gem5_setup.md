# gem5 Setup Guide

This guide will walk you through setting up gem5 from scratch, including installing prerequisites, cloning the repository, building, and running simulations.

## Prerequisites

Before you can clone, build, and run gem5, ensure the following software and libraries are installed on your system:

1. **Build tools**:
   - **GCC 10 or newer**:
     ```bash
     sudo apt-get install gcc-10 g++-10
     ```
   - **Python 3.x**:
     ```bash
     sudo apt-get install python3
     ```
   - **SCons** (gem5's build system):
     ```bash
     sudo apt-get install scons
     ```
   - **zlib**:
     ```bash
     sudo apt-get install zlib1g-dev
     ```
   - **Protobuf (Optional)** (for trace capture/playback support):
     ```bash
     sudo apt-get install protobuf-compiler libprotobuf-dev
     ```

2. **Other useful tools**:
   - **Capstone** (for disassembly support):
     ```bash
     sudo apt-get install libcapstone-dev
     ```

   - **Python packages** (optional, for specific simulations):
     ```bash
     sudo apt-get install python3-pip
     pip3 install matplotlib numpy
     ```

## Cloning the gem5 Repository

To get started with gem5, you need to clone the gem5 repository:

```bash
git clone https://gem5.googlesource.com/public/gem5
cd gem5
```

If you are working with a specific branch or version (e.g., 23.1), you can check out that branch:

```bash
git checkout stable
```

## Building gem5

gem5 can be built for multiple Instruction Set Architectures (ISAs) like X86, ARM, RISCV, etc. Below are the steps to configure and build gem5.

### 1. Defining Configuration (Kconfig)

First, define the build configuration using the `defconfig` command.

- **Build for all ISAs** (Default setup):
  ```bash
  scons defconfig gem5_build build_opts/ALL
  ```

- **Or, build for a specific ISA** (e.g., X86):
  ```bash
  scons defconfig gem5_build build_opts/X86
  ```

### 2. Customizing Configuration (Optional)

If you want to customize your build (e.g., enabling specific features like KVM or SystemC), you can set specific config options:

```bash
scons setconfig gem5_build USE_KVM=y
```

Alternatively, you can use the interactive `menuconfig` interface:

```bash
scons menuconfig gem5_build
```

### 3. Building gem5

Once the configuration is done, build gem5 using:

```bash
scons -j$(nproc) gem5_build/gem5.opt  # Use as many cores as available
```

You can replace `gem5.opt` with other variants like `gem5.debug` for debugging or `gem5.fast` for the fastest runtime without debugging features.

## Running gem5

After successfully building gem5, you can run simulations using the built binary. Here’s a basic example using the `hello_world.py` script provided in gem5's repository:

```bash
gem5_build/gem5.opt configs/example/hello_world.py
```

This will run a simple "Hello, World!" example simulation using the built gem5 binary.

### Example: Running Full-System Simulations

For more complex simulations, such as running a full-system (e.g., X86 Ubuntu):

```bash
gem5_build/gem5.opt configs/example/fs.py --kernel=x86-linux-kernel --disk-image=ubuntu-20.04.img
```

This command runs a full-system simulation with an X86 Linux kernel and a Ubuntu disk image. Ensure you have the appropriate resources (kernel, disk image) available.

## Additional Build Variants

### Debug Build

For detailed debugging support, you can build gem5 in the `debug` variant, which disables optimizations:

```bash
scons -j$(nproc) gem5_build/gem5.debug
```

### Fast Build

For maximum performance with no debugging, use the `fast` build variant:

```bash
scons -j$(nproc) gem5_build/gem5.fast
```

## Troubleshooting

### Error: "gcc version 10 or newer required"
If you encounter the error indicating that **gcc version 10 or newer** is required, follow these steps to install GCC 10:

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-10 g++-10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 100
```

### Capstone Warnings
If you see a warning about the Capstone disassembly framework, install the Capstone library:

```bash
sudo apt-get install libcapstone-dev
```

## Saving and Sharing Custom Configurations

After customizing your gem5 build using `menuconfig` or other configuration options, you can save the configuration for reuse:

```bash
scons savedefconfig gem5_build my_custom_defconfig
```

This will save your configuration as `my_custom_defconfig`, which you can reuse later or share with others.

## Further Reading

For more detailed documentation and advanced configurations, visit the official gem5 documentation:

- [gem5 Documentation](https://www.gem5.org/documentation/)
- [gem5 Learning Series](https://www.gem5.org/documentation/learning_gem5/introduction/)

