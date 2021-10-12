---
layout: post
title: "KVM/QEMU setup on Arch Linux"
date: 2021-10-12
excerpt: "Have fun and get the most out of your machine."
tags: [tutorial, arch linux]
---

It's time to learn how to install QEMU with KVM, which in my opinion it is the best VM software for GNU/Linux. Why? Mainly because the kernel modules needed for KVM are part of the Linux kernel which means it would not create a mess like VirtualBox, also QEMU used together with KVM can achieve near native performance, and last but not least, they are free and open source unlike VMware.

## Prerequisites
To check whether virtualization is enabled or supported by your processor, use the following command:

```
LC_ALL=C lscpu | grep Virtualization
```

It should return `Virtualization: VT-x` for Intel processors and `Virtualization: AMD-V` for AMD processors. If nothing is displayed after this command, it means that either your CPU does not support virtualization or it is not enabled. If your CPU was manufactured in the last 10 years, it should support virtualization, you will need to enter the BIOS to enable it.

## Installation

To install all needed packages, run the following code:
```
sudo pacman -S qemu virt-manager ebtables dnsmasq vde2
```

- Qemu: Provides the actual emulation layer.
- KVM: The technology in the Linux kernel for using accelerated virtualization. 
- Virt-manager: GUI for libvirt, a software for managing virtual machines.
- Ebtables: Filtering tool for a Linux-based bridging firewall.
- Dnsmasq: DNS forwarder and DHCP server.
- Bridge-utils : Network bridge needed for VMs
- Vde2: Virtual Distributed Ethernet for QEMU and other emulators.

After that, we will need to enable the libvirt service using `sudo systemctl enable --now libvirtd`. You can check the libvirtd service status with `sudo systemctl status libvirtd`. Which should output something similar as below, make sure that it says it is loaded and active.
```
● libvirtd.service - Virtualization daemon
     Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: disabled)
     Active: active (running) since Tue 2021-10-12 14:12:19 CEST; 1min 4s ago
TriggeredBy: ● libvirtd-admin.socket
             ● libvirtd.socket
             ● libvirtd-ro.socket
       Docs: man:libvirtd(8)
             https://libvirt.org
   Main PID: 1685157 (libvirtd)
      Tasks: 19 (limit: 32768)
     Memory: 10.3M
        CPU: 287ms
     CGroup: /system.slice/libvirtd.service
             └─1685157 /usr/bin/libvirtd --timeout 120
```

By default only the root can create and manage virtual machines. In order to use KVM with your standard user, you should add your user to the libvirt group with:
```
sudo usermod -G libvirt -a user_name
```

## Create a VM
Open "virt-manager" and select the "+" button:
![2021-10-11-19:17:13](https://user-images.githubusercontent.com/34800654/136854851-2842f40c-6527-4f81-b65f-09aa86bf5fcc.png)

If you have an ISO already installed, select the first option, "Local install media" and click "Forward".
![2021-10-11-19:18:07](https://user-images.githubusercontent.com/34800654/136854852-955f43bf-c1d5-46ac-8c05-ed1997e327c4.png)

Next we will select "Browse" and choose our downloaded ISO, also if the automatic detection of the operating system did not work, you will need to uncheck the option "Automatically detect from the installation media/source" and manually select it.
![2021-10-11-19:20:17](https://user-images.githubusercontent.com/34800654/136854853-59b8b624-dc62-42c1-b09c-4831264218f1.png)

Now we will choose the amount of RAM and CPU cores we will allocate for this VM, if you just want to test out a Linux distribution, 4GB RAM and 2 cores will be more than enough.
![2021-10-11-19:21:07](https://user-images.githubusercontent.com/34800654/136854854-c51b85b1-a543-4ca0-afe4-391525dcb541.png)

Then choose the desired disk size for the Virtual Machine.
![2021-10-11-19:21:54](https://user-images.githubusercontent.com/34800654/136854855-6481fde6-079b-40d6-ae50-90620d515228.png)

Double-check your configuration, choose the name you like for the VM and select your preferred network connection, usually the default option works fine.
![2021-10-11-20:06:01](https://user-images.githubusercontent.com/34800654/136854859-aeb7912f-a18b-4329-836e-64915abe5d9e.png)

If this is the first time a VM is created, virt-manager will display a window asking to start the network service, if this window appears, click yes. 
![2021-10-11-20:06:24](https://user-images.githubusercontent.com/34800654/136854862-95539f8c-ad86-46ee-a4f1-9f50195c3243.png)


## Optional
### Nested virtualization
If you want to run another VM inside a VM run these commands on the host: 
```
# For Intel: 
sudo modprobe -r kvm_intel
sudo modprobe kvm_intel nested=1
```
```
# For AMD: 
sudo modprobe -r kvm_amd
sudo modprobe kvm_amd nested=1
```
However, you will need to run these commands each time you reboot, if you want to make it persistent, create `/etc/modprobe.d/kvm_intel.conf` and write:
```
options kvm_intel nested=1
```
You can verify that the nested virtualization is working with the command:
```
# For Intel:
systool -m kvm_intel -v | grep nested

# For AMD:
systool -m kvm_amd -v | grep nested
```

Which should return: `nested = "Y"`


### Snapshots
Snapshots save the current state of a VM for future use, if you are going to make a potentially destructive operation, it is recommended to create a snapshot before in order to revert to the previous state of the VM if something goes wrong.

To create a snapshot in virt-manager, open the window of the VM you want to create a snapshot and select the last button in the toolbar:
![2021-10-12-14:16:56](https://user-images.githubusercontent.com/34800654/136954616-3673bd5d-ec32-4bc3-94e6-d936db117517.png)

Now select the "+" button in order to create a snapshot:
![2021-10-12-14:15:57](https://user-images.githubusercontent.com/34800654/136954317-78c30f45-709f-48da-b083-b9cfebbe367f.png)

Finally, choose a name and write a description for your snapshot.
![2021-10-12-14:17:40](https://user-images.githubusercontent.com/34800654/136954625-b46a1401-5f8b-41c7-9039-7df5667a1846.png)

If you are user of VirtualBox... Good news! You can convert an existing VirtualBox `.vdi` snapshot format into a compatible `.qcow2` snapshot format that virt-manager uses. Use this command:
```
sudo qemu-img convert -f vdi -O qcow2 virtualboximagelocation targetlocation

# Example
sudo qemu-img convert -f vdi -O qcow2 PopOS\ 21.04.vdi /var/lib/libvirt/images/popos-21-04.qcow2
```

And that's it! If you followed correctly this short tutorial, you should be good to go and build your study labs.
