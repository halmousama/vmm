# VMM: Your Personal Virtual Machine Manager

**VMM** is a simple, powerful, and interactive command-line tool for managing QEMU/KVM virtual machines, written in pure Bash. It's designed to hide the complexity of QEMU commands behind an intuitive and easy-to-use interface, perfect for developers and power users who love the terminal.


## ✨ Features

- **Interactive Menus**: Uses `fzf` for fast and fuzzy selection of VMs and ISOs.
- **Self-Contained Structure**: Manages all your VMs, disk images, and ISOs in a single, organized `~/.vmm` directory.
- **Secure File Handling**: Safely moves ISO files to its library by copying, verifying the checksum, and then deleting the original.
- **Configuration-based**: Each VM has its own simple `.conf` file, making customization easy and transparent.
- **Full Lifecycle Management**: Create, run, install, edit, and delete VMs with simple commands.
- **Pure Bash**: No heavy dependencies, just common Linux utilities and `fzf`.

## ⚙️ Prerequisites

Before you begin, ensure you have the following installed on your Debian-based system:

- **QEMU/KVM**: The core virtualization packages.
- **fzf**: For the interactive fuzzy finder menus.

```bash
sudo apt update
sudo apt install qemu-system-x86 libvirt-daemon-system fzf
```

You also need to add your user to the `kvm` and `libvirt` groups:

```bash
sudo adduser $USER libvirt
sudo adduser $USER kvm
```
**Important:** You will need to log out and log back in for these group changes to take effect.

## 🚀 Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/halmousama/vmm.git
    cd vmm
    ```

2.  **Make the script executable and place it in your path:**
    ```bash
    chmod +x vmm-bin
    sudo cp "$(pwd)/vmm-bin" /usr/local/bin/vmm
    ```
    Now you can run the tool from anywhere by simply typing `vmm`.

## 📖 Usage

The tool is structured around two main modules: `iso` and `vm`. You can get help anytime by running `vmm help` or `vmm help <module>`.

### Managing ISOs (`vmm iso ...`)

First, you need to build your ISO library.

-   **Add an ISO from a path:**
    ```bash
    vmm iso add ~/Downloads/operating-system.iso
    ```

-   **Find and add ISOs on your system:**
    ```bash
    vmm iso find
    ```

-   **List all ISOs in your library:**
    ```bash
    vmm iso list
    ```

-   **Remove an ISO from the library:**
    ```bash
    vmm iso remove
    ```

### Managing Virtual Machines (`vmm vm ...`)

-   **Create a new VM (interactive):**
    ```bash
    vmm vm create
    ```
    This will guide you through naming the VM, setting resources, and selecting an ISO from your library.

-   **Install an OS on your new VM:**
    ```bash
    vmm vm install my-vm-name
    ```
    This will start the VM and boot from the selected ISO.

-   **Run the VM after installation:**
    ```bash
    vmm vm run my-vm-name
    ```
    If you omit the VM name (`vmm vm run`), you will get an interactive menu to choose from.

-   **Edit a VM's configuration:**
    ```bash
    vmm vm edit my-vm-name
    ```
    This opens the `.conf` file in `nano`, where you can uncomment and tweak advanced settings.

-   **List all your VMs:**
    ```bash
    vmm vm list
    ```

-   **Delete a VM (use with caution!):**
    ```bash
    vmm vm delete my-debian-vm
    ```
    This will ask for strict confirmation before deleting the VM's config file and its disk image.

## 🔧 Configuration

Each virtual machine has a configuration file located in `~/.vmm/configs/`. The file is a simple key-value store. You can manually edit it or use `vmm vm edit`.

Example `my-vm-name.conf`:
```ini
# --- VM Configuration for my-vm-name ---

# --- Basic Settings (Required) ---
ram=2G
cpu_cores=2
disk_image=/home/user/.vmm/images/my-debian-vm.qcow2
iso_file=/home/user/.vmm/iso/debian-12-netinst.iso

# --- Advanced Settings (Uncomment to enable) ---
#cpu_model=host
#enable_kvm=true
#vga_card=virtio
#...and more
```

## ⚖️ License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
