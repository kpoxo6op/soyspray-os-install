# Ubuntu VMs Setup

This repository contains the configuration for setting up Ubuntu VMs based on Soyo Mini PCs.

## Prerequisites

Soyo PCs

Set BIOS to boot from USB for full autoinstall experience

Set static IPs with ASUS mobile app via DHCP

## Notes

Soyo BIOS does no support wake up on LAN.

Soyo Ethernet is not supported by Ubuntu Server out of the box.

Downloaded drivers
https://www.touslesdrivers.com/index.php?v_page=23&v_code=79082&v_langue=en

## Steps

Download Ubuntu Server

```powershell
Invoke-WebRequest -Uri "https://mirror.2degrees.nz/ubuntu-releases/24.04.1/ubuntu-24.04.1-live-server-amd64.iso" -OutFile "C:\Users\borex\Downloads\ubuntu-24.04.1-live-server-amd64.iso"
```

donwload packages on online computer

```sh
mkdir driver-build-packages
cd driver-build-packages
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install -y apt-rdepends
apt-rdepends build-essential gcc-13 make \
  | grep -v "^ " \
  | grep -v "(virtual)" \
  | grep -v "^libc-dev$" \
  | grep -v "^debconf-2.0$" \
  | sort -u > package_list.txt
cat package_list.txt | xargs apt-get download
```

Download *MATCHING* Kernel Headers for offline computer

```sh
sudo add-apt-repository ppa:canonical-kernel-team/ppa
sudo apt update
# matching `apt search linux-headers` not found, try closest ones
# apt-get download linux-headers-6.8.0-41-generic
# salvage linux-headers-6.8.0-41-generic from one of the miniPCs if 6.8.0-40 does not work
apt-get download linux-headers-6.8.0-40-generic
```

## Create Autoinstall USBs

```sh
# copy driver-build-packages to USB
# extract  yt6801-linux-driver-1.0.29.zip to USB
```
