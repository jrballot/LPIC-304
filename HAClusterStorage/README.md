# Topic 335: High Availability Cluster Storage

## 335.1 DRBD / cLVM

Weight: 3

Description: Candidates are expected to have the experience and knowledge to install, configure, maintain and troubleshoot DRBD devices. This includes integration with Pacemaker. DRBD configuration of version 8.4.x is covered. Candidates are further expected to be able to manage LVM configuration within a shared storage cluster.

Key Knowledge Areas:

    Understanding of DRBD resources, states and replication modes
    Configuration of DRBD resources, networking, disks and devices
    Configuration of DRBD automatic recovery and error handling
    Management of DRBD using drbdadm
    Basic knowledge of drbdsetup and drbdmeta
    Integration of DRBD with Pacemaker
    cLVM
    Integration of cLVM with Pacemaker

Terms and Utilities:

    Protocol A, B and C
    Primary, Secondary
    Three-way replication
    drbd kernel module
    drbdadm
    drbdsetup
    drbdmeta
    /etc/drbd.conf
    /proc/drbd
    LVM2
    clvmd
    vgchange, vgs


## 335.2 Clustered File Systems

Weight: 3

Description: Candidates should know how to install, maintain and troubleshoot installations using GFS2 and OCFS2. This includes integration with Pacemaker as well as awareness of other clustered filesystems available in a Linux environment.

Key Knowledge Areas:

    Understand the principles of cluster file systems
    Create, maintain and troubleshoot GFS2 file systems in a cluster
    Create, maintain and troubleshoot OCFS2 file systems in a cluster
    Integration of GFS2 and OCFS2 with Pacemaker
    Awareness of the O2CB cluster stack
    Awareness of other commonly used clustered file systems

Terms and Utilities:

    Distributed Lock Manager (DLM)
    mkfs.gfs2
    mount.gfs2
    fsck.gfs2
    gfs2_grow
    gfs2_edit
    gfs2_jadd
    mkfs.ocfs2
    mount.ocfs2
    fsck.ocfs2
    tunefs.ocfs2
    mounted.ocfs2
    o2info
    o2image
    CephFS
    GlusterFS
    AFS
