## **Setting Up Verilator (Latest Version from Source)**
Verilator is an open-source tool used to compile and simulate Verilog code. This guide will help you install the latest version of Verilator from source on your system.

### **1. Prerequisites**
Before proceeding, ensure your system has the necessary dependencies installed.

#### Install Required Dependencies
```bash
sudo apt update && sudo apt install -y git perl python3 make autoconf g++ flex bison ccache help2man
```

### **2. Installation Steps**

#### 1️⃣ Clone the Verilator Repository
```bash
cd ~
git clone https://github.com/verilator/verilator.git
cd verilator
```

#### 2️⃣ Checkout the Latest Stable Version
Find the latest **stable** version from [Verilator Releases](https://github.com/verilator/verilator/tags) and check it out:
```bash
git checkout v5.032  # Replace with latest version if available
```

#### 3️⃣ Build Verilator
Run the following commands to build Verilator from source:
```bash
autoconf
./configure
make -j$(nproc)
```

#### 4️⃣ Install Verilator
```bash
sudo make install
```

## **3. Verify Installation**
To ensure Verilator is installed correctly, check its version:
```bash
verilator --version
```
You should see output like:
```
Verilator 5.032 2025-01-01 rev v5.032  # (or the latest installed version)
```

## **4. Troubleshooting**

### **Verilator command not found**
If Verilator is installed but not recognized, add it to your PATH:
```bash
export PATH=/usr/local/bin:$PATH
```
To make this change permanent, add it to your `~/.bashrc` or `~/.profile`:
```bash
echo 'export PATH=/usr/local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## **5. Compilation errors**
Ensure all required dependencies are installed. If errors persist, try cleaning and rebuilding:
```bash
make clean
make -j$(nproc)
sudo make install
```