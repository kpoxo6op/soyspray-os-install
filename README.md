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

# Allow vagrant User to Execute sudo Without Password Prompt:
ssh vagrant@192.168.56.10 "echo 'vagrant ALL=(ALL) NOPASSWD:ALL' | sudo tee -a /etc/sudoers"
ssh vagrant@192.168.56.11 "echo 'vagrant ALL=(ALL) NOPASSWD:ALL' | sudo tee -a /etc/sudoers"
ssh vagrant@192.168.56.12 "echo 'vagrant ALL=(ALL) NOPASSWD:ALL' | sudo tee -a /etc/sudoers"

# Verify setup
ssh vagrant@192.168.56.10 "sudo whoami"

# Install Python 3, pip, and sshpass: These are needed for Kubespray to work.
ssh vagrant@192.168.56.10 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh vagrant@192.168.56.11 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh vagrant@192.168.56.12 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"

# Upgrade pip: Ensure pip is the latest version to avoid dependency issues.
ssh vagrant@192.168.56.10 "sudo -H pip3 install --upgrade pip"
ssh vagrant@192.168.56.11 "sudo -H pip3 install --upgrade pip"
ssh vagrant@192.168.56.12 "sudo -H pip3 install --upgrade pip"

# Clone Kubespray and Install Python Requirements:
ssh vagrant@192.168.56.10 "
    if [ ! -d kubespray ]; then
        git clone https://github.com/kubernetes-sigs/kubespray.git;
    fi && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt
"
ssh vagrant@192.168.56.11 "
    if [ ! -d kubespray ]; then
        git clone https://github.com/kubernetes-sigs/kubespray.git;
    fi && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt
"
ssh vagrant@192.168.56.12 "
    if [ ! -d kubespray ]; then
        git clone https://github.com/kubernetes-sigs/kubespray.git;
    fi && cd kubespray && sudo pip3 install --ignore-installed -r requirements.txt
"
```
