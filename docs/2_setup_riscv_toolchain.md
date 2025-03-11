## **Install the RISC-V Toolchain**

The RISC-V toolchain consists of several components, including:
- **GNU Toolchain (GCC, Binutils, GDB, etc.)** – for compiling and debugging RISC-V programs.
- **Spike (RISC-V ISA Simulator)** – for running RISC-V binaries.
- **RISC-V Proxy Kernel and Bootloader (pk, bbl, OpenSBI)** – required for running programs on Spike.
- **QEMU (optional)** – to emulate RISC-V on your x86 VM.

### **1. Install Required Dependencies**
Before building the toolchain, install necessary dependencies:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y autoconf automake autotools-dev curl \
    libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex \
    texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev git \
    python3 python3-pip cmake ninja-build
```

### **2. Clone and Build the RISC-V GNU Toolchain**
The official RISC-V GNU toolchain repository contains GCC, Binutils, and other essential tools.

```bash
mkdir -p ~/riscv && cd ~/riscv
git clone --recursive https://github.com/riscv-collab/riscv-gnu-toolchain
cd riscv-gnu-toolchain
```
> **Note**: This step will take a long time due to the large size of the repository (More than 9GB).

##### **2.2.1: Build the Toolchain for RISC-V**
To build the toolchain for **RV64GC (64-bit general-purpose RISC-V ISA)**, run:
```bash
./configure --prefix=/opt/riscv --enable-multilib
make -j$(nproc)
```
> **Note**: This step will take a long time (~1 hour, depending on your system).

##### **2.2.2: Add Toolchain to PATH**
Once the toolchain is built, update your `PATH` environment variable:
```bash
echo 'export PATH=/opt/riscv/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

#### **2.3: Verify the Installation**
After installation, check if the toolchain is working:
```bash
riscv64-unknown-elf-gcc --version
```
You should see output similar to:
```
riscv64-unknown-elf-gcc (g04696df096) 14.2.0
Copyright (C) 2024 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```