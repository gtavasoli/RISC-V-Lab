## Setting Up the Virtual Machine (VM)
We will create a VM that will serve as our testing environment. We need to decide on the following:
- **Hypervisor**: Are you using VirtualBox or VMware?
- **Operating System**: Ubuntu 22.04 LTS is recommended for compatibility with RISC-V toolchains and development.
- **System Resources**:
  - CPU: At least **4 cores**
  - RAM: **8GB** or more (recommended)
  - Disk Space: **20GB+**

### 1.1: Download Ubuntu 22.04 LTS
- Get the ISO from [Ubuntu official website](https://releases.ubuntu.com/22.04/).

### 1.2: Create a VM
- If using **VirtualBox**:
  1. Open VirtualBox and click **New**.
  2. Name the VM and select **Linux → Ubuntu (64-bit)**.
  3. Assign **8GB RAM** and **4 cores**.
  4. Create a **20GB dynamically allocated** virtual hard disk.
  5. Attach the **Ubuntu ISO** to the virtual CD drive.
  6. Start the VM and proceed with Ubuntu installation.

- If using **VMware**:
  1. Click **Create a New Virtual Machine**.
  2. Select **Typical Installation** and choose the Ubuntu ISO.
  3. Allocate **8GB RAM** and **4 cores**.
  4. Set the disk size to **20GB+**.
  5. Finish the setup and start the VM.