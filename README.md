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

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/vagrant-ubuntu-vms.git
   cd vagrant-ubuntu-vms

```sh
vagrant up

#login to WSL2

whoami
git config --global user.name "kpoxo6op"
git config --global user.email "kpoxo6op@gmail.com"
cat ~/.ssh/id_rsa
ssh-keygen -t rsa -b 2048 -f ~/.ssh/id_rsa -q -N ""

ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2222 vagrant@127.0.0.1
ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2223 vagrant@127.0.0.1
ssh-copy-id -i ~/.ssh/id_rsa.pub -p 2224 vagrant@127.0.0.1

#Configure sudo Without Password
sudo visudo
vagrant ALL=(ALL) NOPASSWD:ALL

#Install Python 3 and Required Packages on Each VM
ssh -p 2222 vagrant@127.0.0.1 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh -p 2223 vagrant@127.0.0.1 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"
ssh -p 2224 vagrant@127.0.0.1 "sudo apt-get update && sudo apt-get install -y python3 python3-pip sshpass"


# Verify Setup
ssh -p 2222 vagrant@127.0.0.1 "sudo whoami"


# windows. Will it persist?
PS C:\Users\borex> netsh interface portproxy add v4tov4 listenport=2222 listenaddress=127.0.0.1 connectport=22 connectaddress=192.168.56.10

PS C:\Users\borex> netsh interface portproxy add v4tov4 listenport=2223 listenaddress=127.0.0.1 connectport=22 connectaddress=192.168.56.11

PS C:\Users\borex> netsh interface portproxy add v4tov4 listenport=2224 listenaddress=127.0.0.1 connectport=22 connectaddress=192.168.56.12

boris@borex-pc:~/code/vagrant-ubuntu-vms$ cat ~/.ssh/id_rsa.pub | ssh -i ~/.ssh/insecure_private_key vagrant@192.168.56.10 "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
boris@borex-pc:~/code/vagrant-ubuntu-vms$ ssh vagrant@192.168.56.10
```

