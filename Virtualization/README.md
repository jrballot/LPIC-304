# Topic 330: Virtualization

## 330.1 Virtualization Concepts and Theory

Weight: 8

Description: Candidates should know and understand the general concepts, theory and terminology of Virtualization. This includes Xen, KVM and libvirt terminology.

Key Knowledge Areas:

    Terminology
    Pros and Cons of Virtualization
    Variations of Virtual Machine Monitors
    Migration of Physical to Virtual Machines
    Migration of Virtual Machines between Host systems
    Cloud Computing

The following is a partial list of the used files, terms and utilities:

    Hypervisor
    Hardware Virtual Machine (HVM)
    Paravirtualization (PV)
    Container Virtualization
    Emulation and Simulation
    CPU flags
    /proc/cpuinfo
    Migration (P2V, V2V)
    IaaS, PaaS, SaaS


## 330.2 Xen

Weight: 9

Description: Candidates should be able to install, configure, maintain, migrate and troubleshoot Xen installations. The focus is on Xen version 4.x.

Key Knowledge Areas:

    Xen architecture, networking and storage
    Xen configuration
    Xen utilities
    Troubleshooting Xen installations
    Basic knowledge of XAPI
    Awareness of XenStore
    Awareness of Xen Boot Parameters
    Awareness of the xm utility

Terms and Utilities:

    Domain0 (Dom0), DomainU (DomU)
    PV-DomU, HVM-DomU
    /etc/xen/
    xl
    xl.cfg
    xl.conf
    xe
    xentop


## 330.3 KVM

Weight: 9

Description: Candidates should be able to install, configure, maintain, migrate and troubleshoot KVM installations.

Key Knowledge Areas:

    KVM architecture, networking and storage
    KVM configuration
    KVM utilities
    Troubleshooting KVM installations

Terms and Utilities:

    Kernel modules: kvm, kvm-intel and kvm-amd
    /etc/kvm/
    /dev/kvm
    kvm
    KVM monitor
    qemu
    qemu-img


## 330.4 Other Virtualization Solutions

Weight: 3

Description: Candidates should have some basic knowledge and experience with alternatives to Xen and KVM.

Key Knowledge Areas:

    Basic knowledge of OpenVZ and LXC
    Awareness of other virtualization technologies
    Basic knowledge of virtualization provisioning tools

Terms and Utilities:

    OpenVZ
    VirtualBox
    LXC
    docker
    packer
    vagrant


## 330.5 Libvirt and Related Tools

Weight: 5

Description: Candidates should have basic knowledge and experience with the libvirt library and commonly available tools.

Key Knowledge Areas:

    libvirt architecture, networking and storage
    Basic technical knowledge of libvirt and virsh
    Awareness of oVirt

Terms and Utilities:

    libvirtd
    /etc/libvirt/
    virsh
    oVirt


## 330.6 Cloud Management Tools

Weight: 2

Description: Candidates should have basic feature knowledge of commonly available cloud management tools.

Key Knowledge Areas:

    Basic feature knowledge of OpenStack and CloudStack
    Awareness of Eucalyptus and OpenNebula

Terms and Utilities:

    OpenStack
    CloudStack
    Eucalyptus
    OpenNebula
