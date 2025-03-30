## **Setting Up RISC-V BOOM (via Chipyard)**
[Chipyard](https://chipyard.readthedocs.io/en/latest/) is a **SoC (System-on-Chip) design framework** that includes BOOM, Rocket, and other RISC-V-based cores. This guide will help you set up the Chipyard repository and run a simple simulation using the BOOM core.

### **1. Install Additional Dependencies**
Ensure you have all required dependencies before proceeding:
```bash
# Add sbt repository for sbt installation
echo "deb https://repo.scala-sbt.org/scalasbt/debian all main" | sudo tee /etc/apt/sources.list.d/sbt.list

echo "deb https://repo.scala-sbt.org/scalasbt/debian /" | sudo tee /etc/apt/sources.list.d/sbt_old.list

curl -sL "https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x2EE0EA64E40A89B84B2DF73499E82A75642AC823" | sudo -H gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/scalasbt-release.gpg --import

sudo chmod 644 /etc/apt/trusted.gpg.d/scalasbt-release.gpg

sudo apt update
sudo apt install -y git default-jdk scala sbt python3 \
    python3-pip python3-setuptools python3-venv cmake \
    ninja-build autoconf automake libtool curl make g++ \
    unzip patch clang-format clang libtinfo-dev libz-dev \
    jq lld llvm-dev llvm-14 llvm-14-dev device-tree-compiler
```

### **Clone the Chipyard Repository**
```bash
cd ~
git clone --recursive https://github.com/ucb-bar/chipyard.git
cd chipyard
```
> The `--recursive` flag ensures all submodules are pulled correctly. It will take some time to clone the repository.

> Note: There are some repositories that are not available anymore (private repositories). Git will ask for credentials to access them. You can skip these repositories by pressing `Enter` when prompted for a username and password.

Download latest release version of `firtool` via the following command:
```bash
cd ~
wget https://github.com/llvm/circt/releases/download/firtool-1.71.0/firrtl-bin-linux-x64.tar.gz

tar -xvf firrtl-bin-linux-x64.tar.gz

mkdir -p ~/chipyard/tools/circt/bin/
cp firtool-1.71.0/bin/* ~/chipyard/tools/circt/bin/

echo 'export PATH=~/chipyard/tools/circt/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# Verify firtool installation
firtool --version
```

Setup Conda
```bash
curl --output anaconda3.sh https://repo.anaconda.com/archive/Anaconda3-2023.07-2-Linux-x86_64.sh
chmod +x anaconda3.sh
./anaconda3.sh
```

Follow instructions then
```bash
echo 'export PATH=/srv/anaconda3/bin/:$PATH' >> ~/.bashrc
source ~/.bashrc

conda --version
```

The following script (`build-setup.sh`) will complete a “full” installation of Chipyard which may take a long time depending on the system. Ensure that this script completes fully (no interruptions) before continuing on. User can use the --skip or -s flag to skip steps:

- `-s 1`: skips initializing Conda environment
- `-s 2`: skips initializing Chipyard submodules
- `-s 3`: skips initializing toolchain collateral (Spike, PK, tests, libgloss)
- `-s 4`: skips initializing ctags
- `-s 5`: skips pre-compiling Chipyard Scala sources
- `-s 6`: skips initializing FireSim
- `-s 7`: skips pre-compiling FireSim sources
- `-s 8`: skips initializing FireMarshal
- `-s 9`: skips pre-compiling FireMarshal default buildroot Linux sources
- `-s 10`: skips installing CIRCT
- `-s 11`: skips running repository clean-up

Run the following command to initialize Chipyard’s environment:
```bash
cd ~/chipyard
./build-setup.sh riscv-tools 
echo 'export PATH=~/chipyard/tools/circt/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
source env.sh
```
Then rerun:

### **Build the BOOM Core**
```bash
cd ~/chipyard/sims/verilator
make CONFIG=LargeBoomV3Config
```

### Execute sample code

```bash
mv hello.c hello.c.original
touch hello.c
```

```c
#define FINISH() asm volatile ("li a0, 0x1; li a7, 0x5d; ecall")

volatile int * const UART_ADDR = (int *)0x10000000;

void putchar(char c) {
    *UART_ADDR = c;
}

void print_hello() {
    const char *str = "Hello from BOOM!\n";
    while (*str) putchar(*str++);
}

void main() {
    print_hello();
    FINISH();
    // while (1);  // halt
}

```

```bash
cd ~/chipyard/tests
riscv64-unknown-elf-gcc -static -mcmodel=medany -fvisibility=hidden   -nostdlib -nostartfiles   -T ~/chipyard/toolchains/riscv-tools/riscv-isa-sim/debug_rom/link.ld   hello.c -o hello.riscv
```

## **Step 4: Run the Test on BOOM**
Run the RISC-V binary on BOOM using Verilator:
```bash
cd ~/chipyard/sims/verilator
make CONFIG=LargeBoomV3Config run-binary BINARY=../../tests/hello.riscv
```