# Vagrant Ubuntu VMs Setup

This repository contains the configuration for setting up multiple Ubuntu VMs using Vagrant and VirtualBox. These VMs are prepared for a future Kubernetes deployment using Kubespray.

## Prerequisites

1. **VirtualBox Installation**
   - Download the VirtualBox installer from the [official website](https://www.virtualbox.org/wiki/Downloads).
   - Install VirtualBox using the installer with the `/norestart` option to avoid a reboot:
     ```powershell
     Start-Process -FilePath "VirtualBox-Installer.exe" -ArgumentList "--silent --ignore-reboot" -Wait
     ```

2. **Vagrant Installation**
   - Download the Vagrant installer from the [official website](https://www.vagrantup.com/downloads).
   - Install Vagrant using the following command to avoid a reboot:
     ```powershell
     Start-Process -FilePath "msiexec.exe" -ArgumentList "/i `"$env:TEMP\Vagrant-Installer.msi`" /qn REBOOT=ReallySuppress" -Wait
     ```

## Setting Up VMs Using Vagrant

In windows:

```sh
git clone https://github.com/yourusername/vagrant-ubuntu-vms.git
cd vagrant-ubuntu-vms
vagrant up
```

In WSL2:

```sh
#login to WSL2
# Vagrant Ubuntu VMs Setup

This repository contains the configuration for setting up multiple Ubuntu VMs using Vagrant and VirtualBox. These VMs are prepared for a future Kubernetes deployment using Kubespray.

## Prerequisites

1. **VirtualBox Installation**
   - Download the VirtualBox installer from the [official website](https://www.virtualbox.org/wiki/Downloads).
   - Install VirtualBox using the installer with the `/norestart` option to avoid a reboot:
     ```powershell
     Start-Process -FilePath "VirtualBox-Installer.exe" -ArgumentList "--silent --ignore-reboot" -Wait
     ```

2. **Vagrant Installation**
   - Download the Vagrant installer from the [official website](https://www.vagrantup.com/downloads).
   - Install Vagrant using the following command to avoid a reboot:
     ```powershell
     Start-Process -FilePath "msiexec.exe" -ArgumentList "/i `"$env:TEMP\Vagrant-Installer.msi`" /qn REBOOT=ReallySuppress" -Wait
     ```

## Setting Up VMs Using Vagrant

In windows:

```sh
git clone https://github.com/yourusername/vagrant-ubuntu-vms.git
cd vagrant-ubuntu-vms
vagrant up
```

In WSL2:

```sh
# Login to WSL2
whoami
git config --global user.name "kpoxo6op"
git config --global user.email "kpoxo6op@gmail.com"
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -q -N ""

# Copy SSH key to VMs for passwordless SSH access
cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/insecure_private_key vagrant@192.168.56.10 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/insecure_private_key vagrant@192.168.56.11 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/insecure_private_key vagrant@192.168.56.12 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"

# Access VMs directly using their IPs
ssh vagrant@192.168.56.10
echo "vagrant ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers
ssh vagrant@192.168.56.11
echo "vagrant ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers
ssh vagrant@192.168.56.12
echo "vagrant ALL=(ALL) NOPASSWD:ALL" | sudo tee -a /etc/sudoers

# Install Python 3 and sshpass
# Required for Kubespray and Ansible setup
ssh vagrant@192.168.56.10 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh vagrant@192.168.56.11 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh vagrant@192.168.56.12 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"

# Install Kubespray Python Requirements
# Clone the Kubespray repository, navigate to its directory, and install the required Python dependencies:
ssh vagrant@192.168.56.10 "git clone https://github.com/kubernetes-sigs/kubespray.git && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt"
ssh vagrant@192.168.56.11 "git clone https://github.com/kubernetes-sigs/kubespray.git && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt"
ssh vagrant@192.168.56.12 "git clone https://github.com/kubernetes-sigs/kubespray.git && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt"

# Verify setup
ssh vagrant@192.168.56.10 "sudo whoami"
```
