# Vagrant Ubuntu VMs Setup

This repository contains the configuration for setting up multiple Ubuntu VMs using Vagrant and VirtualBox. These VMs are prepared for a future Kubernetes deployment using Kubespray.

## Prerequisites

Install VirtualBox
Download the VirtualBox installer from the [official website](https://www.virtualbox.org/wiki/Downloads).

```powershell
Start-Process -FilePath "VirtualBox-Installer.exe" -ArgumentList "--silent --ignore-reboot" -Wait
```

Install Vagrant

```powershell
Invoke-WebRequest -Uri "https://releases.hashicorp.com/vagrant/2.4.1/vagrant_2.4.1_windows_amd64.msi" -OutFile "$env:TEMP\Vagrant-Installer.msi"

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


## Troubleshooting

Vagrant gets stuck at 

```ruby
==> vm1: Waiting for machine to boot. This may take a few minutes...
    vm1: SSH address: 127.0.0.1:2222
    vm1: SSH username: vagrant
    vm1: SSH auth method: private key
```

try to  uninstall vagrant

```batch
wmic product where "name like 'Vagrant%'" call uninstall /nointeractive
```

start over from the simplest vagrantfile

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.define "vm1" do |vm|
    vm.vm.network "private_network", ip: "192.168.56.10"

    vm.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update -y
      sudo apt-get upgrade -y
    SHELL
  end
end
```


Test SSH access from WSL2 again.

```sh
mkdir -p ~/.vagrant.d
cp /mnt/c/Users/borex/.vagrant.d/insecure_private_key ~/.vagrant.d/insecure_private_key
chmod 600 ~/.vagrant.d/insecure_private_key
ssh -i ~/.vagrant.d/insecure_private_key vagrant@192.168.56.10
```

Download Ubuntu Server

```powershell
Invoke-WebRequest -Uri "https://mirror.2degrees.nz/ubuntu-releases/24.04.1/ubuntu-24.04.1-live-server-amd64.iso" -OutFile "C:\Users\borex\Downloads\ubuntu-24.04.1-live-server-amd64.iso"
```

Write image with Rufus (no CLI) to USB drive.

```powershell
New-Item -Path "F:\autoinstall" -ItemType Directory -Force
Copy-Item -Path "C:\Users\borex\code\vagrant-ubuntu-vms\autoinstall" -Destination "F:\" -Recurse -Force
```

Add `autoinstall ds=nocloud;s=/cdrom/autoinstall/` to the linux or append line in grub.cfg or txt.cfg respectively

Example Boot Configuration Modification:

For grub.cfg:

linux /casper/vmlinuz boot=casper autoinstall ds=nocloud\;s=/cdrom/autoinstall/ quiet ---

For txt.cfg:

append initrd=/casper/initrd boot=casper autoinstall ds=nocloud\;s=/cdrom/autoinstall/ quiet ---


### Another cloud init try with cloudinit ISO

```sh
boris@borex-pc:~/code/vagrant-ubuntu-vms/autoinstall$ genisoimage -output cloud-init.iso -volid cidata -joliet -rock user-data meta-data
```

Press Del at SOYO mini PC boot

Verify Cloud-Init ISO is Mounted During Installation. Ctrl+Alt+F2

```sh
lsblk
mount | grep cidata
```

```sh
less /var/log/cloud-init.log
```

## Day 3

Set static IPs with ASUS mobile app via DHCP

drivers https://www.touslesdrivers.com/index.php?v_page=23&v_code=79082&v_langue=en

## Day 4

donwload packages on online computer

```sh
mkdir -p ~/make_packages
cd ~/make_packages
sudo apt-get update
apt-get download make
sudo apt-get install apt-rdepends -y
apt-rdepends make | grep -v "^ " | xargs apt-get download
```

```sh
mkdir -p ~/gcc_packages
cd ~/gcc_packages
sudo apt-get update
sudo apt-get install -y apt-rdepends
apt-rdepends gcc-13 | grep -v "^ " | xargs apt-get download
```

```sh
mkdir -p ~/build_essential_packages
cd ~/build_essential_packages
sudo apt-get update
sudo apt-get install -y apt-rdepends
apt-rdepends build-essential | grep -v "^ " | grep -v "^libc-dev$" | xargs apt-get download
```

```sh
mkdir -p ~/driver_build_packages
cd ~/driver_build_packages
sudo apt-get update
sudo apt-get install -y apt-rdepends
apt-rdepends build-essential gcc-13 make \
    | grep -v "^ " \
    | grep -v "(virtual)" \
    | grep -v "^libc-dev$" \
    | sort -u > package_list.txt
cat package_list.txt | xargs apt-get download
```

Download Kernel Headers for offline computer

```sh
cd ~/driver_build_packages
apt-get download linux-headers-6.8.0-41-generic
```


insert USB stick in offline computer (draft commands)

```sh
sudo mkdir /media/usb
# not the device #, mount it
lsblk
sudo mount /dev/sdb1 /media/usb
cp -r make /tmp
cd /tmp
sudo dpkg -i *.deb
# ./yt_nic_install.sh
make -v
```

try to automate it with autoinstall

## Day 4

installer WSL2 prep <https://youtu.be/zJoK8MMI23Q?si=bzzq4EwIPcg5FR1K>

```sh
sudo apt-get install python3-pip python3.12-venv -y

cd ~/code
git clone https://github.com/kubernetes-sigs/kubespray.git && cd kubespray
pip3 install -r requirements.txt --break-system-packages --ignore-installed
pip3 install ruamel.yaml --break-system-packages
```

<https://github.com/kubernetes-sigs/kubespray/blob/master/docs/ansible/ansible.md#installing-ansible>

```sh
cp -rfp inventory/sample inventory/soy-cluster
pip3 install ruamel.yaml
declare -a IPS=(node-0,192.168.1.100 node-1,192.168.1.101 node-2,192.168.1.102)
CONFIG_FILE=inventory/soy-cluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
cat inventory/soy-cluster/hosts.yaml
ansible-playbook -i inventory/soy-cluster/hosts.yaml --become --become-user=root --user ubuntu cluster.yml
```

Check the run

```sh
cd ~
ssh ubuntu@192.168.1.100 -i pk.pem
sudo kubectl config get-contexts
sudo cat /root/.kube/config
```
