# Topic 330: Virtualization

## 330.1 Virtualization Concepts and Theory

**Weight: 8**

Description: Candidates should know and understand the general concepts, theory and terminology of Virtualization. This includes Xen, KVM and libvirt terminology.

Key Knowledge Areas:

- Terminology
- Pros and Cons of Virtualization
- Variations of Virtual Machine Monitors
- Migration of Physical to Virtual Machines
- Migration of Virtual Machines between Host systems
- Cloud Computing

The following is a partial list of the used files, terms and utilities:

- Hypervisor
- Hardware Virtual Machine (HVM)
- Paravirtualization (PV)
- Container Virtualization
- Emulation and Simulation
- CPU flags
- /proc/cpuinfo
- Migration (P2V, V2V)
- IaaS, PaaS, SaaS
---

### Virtualization Concepts

## 330.2 Xen

**Weight: 9**

Description: Candidates should be able to install, configure, maintain, migrate and troubleshoot Xen installations. The focus is on Xen version 4.x.

Key Knowledge Areas:

- Xen architecture, networking and storage
- Xen configuration
- Xen utilities
- Troubleshooting Xen installations
- Basic knowledge of XAPI
- Awareness of XenStore
- Awareness of Xen Boot Parameters
- Awareness of the xm utility

Terms and Utilities:

- Domain0 (Dom0), DomainU (DomU)
- PV-DomU, HVM-DomU
- /etc/xen/
- xl
- xl.cfg
- xl.conf
- xe
- xentop

---
## 330.3 KVM

**Weight: 9**

Description: Candidates should be able to install, configure, maintain, migrate and troubleshoot KVM installations.

Key Knowledge Areas:

- KVM architecture, networking and storage
- KVM configuration
- KVM utilities
- Troubleshooting KVM installations

Terms and Utilities:

- Kernel modules: kvm, kvm-intel and kvm-amd
- /etc/kvm/
- /dev/kvm
- kvm
- KVM monitor
- qemu
- qemu-img
---

### OpenSuse KVM installation

In order to install KVM with QEMU and libvirt on OpenSuse 15.1 you will need to install two patterns:

- **kvm_server** - Will install **KVM** and **QEMU** Tools
- **kvm_tools** - Will install **libvirt** tools like **virsh**

 ```SHELL
$ sudo zypper in -t pattern kvm_server kvm_tools
 ```

### CentOS 7 installation

With CentOS you can install individual packages or use package groups like following:

```SHELL
# yum groupinstall "Virtualization Tools" --optional
```

This command will install libvirt, qemu-kvm, qemu-img and qemu-kvm-tools (--optional).



## 330.4 Other Virtualization Solutions

**Weight: 3**

Description: Candidates should have some basic knowledge and experience with alternatives to Xen and KVM.

Key Knowledge Areas:

- Basic knowledge of OpenVZ and LXC
- Awareness of other virtualization technologies
- Basic knowledge of virtualization provisioning tools

Terms and Utilities:

- OpenVZ
- VirtualBox
- LXC
- docker
- packer
- vagrant


## 330.5 Libvirt and Related Tools

**Weight: 5**

Description: Candidates should have basic knowledge and experience with the libvirt library and commonly available tools.

Key Knowledge Areas:

- libvirt architecture, networking and storage
- Basic technical knowledge of libvirt and virsh
- Awareness of oVirt

Terms and Utilities:

- libvirtd
- /etc/libvirt/
- virsh
- oVirt
---

### OpenSuse/CentOS libvirt installations

Following the instructions in the previous chapter you should have libvirt installed. Otherwise install the pattern **kvm_tools** for OpenSuse or the **Virtualization Tools** for RedHat based systems.

### Managing libvirtd

```SH
$ sudo systemctl status libvirtd
$ sudo systemctl stop libvirtd
$ sudo systemctl start libvirtd
$ sudo systemctl enable libvirtd

```
 > WARNING: You should not run **libvirtd** and **xendomains** in parallel, they execute the same task and can interfere with one another.

 Using **virt-install** to install

 Options:
 - --name : vm name
 - --memory : amount of memory in MiB

 Guest storage:
 - --disk : storage configuration details, set to **none** for no disk space on vm
 - --filesystem : path of the file system for the vm

 Instalation method:
 - --location : location of instalation media
 - --cdrom : path for the ISO image, **CANNOT BE** a real CD/DVD-ROM device
 - --pxe : PXE Boot
 - --import : import a existing disk image
 - --boot : define boot sequence

 Creating a VM

 ```SHELL
 # virt-install --name guest-01 --memory 256Mib \
   --vcpus 1 --disk size=8 --cdrom=$PATH/OS.iso --os-variant rhel7
 ```


## 330.6 Cloud Management Tools

**Weight: 2**

Description: Candidates should have basic feature knowledge of commonly available cloud management tools.

Key Knowledge Areas:

- Basic feature knowledge of OpenStack and CloudStack
- Awareness of Eucalyptus and OpenNebula

Terms and Utilities:

- OpenStack
- CloudStack
- Eucalyptus
- OpenNebula
